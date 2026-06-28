# Custom Skills for Claude & AI Coding Assistants

This repository contains a collection of custom skills designed to enhance AI coding assistants (such as Claude Code) for academic writing, document formatting, and writing style humanization.

## Credits & Original Source

These skills were created and shared by **อาจารย์เจนนี่ (Ajarn Jennie)**.
- **Original Post:** [Ajarn Jennie - แจก Skill! ทางลัดของสายเอกสารและคนทำ Paper](https://www.facebook.com/share/p/17iouCyciY/)

---

## Index of Skills

1. [Academic Paper Writer (`academic-paper-writer.skill`)](#1-academic-paper-writer-academic-paper-writerskill) — Professional assistant for drafting and revising international research papers.
2. [Thai DOCX Formatting (`thai-docx.skill`)](#2-thai-docx-formatting-thai-docxskill) — Python tool for ensuring proper word-wrapping and line breaks in Thai Word documents.
3. [Writing Style Humanizer (`humanizer-main.zip`)](#3-writing-style-humanizer-humanizer-mainzip) — Style editor to eliminate AI writing tells and match natural human voices.

---

## Skill Explanations

### 1. Academic Paper Writer (`academic-paper-writer.skill`)
*   **Purpose:** Helps researchers turn raw data, rough ideas, or drafts into publication-ready academic manuscripts for international journals.
*   **Target Language:** English (for the paper content), with Thai used for discussion and explanations.
*   **Key Features:**
    *   **Anti-AI Vocabulary Filter:** Strictly bans flowery, overused AI words and phrases (e.g., *delve into, landscape, multifaceted, in the realm of, utilize, elucidate, plays a crucial role*).
    *   **Specific Claims:** Forces the AI to back up arguments with concrete numbers and specific studies instead of writing generic sentences like "previous studies have shown."
    *   **Structured Paper Templates:** Provides outlines and checklist rules for Empirical/R&D Papers and Systematic Reviews (adhering to PRISMA guidelines).
    *   **Fact-Checked References:** Default citation style is APA 7th Edition. The skill mandates using web search to find real, verifiable papers within the last 5 years and forbids hallucinating or fabricating references.
    *   **Quality Checklist:** Mentally audits drafts before final output to verify rhythm variation, specific evidence, and human-like writing tone.

### 2. Thai DOCX Formatting (`thai-docx.skill`)
*   **Purpose:** Resolves the common issue where Thai text wraps prematurely in Word documents, creating short lines and large, empty right margins.
*   **Target Language:** Thai and mixed Thai-English documents.
*   **Key Features:**
    *   **Zero-Width Space Injection:** Uses `pythainlp` in a Python script (`scripts/thai_docx.py`) to tokenize Thai words and insert Zero-Width Space characters (`U+200B`). These characters are invisible but instruct Microsoft Word's layout engine on where it is safe to break lines.
    *   **Left-Alignment Rule:** Enforces left alignment because justified layouts stretch Thai spacing and characters unnaturally.
    *   **Python Integration:** Relies on the Python `python-docx` library (avoiding JavaScript-based docx libraries which lack proper Thai wrapping support).
    *   **CLI & Import Modes:** Can be run as a command-line script (`python scripts/thai_docx.py input.txt -o output.docx`) or imported as helper functions `insert_zwsp()` and `create_docx()`.

### 3. Writing Style Humanizer (`humanizer-main.zip`)
*   **Purpose:** Edits and refines text to strip away all signatures of AI generation, restoring personality, flow, and natural phrasing. Based on Wikipedia's "Signs of AI writing" guide.
*   **Target Language:** Multilingual (primarily English/Thai text editing).
*   **Key Features:**
    *   **33 AI-Pattern Auditors:** Scans and eliminates typical AI writing habits including:
        *   Inflated symbolism and promotional language (*boasts a, vibrant, testament to, nestled in*).
        *   Superficial analyses ending in present participles (*highlighting, showcasing, fostering*).
        *   Structural patterns like outline-like "Challenges and Legacy" sections, boldface overuse, and bullet-point lists starting with bold inline-headers.
        *   Grammatical tells such as copula avoidance (replacing "is/are" with "serves as/stands as"), negative parallelisms (*not only... but also*), and sycophantic/servile tones (*Great question! Certainly!*).
    *   **Voice Calibration:** Can ingest a writing sample from the user to analyze sentence length variety, punctuation habits (e.g., dash usage), vocabulary register, and transition styles to match the user's authentic voice in the output.
    *   **Strict Em-Dash/En-Dash Ban:** Hard rule to replace all em dashes (`—`) and en dashes (`–`) with periods, commas, colons, or parentheses, removing one of the strongest statistical AI tells.

---

## How to Install and Use Custom Skills

To load these skills into your AI assistant workspace:

1. **Locate your custom skill folder:**
   * **Global path:** `~/.gemini/config/skills/` (for Gemini-based agents) or `~/.claude/config/skills/` (for Claude-based agents).
   * **Project path:** Create an `.agents/skills/` folder at the root of your project directory.
2. **Extract the skill package:**
   * Unzip the `.skill` or `.zip` file into the target skills folder.
   * *Example structure:*
     ```
     .agents/
     └── skills/
         ├── academic-paper-writer/
         │   └── SKILL.md
         ├── thai-docx/
         │   ├── SKILL.md
         │   └── scripts/
         │       └── thai_docx.py
         └── humanizer-main/
             ├── SKILL.md
             ├── AGENTS.md
             ├── LICENSE
             └── README.md
     ```
3. **Verify registration:**
   Your AI assistant will automatically discover the skills in standard directories when they are matched by the triggers specified in the YAML frontmatter of their respective `SKILL.md` files.
