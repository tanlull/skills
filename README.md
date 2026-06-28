# Claude Custom Skills

คอลเลกชัน Custom Skills สำหรับช่วยเหลืองานเอกสาร การทำ Paper วิชาการ และปรับแต่งสำนวนภาษาให้มีความเป็นธรรมชาติสำหรับ AI Coding Assistants (เช่น Claude Code)

> **Credits & Original Source:** 
> สร้างและเผยแพร่โดย **อาจารย์เจนนี่ (Ajarn Jennie)** 
> ติดตามโพสต์ต้นฉบับได้ที่: [แจก Skill! ทางลัดของสายเอกสารและคนทำ Paper](https://www.facebook.com/share/p/17iouCyciY/)

---

## รายชื่อและรายละเอียดของ Skills

| ชื่อ Skill | สรุปย่อ | เอกสารรายละเอียด |
| :--- | :--- | :--- |
| **Academic Paper Writer**<br>`academic-paper-writer.skill` | ผู้ช่วยเขียนและปรับปรุงบทความวิชาการ (Research Paper) เป็นภาษาอังกฤษเชิงวิชาการที่กระชับ เลี่ยงคำศัพท์ฟุ่มเฟือยที่เป็นจุดสังเกตของ AI และมีระบบตรวจสอบการอ้างอิงตามมาตรฐาน APA 7th Edition | [อ่านรายละเอียด](docs/academic-paper-writer.md) |
| **Thai DOCX Formatting**<br>`thai-docx.skill` | แก้ปัญหาการตัดคำภาษาไทยในไฟล์ Word (`.docx`) โดยการใช้ `pythainlp` แทรก Zero-Width Space (`\u200b`) ระหว่างคำไทยเพื่อระบุจุดตัดบรรทัดที่ถูกต้อง ทำให้ตัวอักษรพิมพ์เต็มบรรทัดสวยงาม | [อ่านรายละเอียด](docs/thai-docx.md) |
| **Writing Style Humanizer**<br>`humanizer-main.zip` | ปรับแต่งสไตล์การเขียนเพื่อลบล้างความเป็น AI (AI Tells) กว่า 33 รูปแบบตามแนวทางของ Wikipedia สามารถเลียนแบบน้ำเสียงผู้ใช้ (Voice Calibration) และลบสัญลักษณ์อย่าง Em Dash ออกทั้งหมด | [อ่านรายละเอียด](docs/humanizer.md) |

---

## วิธีติดตั้งและใช้งานเบื้องต้น

1. นำไฟล์ `SKILL.md` (และโครงสร้างโฟลเดอร์ของแต่ละ Skill) ไปใส่ไว้ในโฟลเดอร์ Custom Skills ของ AI Assistant:
   * **สำหรับระดับ Project:** วางไว้ที่ `.agents/skills/` (เช่น `.agents/skills/thai-docx/SKILL.md`)
   * **สำหรับระดับ Global:** วางไว้ที่ `~/.gemini/config/skills/` (Gemini) หรือ `~/.claude/config/skills/` (Claude)
2. เมื่อพิมพ์คำสั่งหรือทำงานที่ตรงกับ Trigger ใน YAML Frontmatter ของ Skill นั้นๆ AI จะดึงคำสั่งไปใช้งานโดยอัตโนมัติ
