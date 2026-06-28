# Claude Custom Skills

คอลเลกชัน Custom Skills สำหรับช่วยเหลืองานเอกสาร การทำ Paper วิชาการ และปรับแต่งสำนวนภาษาให้มีความเป็นธรรมชาติสำหรับ AI Coding Assistants (เช่น Claude Code)

> **Credits & Original Source:** 
> สร้างและเผยแพร่โดย **อาจารย์เจนนี่ (Ajarn Jennie)** 
> ติดตามโพสต์ต้นฉบับได้ที่: [แจก Skill! ทางลัดของสายเอกสารและคนทำ Paper](https://www.facebook.com/share/p/17iouCyciY/)

---

## รายชื่อและรายละเอียดของ Skills

### 1. Academic Paper Writer (`academic-paper-writer.skill`)
* **สรุปย่อ:** ผู้ช่วยเขียนและปรับปรุงบทความวิชาการ (Research Paper/Manuscript) เพื่อเตรียมส่งตีพิมพ์ในระดับนานาชาติ เน้นเขียนด้วยภาษาอังกฤษเชิงวิชาการที่กระชับ ตรงประเด็น เลี่ยงคำศัพท์ฟุ่มเฟือยที่เป็นจุดสังเกตของ AI (เช่น *delve, landscape, multifaceted*) และระบบตรวจสอบการอ้างอิงตามมาตรฐาน APA 7th Edition
* **ข้อมูลเชิงลึก:** [ดูรายละเอียดเพิ่มเติมใน docs/academic-paper-writer.md](docs/academic-paper-writer.md)

### 2. Thai DOCX Formatting (`thai-docx.skill`)
* **สรุปย่อ:** แก้ไขปัญหาการตัดบรรทัดภาษาไทยในเอกสาร Word (`.docx`) โดยระบบจะใช้ `pythainlp` บน Python ในการตัดคำแล้วแทรกตัวอักษรล่องหน Zero-Width Space (ZWS, `\u200b`) ระหว่างคำ เพื่อบอกให้ Microsoft Word รู้จุดตัดบรรทัดที่ถูกต้อง ทำให้ตัวอักษรพิมพ์เต็มบรรทัดสวยงาม ไม่ตัดขึ้นบรรทัดใหม่ก่อนเวลาอันควร
* **ข้อมูลเชิงลึก:** [ดูรายละเอียดเพิ่มเติมใน docs/thai-docx.md](docs/thai-docx.md)

### 3. Writing Style Humanizer (`humanizer-main.zip`)
* **สรุปย่อ:** สเกลาร์ปรับแต่งสไตล์การเขียนเพื่อลบล้างความเป็น AI (AI Tells) กว่า 33 รูปแบบ อ้างอิงตามแนวทาง "Signs of AI writing" ของ Wikipedia ช่วยเกลี่ยโครงสร้างประโยคให้นุ่มนวล เป็นธรรมชาติ มีชีวิตชีวา และสามารถป้อนตัวอย่างเขียนของตัวเองเพื่อถอดรหัสและเลียนแบบน้ำเสียงผู้ใช้ (Voice Calibration) ได้
* **ข้อมูลเชิงลึก:** [ดูรายละเอียดเพิ่มเติมใน docs/humanizer.md](docs/humanizer.md)

---

## วิธีติดตั้งและใช้งานเบื้องต้น

1. นำไฟล์ `SKILL.md` (และโครงสร้างโฟลเดอร์ของแต่ละ Skill) ไปใส่ไว้ในโฟลเดอร์ Custom Skills ของ AI Assistant:
   * **สำหรับระดับ Project:** วางไว้ที่ `.agents/skills/` (เช่น `.agents/skills/thai-docx/SKILL.md`)
   * **สำหรับระดับ Global:** วางไว้ที่ `~/.gemini/config/skills/` (Gemini) หรือ `~/.claude/config/skills/` (Claude)
2. เมื่อพิมพ์คำสั่งหรือทำงานที่ตรงกับ Trigger ใน YAML Frontmatter ของ Skill นั้นๆ AI จะดึงคำสั่งไปใช้งานโดยอัตโนมัติ
