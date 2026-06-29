---
name: compile-cpp-dll
description: Build C/C++ source, headers, Visual Studio solutions, .vcxproj projects, or CMake projects into Windows DLL artifacts. Use when the user asks to compile .cpp/.h/.sln/.vcxproj/CMakeLists.txt into a .dll, create an MQL4/MQL5/MetaTrader DLL, cross-compile a Windows DLL from macOS or Linux, verify exported DLL functions, inspect DLL dependencies, or prepare reusable build commands across operating systems.
---

# Compile C++ DLL

## Workflow

1. Locate the build target:
   - Prefer an explicitly named `.sln`, `.vcxproj`, or `CMakeLists.txt`.
   - If only `.cpp`/`.h` files are present, compile the source files directly.
   - If several candidates exist, choose the one nearest the user's requested path and state the assumption.

2. Choose the build path:
   - On Windows with Visual Studio Build Tools available, prefer `MSBuild.exe` for `.sln` or `.vcxproj`.
   - For CMake projects, prefer `cmake --build` if the project already defines a shared library target.
   - On macOS or Linux when the output must be a Windows `.dll`, use MinGW-w64 (`x86_64-w64-mingw32-g++` for x64, `i686-w64-mingw32-g++` for x86).
   - For MetaTrader 5, build x64 unless the user explicitly asks for 32-bit. For MetaTrader 4, ask or infer x86 only when the user's environment requires it.

3. Build in a separate output directory. Do not overwrite existing Visual Studio `Debug/`, `Release/`, or `x64/Release/` outputs unless the user asks.

4. Verify the artifact:
   - Confirm the file is a Windows DLL (`PE32+` for x64, `PE32` for x86).
   - Inspect exports with `objdump -p`, `llvm-objdump -p`, `dumpbin /exports`, or the script output.
   - If Wine is available and a simple exported function can be called safely, run a smoke test.
   - Report the DLL path, architecture, exported function names, dependency notes, and any warnings.

## Helper Script

Use `scripts/compile_cpp_dll.py` for repeatable builds, especially outside Windows or when MSBuild is missing.

Typical commands:

```bash
python3 ~/.codex/skills/compile-cpp-dll/scripts/compile_cpp_dll.py /path/to/project --arch x64 --config Release
python3 ~/.codex/skills/compile-cpp-dll/scripts/compile_cpp_dll.py /path/to/project/project.vcxproj --name mylib --out-dir /tmp/mylib-build
python3 ~/.codex/skills/compile-cpp-dll/scripts/compile_cpp_dll.py /path/to/src --sources a.cpp b.cpp --lib winhttp --lib ws2_32
```

Useful options:

- `--build-system auto|msbuild|cmake|mingw|cl`
- `--arch x64|x86`
- `--config Release|Debug`
- `--name NAME` for the output DLL base name
- `--source FILE`, repeatable, to override source discovery
- `--include DIR`, `--define NAME[=VALUE]`, `--lib NAME`, `--lib-path DIR`
- `--extra-cxxflag FLAG`, `--extra-ldflag FLAG`
- `--no-static-runtime` if a smaller MinGW DLL is preferred and runtime DLL dependencies are acceptable

The script reads `.vcxproj` source lists and common preprocessor definitions when possible. For MinGW builds, it also detects common Windows libraries such as `winhttp`, `wininet`, `ws2_32`, `crypt32`, `advapi32`, and `user32` from includes/API usage.

## Toolchain Notes

Install one suitable toolchain on the current OS:

- Windows: Visual Studio Build Tools with C++ workload; run from a Developer Command Prompt when using `cl`.
- macOS: `brew install mingw-w64 cmake`.
- Debian/Ubuntu: `sudo apt-get install mingw-w64 cmake`.
- Fedora: `sudo dnf install mingw64-gcc-c++ mingw32-gcc-c++ cmake`.

MinGW import libraries (`.a`) are for other MinGW C/C++ programs linking against the DLL. MetaTrader normally needs only the `.dll`.

## MetaTrader DLL Checks

For MQL imports, prefer C ABI exports:

```cpp
extern "C" __declspec(dllexport) int __stdcall MyFunction(...);
```

Avoid C++ classes, references, STL types, or exceptions across the DLL boundary. Use simple C-compatible types (`int`, `double`, `wchar_t*`, buffers plus sizes). For MT5, ensure the EA enables DLL imports and copy the finished DLL to the terminal's `MQL5/Libraries` folder when the user wants deployment.
