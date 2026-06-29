# Claude Custom Skills

คอลเลกชัน Custom Skills สำหรับช่วยเหลืองานเอกสาร การทำ Paper วิชาการ และปรับแต่งสำนวนภาษาให้มีความเป็นธรรมชาติสำหรับ AI Coding Assistants (เช่น Claude Code)

---

## รายชื่อและรายละเอียดของ Skills

| ชื่อ Skill | สรุปย่อ | ผู้สร้าง / แหล่งที่มา | เอกสารรายละเอียด |
| :--- | :--- | :--- | :--- |
| **Academic Paper Writer**<br>[`academic-paper-writer.skill`](academic-paper-writer.skill) | ผู้ช่วยเขียนและปรับปรุงบทความวิชาการ (Research Paper) เป็นภาษาอังกฤษเชิงวิชาการที่กระชับ เลี่ยงคำศัพท์ฟุ่มเฟือยที่เป็นจุดสังเกตของ AI และมีระบบตรวจสอบการอ้างอิงตามมาตรฐาน APA 7th Edition | [อาจารย์เจนนี่ (Ajarn Jennie)](https://www.facebook.com/share/p/17iouCyciY/) | [อ่านรายละเอียด](docs/academic-paper-writer.md) |
| **Compile C++ DLL**<br>[`compile-cpp-dll.skill`](compile-cpp-dll.skill) | คอมไพล์ซอร์สโค้ด C/C++ หรือโปรเจกต์ Visual Studio / CMake ให้เป็นไฟล์ Windows DLL (`.dll`) บนทุกระบบปฏิบัติการ (รวมถึง Cross-compile จาก macOS/Linux ผ่าน MinGW) พร้อมตรวจสอบฟังก์ชันที่ส่งออก (Exported Functions) | [Tanya S. (tanlull)](https://github.com/tanlull) | [อ่านรายละเอียด](docs/compile-cpp-dll.md) |
| **macOS MQL5 Compiler**<br>[`macos-mq5-compile.skill`](macos-mq5-compile.skill) | คอมไพล์ไฟล์ MQL5 (`.mq5`) เป็นบอทเทรด/อินดิเคเตอร์ (`.ex5`) แบบ Headless บนเครื่อง macOS ผ่าน Wine + MetaEditor พร้อมตรวจสอบและแสดงรายงานข้อผิดพลาด/คำเตือน | [Tanya S. (tanlull)](https://github.com/tanlull) | [อ่านรายละเอียด](docs/macos-mq5-compile.md) |
| **Thai DOCX Formatting**<br>[`thai-docx.skill`](thai-docx.skill) | แก้ปัญหาการตัดคำภาษาไทยในไฟล์ Word (`.docx`) โดยการใช้ `pythainlp` แทรก Zero-Width Space (`\u200b`) ระหว่างคำไทยเพื่อระบุจุดตัดบรรทัดที่ถูกต้อง ทำให้ตัวอักษรพิมพ์เต็มบรรทัดสวยงาม | [อาจารย์เจนนี่ (Ajarn Jennie)](https://www.facebook.com/share/p/17iouCyciY/) | [อ่านรายละเอียด](docs/thai-docx.md) |
| **Thai Slide Fonts**<br>[`thai-slide-fonts.skill`](thai-slide-fonts.skill) | ปรับแต่งและแก้ไขฟอนต์ภาษาไทยในสไลด์ PowerPoint (`.pptx`) ให้แสดงผลถูกต้องและอ่านง่าย โดยแก้ไขช่องสล็อต `a:cs` (Complex Script) เพื่อไม่ให้ฟอนต์เพี้ยน พร้อมระบบย่อ/ขยายขนาดฟอนต์ของสไลด์เดิมหรือสไลด์ใหม่ให้อ่านได้ชัดเจนบนเครื่องฉายโปรเจคเตอร์ | [อาจารย์เจนนี่ (Ajarn Jennie)](https://www.facebook.com/share/p/17iouCyciY/) | [อ่านรายละเอียด](docs/thai-slide-fonts.md) |
| **Writing Style Humanizer**<br>[`humanizer-main.zip`](humanizer-main.zip) | ปรับแต่งสไตล์การเขียนเพื่อลบล้างความเป็น AI (AI Tells) กว่า 33 รูปแบบตามแนวทางของ Wikipedia สามารถเลียนแบบน้ำเสียงผู้ใช้ (Voice Calibration) และลบสัญลักษณ์อย่าง Em Dash ออกทั้งหมด | [อาจารย์เจนนี่ (Ajarn Jennie)](https://www.facebook.com/share/p/17iouCyciY/) | [อ่านรายละเอียด](docs/humanizer.md) |

---

## วิธีติดตั้งและใช้งานเบื้องต้น

1. นำไฟล์ `SKILL.md` (และโครงสร้างโฟลเดอร์ของแต่ละ Skill) ไปใส่ไว้ในโฟลเดอร์ Custom Skills ของ AI Assistant:
   * **สำหรับระดับ Project:** วางไว้ที่ `.agents/skills/` (เช่น `.agents/skills/thai-docx/SKILL.md`)
   * **สำหรับระดับ Global:** วางไว้ที่ `~/.gemini/config/skills/` (Gemini) หรือ `~/.claude/config/skills/` (Claude)
2. เมื่อพิมพ์คำสั่งหรือทำงานที่ตรงกับ Trigger ใน YAML Frontmatter ของ Skill นั้นๆ AI จะดึงคำสั่งไปใช้งานโดยอัตโนมัติ
