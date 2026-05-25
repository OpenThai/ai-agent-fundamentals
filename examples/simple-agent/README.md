# Simple Agent — Minimal Agent Loop

> 🚧 ตัวอย่างนี้ยังไม่มีโค้ด — กำลังเตรียมการ

## แนวคิด

ตัวอย่างนี้จะแสดง Agent พื้นฐานที่สุดที่มีแค่วงวน:

1. รับ input
2. LLM คิด (thought)
3. LLM ตัดสินใจว่าจะใช้เครื่องมืออะไร (action)
4. รันเครื่องมือ (tool execution)
5. ส่งผลลัพธ์กลับให้ LLM (observation)
6. วนซ้ำจนได้คำตอบ

## สิ่งที่จะได้เรียนรู้

- Agent Loop พื้นฐานคืออะไร
- LLM เป็น "สมอง" ที่ตัดสินใจเลือก action อย่างไร
- Tool result ถูกส่งกลับให้ LLM อย่างไร
- เงื่อนไขการหยุดทำงานของ Agent

## โครงสร้างที่วางแผน

```
simple-agent/
├── main.py          # วงวน Agent พื้นฐาน
├── tools.py         # ลงทะเบียนเครื่องมือ
├── agent.py         # Logic การเรียก LLM และจัดการ response
└── requirements.txt
```

## เทคโนโลยีที่วางแผน

- Python
- OpenAI API / Anthropic API (Function Calling)
- ไม่ใช้ framework — สร้าง Agent loop เอง

## เป้าหมาย

ทำให้เห็นว่า Agent ไม่ได้ซับซ่อน — แค่ loop ที่ LLM ＋ Tools ＋ Memory
