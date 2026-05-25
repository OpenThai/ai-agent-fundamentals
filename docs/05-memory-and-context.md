# บทที่ 5: Memory และ Context

---

Agent ที่ไม่มีหน่วยความจำก็เหมือนคนที่ลืมทุกอย่างหลังจากพูดจบประโยค — ทำงานต่อเนื่องไม่ได้

Memory และ Context คือสิ่งที่ทำให้ Agent "จำ" และ "เข้าใจบริบท" ได้

---

## Memory vs Context

| | Memory | Context |
|---|--------|---------|
| **คืออะไร** | ระบบเก็บและเรียกค้นข้อมูล | ข้อมูลที่ Agent มีอยู่ตอนทำงาน |
| **อายุ** | อยู่ข้าม session | อยู่ภายใน session ปัจจุบัน |
| **รูปแบบ** | ฐานข้อมูล, vector store, files | ประวัติสนทนา, system prompt, tool results |
| **จัดการโดย** | Memory Manager | Runtime / Prompt Builder |

---

## ประเภทของ Memory

### 1. Short-term Memory (STM)

จำภายใน session เดียวกัน — เหมือนความจำระยะสั้นของมนุษย์

- **เก็บ**: ประวัติสนทนา, tool results ล่าสุด, state ปัจจุบัน
- **ข้อจำกัด**: Context Window — จำกัดด้วย token limit
- **แก้ปัญหา**: Token management, summarization, sliding window

### 2. Long-term Memory (LTM)

จำข้าม session — เหมือนความจำระยะยาวของมนุษย์

- **เก็บ**: ข้อมูลผู้ใช้, โปรเจกต์, การตั้งค่า, ความรู้ที่สะสม
- **รูปแบบ**: ฐานข้อมูล (SQLite, Postgres), Vector Store (Chroma, Pinecone), Files
- **การเรียกค้น**: RAG, keyword search, metadata filter

### 3. Working Memory

ข้อมูลที่ Agent กำลังทำงานอยู่ตอนนี้ — คล้าย RAM

- **เก็บ**: Task state, intermediate results, plan ที่กำลังทำ
- **ลบทิ้ง**: เมื่องานนั้นเสร็จ

### 4. Episodic Memory

จำเหตุการณ์ที่เกิดขึ้น — Agent จำได้ว่า "ครั้งที่แล้วทำแบบนี้แล้วล้มเหลว"

- **ใช้**: Reflection, learning จากประสบการณ์
- **ประโยชน์**: ปรับปรุงพฤติกรรม ไม่ผิดซ้ำ

---

## Context Engineering

การสร้าง Context ที่ดีเป็นศิลปะ:

### องค์ประกอบของ Context

```
System Prompt (ตัวตน + กฎ)
├─ Role: "คุณคือ Research Agent..."
├─ Tools: รายการเครื่องมือที่มี
├─ Rules: "ห้าม delete ข้อมูล", "ต้องขออนุมัติก่อนส่งอีเมล"
└─ Style: "ตอบเป็นภาษาไทย กระชับ มีแหล่งอ้างอิง"

Working Context (ข้อมูล session)
├─ ประวัติสนทนาล่าสุด (sliding window)
├─ Tool Results (ที่เพิ่งได้มา)
├─ Current Plan (แผนที่กำลังทำ)
└─ State Flags (กำลังรออนุมัติ, error, ฯลฯ)

Retrieved Context (จาก long-term memory)
├─ ข้อมูลผู้ใช้
├─ ข้อมูลโปรเจกต์
├─ ความรู้เฉพาะด้าน (ผ่าน RAG)
└─ episodic memory ที่เกี่ยวข้อง
```

---

## RAG (Retrieval-Augmented Generation)

RAG คือวิธีการค้นหาข้อมูลที่เกี่ยวข้องจากฐานความรู้ แล้วใส่เข้าไปใน context

### ขั้นตอน

1. **Index** — แปลงเอกสารเป็น embeddings เก็บใน vector store
2. **Query** — เมื่อมีคำถาม, search vector store
3. **Retrieve** — ดึงเอกสารที่เกี่ยวข้องมากที่สุด
4. **Augment** — ใส่เอกสารเหล่านั้นลงใน prompt
5. **Generate** — LLM ตอบโดยใช้ข้อมูลที่ค้นมา

### RAG vs Fine-tuning

| | RAG | Fine-tuning |
|---|-----|-------------|
| ข้อมูล | dynamic (เปลี่ยนได้ตลอด) | static (ต้อง train ใหม่) |
| Cost | ต่ำต่อ query | สูง (train + hosting) |
| Accuracy | ขึ้นกับ retrieval quality | ขึ้นกับ training data |
| ใช้กับ | ข้อมูลปัจจุบัน, เอกสารบริษัท | รูปแบบการตอบ, domain knowledge |
| อัปเดต | เพิ่มเอกสารใน store | train ใหม่ทั้งหมด |
| ความโปร่งใส | บอกแหล่งอ้างอิงได้ | black box |

---

## การจัดการ Context Window

Context Window มีจำกัด — ต้องบริหารอย่างชาญฉลาด:

| เทคนิค | วิธี |
|--------|------|
| **Sliding Window** | เก็บเฉพาะ N รายการล่าสุด |
| **Summarization** | ย่อประวัติเก่าให้สั้นลง |
| **Priority Eviction** | ลบข้อมูลสำคัญน้อยก่อน |
| **Token Budgeting** | จัดสรร token ให้แต่ละส่วน |
| **Structured Context** | เก็บ context เป็นส่วนๆ มี label ชัดเจน |

---

## ความเข้าใจผิดที่พบบ่อย

> "ยิ่ง context ยาว ยิ่งดี"

Context ยาว = Token มาก = ค่าใช้จ่ายสูง + latency สูง + LLM อาจ "ลืม" ข้อมูลกลางๆ ยาวๆ ได้ (Lost in the Middle problem)

> "RAG แก้ปัญหา Agent ลืมได้ทั้งหมด"

RAG ช่วยได้มากถ้า retrieval quality ดี แต่ถ้าค้นหาไม่เจอหรือเจอข้อมูลผิด Agent ก็จะตอบผิด

> "Long-term memory = เก็บทุกอย่าง"

เก็บทุกอย่าง = noise = retrieval ล้มเหลว ต้องมีกลไกเลือกสิ่งที่ควรเก็บ สิ่งที่ควรลืม

---

## สรุป

- Memory มีหลายระดับ: short-term, long-term, working, episodic
- Context คือทุกอย่างที่ Agent มีตอนทำงาน ต้องจัดการอย่างมีประสิทธิภาพ
- RAG ช่วยให้ Agent ใช้ข้อมูลภายนอกได้ ไม่พึ่งพาความรู้ในโมเดลอย่างเดียว
- Context Window management เป็นทักษะจำเป็น
- ข้อมูลที่มีคุณภาพ > ข้อมูลที่มากมาย

---

**บทต่อไป:** [Planning และ Reflection](06-planning-and-reflection.md) — การวางแผนและตรวจสอบตัวเอง
