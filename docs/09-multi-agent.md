# บทที่ 9: Multi-agent Systems

---

Agent ตัวเดียวอาจไม่พอสำหรับงานที่ซับซ้อน — บางครั้งเราต้องการ **Agent หลายตัวทำงานร่วมกัน** แต่ละตัวมีบทบาทและความเชี่ยวชาญของตัวเอง

---

## Multi-agent คืออะไร?

Multi-agent System คือระบบที่มี Agent หลายตัวทำงานร่วมกันเพื่อเป้าหมายร่วม โดย:

- แต่ละตัวมี **บทบาท** เฉพาะ
- สื่อสารและส่งต่องานระหว่างกัน
- อาจมีโครงสร้างแบบ hierarchy หรือ peer-to-peer
- รวมผลลัพธ์เพื่อให้ได้คำตอบที่ดีกว่า

---

## ทำไมต้องใช้หลาย Agent?

| ปัญหา | Single Agent | Multi-agent |
|-------|-------------|-------------|
| Context window | จำกัด — ต้องใส่ทุกอย่างใน prompt เดียว | แบ่งงานกัน -> แต่ละ context เล็กลง |
| Tool specialization | Agent หนึ่งตัว tools เยอะ -> สับสน | แต่ละตัวมี tools เฉพาะตัว |
| Error isolation | ผิดพลาดทั้งระบบ | แยก Agent ล้มเหลวได้ ตัวอื่นยังทำงาน |
| Reflection quality | ตรวจสอบตัวเอง bias | Agent ตรวจสอบกันและกันได้ |
| Parallel work | ทำทีละอย่าง | ทำพร้อมกันได้ |

---

## รูปแบบสถาปัตยกรรม

### 1. Supervisor / Manager Agent

Agent หนึ่งตัวเป็น "ผู้จัดการ" แตกงานและส่งให้ Agent ลูกน้อง

```
Manager Agent
├─ รับโจทย์ → วางแผน → มอบหมาย
├─ Research Agent — ค้นหาข้อมูล
├─ Analysis Agent — วิเคราะห์ข้อมูล
├─ Writing Agent — เขียนรายงาน
└─ Review Agent — ตรวจสอบคุณภาพ
```

**เหมาะกับ**: งานที่มีโครงสร้างชัดเจน แบ่งส่วนได้

### 2. Peer-to-peer

Agent แต่ละตัวเท่ากัน ปรึกษาหารือกัน

```
Agent A: "ฉันต้องการข้อมูลตลาด"
Agent B: "ฉันค้นข้อมูลมาให้"
Agent C: "ฉันวิเคราะห์แนวโน้มได้"
→ แลกเปลี่ยนข้อมูลกัน
→ ลงความเห็นร่วม
```

**เหมาะกับ**: งานที่ต้อง brainstorm หรือ consensus

### 3. Pipeline

Agent ต่อกันเป็นสายการผลิต — ผลลัพธ์ของ Agent หนึ่ง ส่งต่อเป็น input ของ Agent ถัดไป

```
Research → Analyze → Write → Review → Publish
Agent A    Agent B    Agent C  Agent D  Agent E
```

**เหมาะกับ**: Workflow ที่มีลำดับตายตัว

### 4. Debate / Discussion

Agent สองตัวขึ้นไปถกเถียงกัน — แต่ละตัวเสนอความเห็นและโต้แย้ง

```
Agent A: "我认为กลยุทธ์ A ดีที่สุด เพราะ..."
Agent B: "ผมไม่เห็นด้วย เพราะ..."
Agent A: "แต่ข้อมูลล่าสุดแสดงว่า..."
→ ได้ข้อสรุปที่มีการถกเถียงรอบด้าน
```

**เหมาะกับ**: งานที่ต้องประเมินหลายมุมมอง

---

## การสื่อสารระหว่าง Agent

### Structured Communication
ส่งข้อมูลในรูปแบบที่กำหนด — JSON, schema-based

```json
{
  "from": "research_agent",
  "to": "analysis_agent",
  "type": "task_result",
  "data": {
    "findings": [...],
    "sources": [...]
  }
}
```

### Unstructured / Natural Language
Agent คุยกันด้วยภาษาธรรมชาติ

```
Research Agent: "ฉันหาข้อมูลครบแล้ว มีตัวเลขตลาดปี 2025"
Analysis Agent: "ขอดูข้อมูลหน่อย"
Research Agent: [ส่งข้อมูล]
Analysis Agent: "ขอบคุณ กำลังวิเคราะห์"
```

---

## ข้อควรระวัง

| ปัญหา | คำอธิบาย |
|-------|----------|
| **Communication overhead** | คุยกันมากเกินไป กิน token |
| **Role confusion** | Agent ทำงานซ้ำซ้อนกัน |
| **Blame game** | เมื่อผิด — ไม่รู้ว่า Agent ไหนผิด |
| **Increased latency** | ต้องรอทุก Agent ทำงาน |
| **System complexity** | Debug ยากกว่า single agent |
| **Cost multiplication** | แต่ละ Agent = ค่า LLM |
| **Groupthink** | Agent เห็นพ้องโดยไม่ได้ตรวจสอบจริง |

---

## เมื่อไม่ควรใช้ Multi-agent

- งานเล็กๆ ที่ Agent ตัวเดียวทำได้
- งานที่ latency sensitive
- ต้นทุนเป็นปัจจัยหลัก
- ระบบยังไม่ stable — เพิ่มความซับซ้อนโดยไม่จำเป็น

---

## Single Agent vs Multi-agent: วิธีเลือก

```
งานนี้...
├─ ต้องการความเชี่ยวชาญหลายด้าน? → Multi-agent
├─ ต้องทำงานพร้อมกัน? → Multi-agent
├─ ต้องการให้ตรวจสอบกันและกัน? → Multi-agent
├─ งานไม่ซับซ้อน ทำคนเดียวได้? → Single Agent
├─ ต้องการความเร็ว? → Single Agent
└─ งบประมาณจำกัด? → Single Agent
```

---

## ความเข้าใจผิดที่พบบ่อย

> "ยิ่งมี Agent เยอะ ยิ่งดี"

Agent มากขึ้นไม่ใช่คำตอบ — ค่าใช้จ่ายเพิ่ม ความซับซ้อนเพิ่ม performance อาจลดลง เริ่มจาก 2-3 ตัวก่อน

> "Multi-agent แก้ปัญหาทุกอย่างได้"

Multi-agent แก้ปัญหา scalability และ specialization แต่ไม่ใช่ silver bullet — ยังมีปัญหาพื้นฐานของ LLM เหมือนเดิม

> "Agent คุยกันเอง = เข้าใจกัน"

ภาษา LLM สื่อสารกันอาจ misunderstand ได้ — ต้องออกแบบ communication protocol ให้ดี

---

## สรุป

- Multi-agent = หลาย Agent ทำงานร่วมกัน แต่ละตัวมีบทบาทเฉพาะ
- รูปแบบ: Supervisor, Peer-to-peer, Pipeline, Debate
- ข้อดี: specialization, isolation, parallel, cross-check
- ข้อเสีย: cost, latency, complexity, communication overhead
- เลือกใช้เมื่อจำเป็น — อย่าเพิ่ม complexity โดยใช่เหตุ

---

**บทต่อไป:** [Evaluation และ Verification](10-evaluation-and-verification.md) — วัดและยืนยันคุณภาพ Agent
