---
name: macos-mq5-compile
description: Compile MQL5 .mq5 Expert Advisors / indicators / scripts headlessly on THIS macOS machine via wine + MetaEditor, returning the error/warning report. Use this skill whenever the user asks to compile, build, check, or "make the .ex5" for an MQL5 file (e.g. "compile Rebirth_V01.mq5", "build the EA", "does this compile?", "check for errors", "ลองคอมไพล์ให้หน่อย"), or hands over a raw `wine .../MetaEditor64.exe /compile:...` command to run. It encodes the exact wine binary, the working MetaTrader prefix, and the polling/log-decoding steps that keep the compile from hanging silently. Do NOT use this for editing/writing MQL5 logic (that's the EA-development skills) — only for the act of compiling. Project CLAUDE.md says "never compile" by default, so only invoke this when the user explicitly asks for a compile.
---

# Compile MQL5 on macOS (wine + MetaEditor)

Compile a `.mq5` and report the result. The hard part is already solved — the
naive command hangs silently. Just run the bundled script:

```bash
bash .agents/skills/macos-mq5-compile/scripts/compile_mq5.sh 90_Working/Rebirth_V01.mq5
```

The path may be absolute or relative to the current directory. The script:
1. Launches MetaEditor with the **native** MetaTrader prefix and the correct wine binary.
2. Polls the (UTF-16) log until the `Result:` line appears, then stops MetaEditor.
3. Prints any errors/warnings and the `Result:` line, and confirms the `.ex5`.

Exit code: `0` = compiled, 0 errors · `1` = compile errors · `2` = setup/timeout.

On success you'll see something like:
```
Result: 0 errors, 0 warnings, 1224 ms elapsed, cpu='X64 Regular'
OK -> 90_Working/Rebirth_V01.ex5
```

## Why not just call wine directly

Two traps make the obvious command hang forever with no output (no `.ex5`, no log):

- **`wine64` does not exist** — the binary is named `wine`. The wrong name fails
  with `exit 127`, which is easy to miss inside a long one-liner.
- **Wrong wineprefix.** Pointing `WINEPREFIX` at the CrossOver bottle
  (`~/Library/Application Support/CrossOver/Bottles/MetaTrader 5`) while running
  it with MetaTrader.app's wine mixes two different wine builds. Every launch then
  tries to reconfigure the bottle (a `wineboot` + `setupapi InstallHinfSection`
  pass) and **deadlocks at ~1.2s CPU**. Launching it repeatedly just piles up
  more wedged prefix-updates. The fix is to use the **native** prefix
  `net.metaquotes.wine.metatrader5`, which already has MetaEditor, the `Include`
  folder, `z: -> /`, and a warm wineserver.

A third, subtler trap: MetaEditor **does not exit** after compiling, and writes
the log incrementally. If you kill it the moment the log file appears, you cut it
off mid-"generating code" and get a truncated log with no `Result:`. The script
waits for the `Result:` marker before stopping it — keep that behavior if you ever
inline the command.

## If you must run it inline (no script)

```bash
WINEPREFIX="/Users/tan/Library/Application Support/net.metaquotes.wine.metatrader5" \
WINEDEBUG=-all \
"/Applications/MetaTrader 5.app/Contents/SharedSupport/wine/bin/wine" \
"C:/Program Files/MetaTrader 5/MetaEditor64.exe" \
/compile:"Z:\Users\tan\git\MQL5-EA\90_Working\Rebirth_V01.mq5" \
/log:"Z:\Users\tan\git\MQL5-EA\90_Working\Rebirth_V01.compile.log"
# wait for the Result line, then: pkill -9 -f MetaEditor64.exe
# decode the log: iconv -f UTF-16LE -t UTF-8 <file>.compile.log
```

`z:` maps to `/`, so a macOS path `/Users/...` becomes `Z:\Users\...` (forward
slashes flipped to backslashes). The same conversion applies to the `/log:` path.

## Notes

- Running MetaEditor alongside the user's live trading terminal is safe — they
  share the native prefix and coexist normally; compiling does not touch trading.
- Per project CLAUDE.md the default is to let the user compile manually. Only run
  this when the user explicitly asks to compile.
- If a run times out on the very first compile after a reboot, retry once — the
  shared wineserver may still be warming up.
