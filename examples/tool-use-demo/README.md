# Tool-use Demo — Agent ที่ใช้เครื่องมืออย่างปลอดภัย

> 🚧 ตัวอย่างนี้ยังไม่มีโค้ด — กำลังเตรียมการ

## แนวคิด

ตัวอย่างนี้จะแสดง Agent ที่สามารถเรียกใช้เครื่องมือภายนอกได้:

- ค้นหาข้อมูลจากเว็บ
- อ่านเนื้อหาจาก URL
- คำนวณตัวเลข
- จัดการไฟล์พื้นฐาน

พร้อมเน้นเรื่อง **ความปลอดภัยในการใช้เครื่องมือ**

## สิ่งที่จะได้เรียนรู้

- การลงทะเบียน Tool Schema
- การให้ LLM เลือก tool ที่เหมาะสม
- การจัดการ Tool Call Request และ Response
- Error handling เมื่อ tool ล้มเหลว
- การจำกัดสิทธิ์และอนุมัติการเรียกใช้ tool

## โครงสร้างที่วางแผน

```
tool-use-demo/
├── main.py           # ตัวอย่าง Agent ใช้ tools
├── tools/
│   ├── web_search.py
│   ├── calculator.py
│   └── file_reader.py
├── safety.py         # Permission และ validation
├── audit.py          # บันทึกทุก tool call
└── requirements.txt
```

## แนวทางความปลอดภัย

- ทุก tool call ต้อง validate parameters ก่อน
- Tool ที่เสี่ยงต้องขอ human approval
- จำกัดการใช้ tool ต่อ session
- มี audit log ทุกครั้งที่เรียกใช้

## เป้าหมาย

แสดง pattern การใช้ tools ของ Agent ที่ปลอดภัยและตรวจสอบได้
