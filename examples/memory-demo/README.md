# Memory Demo — Agent ที่มีความจำและบริบท

> 🚧 ตัวอย่างนี้ยังไม่มีโค้ด — กำลังเตรียมการ

## แนวคิด

ตัวอย่างนี้จะแสดง Agent ที่มีระบบความจำ:

- **Short-term Memory**: จดจำประวัติการสนทนาภายใน session
- **Long-term Memory**: จำข้อมูลผู้ใช้และโปรเจกต์ข้าม session
- **Working Memory**: จดจำสถานะของงานที่กำลังทำ
- **RAG**: ค้นหาข้อมูลที่เกี่ยวข้องจากฐานความรู้

## สิ่งที่จะได้เรียนรู้

- ความแตกต่างของ memory แต่ละประเภท
- การเก็บและเรียกค้นข้อมูลใน Vector Store
- การจัดการ Context Window
- เทคนิคการ Summarize ประวัติเก่า
- การออกแบบระบบ Memory ที่มีประสิทธิภาพ

## โครงสร้างที่วางแผน

```
memory-demo/
├── main.py              # Agent พร้อมระบบความจำ
├── memory/
│   ├── short_term.py    # In-memory history
│   ├── long_term.py     # ฐานข้อมูล + vector store
│   └── working.py       # Session state
├── rag/
│   ├── embedder.py      # สร้าง embeddings
│   └── retriever.py     # ค้นหาข้อมูล
├── store/               # ข้อมูลที่เก็บ (SQLite, vector)
└── requirements.txt
```

## เทคโนโลยีที่วางแผน

- Python
- SQLite สำหรับ long-term memory
- Chroma หรือ FAISS สำหรับ vector store
- Sentence Transformers สำหรับ embeddings

## เป้าหมาย

แสดงให้เห็นว่า Agent สามารถจำและใช้บริบทจากอดีตมาปรับปรุงการทำงานได้อย่างไร
