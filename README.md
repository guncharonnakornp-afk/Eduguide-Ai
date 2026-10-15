# EduGuide AI

แพลตฟอร์ม AI แนะแนวการศึกษาและอาชีพสำหรับนักเรียนไทย + Dashboard สำหรับครูแนะแนว
โปรเจกต์สำหรับ **JUMP THAILAND Hackathon 2026**

> เปลี่ยนการแนะแนวจาก “ถามทีละคน” เป็น “เข้าใจทั้งห้อง”

## ฟีเจอร์ (ใช้งานได้จริง)

| ส่วน | รายละเอียด |
|---|---|
| แบบประเมิน 4 ขั้น | ข้อมูลพื้นฐาน → ความสนใจ → ความถนัด → GPAX/งบ/ทุน (+ ยืนยันเบอร์ด้วย OTP เดโม) |
| AI Recommendation | จับคู่ 15 กลุ่มสาขาแบบ rule-based ถ่วงน้ำหนัก ความสนใจ 50% · ความถนัด 25% · GPAX 15% · งบ 10% (+โบนัสเป้าหมายอาชีพ) อธิบายเหตุผลได้ทุกข้อ |
| ผลลัพธ์ | 3 คณะที่เหมาะ + อาชีพ + Skill Gap + ทุน + แผน 90 วัน + คำถามไปคุยกับครู |
| AI Chat | ตอบเรื่อง TCAS ทุน เปรียบเทียบคณะ แผนเตรียมตัว และข้อมูลแต่ละสาขา (ไม่เรียก API ภายนอก) |
| Dashboard ครู | KPI รายห้อง · กลุ่มความสนใจ · รายชื่อติดตามเรียงตามความเสี่ยง · Export CSV · พิมพ์ PDF |
| ส่งผลให้ครู | นักเรียนคัดลอก “รหัส EG1:…” หรือลิงก์ให้ครู ครูวางใน Dashboard (ไม่ต้องมีเซิร์ฟเวอร์ ไม่ส่งเบอร์โทร) |

## รันบนเครื่อง

ไม่ต้อง build เปิด `index.html` ได้เลย หรือ

```bash
npx serve .        # หรือ python3 -m http.server 8000
```

## Deploy ขึ้น GitHub Pages

1. Push โค้ดทั้งหมดขึ้น branch `main`
2. GitHub → **Settings → Pages → Source: GitHub Actions**
3. ระบบจะ deploy อัตโนมัติด้วย `.github/workflows/pages.yml`
4. URL เว็บ: `https://<username>.github.io/<repo>/`

## ทดสอบ

```bash
node tests/engine.test.js                 # unit test ของ engine
npm i playwright && node tests/e2e.js     # ทดสอบเบราว์เซอร์ครบ flow
```

## โครงสร้าง

```
index.html        หน้าเดียว (hash routing)
assets/style.css  ธีมสว่าง/มืด, responsive, print stylesheet
js/data.js        ข้อมูลสาขา คำถาม ทุน (แก้/เพิ่มสาขาได้ที่นี่)
js/engine.js      Recommendation engine + risk flag + รหัสส่งครู
js/app.js         UI, แชต, Dashboard
tests/            unit + e2e
```

## ข้อจำกัดและแผนต่อยอด (ตรงตามสถาปัตยกรรมในสไลด์)

- **ข้อมูลเก็บในเบราว์เซอร์ (localStorage)** ครูเห็นเฉพาะข้อมูลที่นำเข้าในเครื่องตัวเอง ระยะต่อไปเพิ่ม Backend (Node/Python + ฐานข้อมูล) เพื่อให้นักเรียนส่งตรงเข้าห้องของครู
- **PIN ครู 1234 เป็นเดโม** ของจริงต้องมีระบบล็อกอินฝั่งเซิร์ฟเวอร์
- **OTP เป็น mock** ที่ `AuthProvider` ใน `js/app.js` ให้เปลี่ยนเป็นการเรียก **AIS API (OTP / SMS / Number Verification)** ผ่าน backend (ห้ามใส่ API key ใน front-end)
- **ชั้น AI เป็น rule-based** อธิบายได้และทำงานออฟไลน์ ต่อยอดเป็น LLM ได้ที่ฟังก์ชัน `answer()` และเพิ่มการตรวจสอบโดยครู (human-in-the-loop)
- ข้อมูลเกณฑ์/ทุนเป็นค่าประมาณระดับกลุ่มสาขา ต้องตรวจสอบประกาศล่าสุดที่ mytcas.com และมหาวิทยาลัย
- ข้อมูลนักเรียนเป็นข้อมูลส่วนบุคคล เมื่อมี backend ต้องทำตาม PDPA (ขอความยินยอม, จำกัดสิทธิ์ครู, ลบข้อมูลได้)

## Tech

HTML · CSS · Vanilla JS (ไม่มี dependency ตอนรัน)

## ทีม

แก้ชื่อทีมและสมาชิกที่นี่ก่อนส่ง
