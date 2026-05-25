# บทที่ 10: Evaluation และ Verification

---

Agent ทำงาน "ถูกต้อง" หรือยัง? — คำถามนี้ตอบยากกว่าที่คิด

Evaluation และ Verification คือกระบวนการที่ช่วยให้เราตอบคำถามนี้ได้

---

## Evaluation vs Verification

| | Evaluation | Verification |
|---|------------|--------------|
| **คือ** | วัดคุณภาพโดยรวม | ตรวจสอบความถูกต้อง |
| **คำถาม** | "Agent ดีแค่ไหน?" | "Agent ทำงานนี้ถูกต้องไหม?" |
| **เมื่อใช้** | หลังพัฒนา, ก่อน deploy | ระหว่างหรือหลังทำงาน |
| **เครื่องมือ** | Benchmarks, metrics, tests | Validation, review, audit |
| **ผลลัพธ์** | Score / Rating | Pass / Fail |

---

## Evaluation: วัดคุณภาพ Agent

### Metrics ที่ควรวัด

| Metric | คำอธิบาย |
|--------|----------|
| **Task Completion Rate** | สัดส่วนงานที่สำเร็จ / งานทั้งหมด |
| **Accuracy** | ความถูกต้องของคำตอบ |
| **Precision / Recall** | สำหรับงาน retrieval หรือ classification |
| **Cost per Task** | จำนวน token / ค่าใช้จ่ายต่อหนึ่งงาน |
| **Latency** | เวลาที่ Agent ใช้ตั้งแต่รับงานถึงเสร็จ |
| **Error Rate** | สัดส่วนที่ Agent ทำงานผิดพลาด |
| **Human Intervention Rate** | จำนวนครั้งที่ต้องให้มนุษย์ช่วย |
| **Hallucination Rate** | สัดส่วนที่ Agent สร้างข้อมูลเท็จ |

### ตัวอย่าง Evaluation Report

```
Task: "วิเคราะห์แนวโน้มตลาด AI ไทย"
──────────────────────────
Task Completion: ✅ สำเร็จ
Accuracy Score:   8.5/10
Cost:             2,350 tokens ($0.047)
Latency:          12.3 seconds
Steps:            6 (plan → search → search → read → analyze → summarize)
Tools Used:       search_web (2), read_url (1)
Human Intervention: 0
Hallucination:    1 (ตัวเลข GDP ผิด — ใช้ข้อมูลปีก่อน)
──────────────────────────
```

---

## Verification: ตรวจสอบผลลัพธ์

### สิ่งที่ควรตรวจสอบ

1. **Factual Correctness** — ข้อมูลถูกต้องหรือไม่?
2. **Completeness** — ครบถ้วนหรือไม่?
3. **Safety** — เนื้อหาปลอดภัย ไม่เป็นอันตราย?
4. **Consistency** — ตรงกับ history และ context หรือไม่?
5. **Format Compliance** — ตรงตามรูปแบบที่กำหนด?
6. **Source Attribution** — อ้างอิงถูกต้องหรือไม่?

### วิธีการ Verification

#### Automated Verification
ใช้ระบบตรวจสอบอัตโนมัติ:
- Validation rules
- Regex pattern matching
- เปรียบเทียบกับ ground truth
- Unit test สำหรับ tool calls
- Schema validation

#### Human Review
มนุษย์ตรวจสอบ:
- Spot check (สุ่มตรวจ)
- Full review (ตรวจทั้งหมด)
- Review by exception (ตรวจเฉพาะกรณีสงสัย)

#### Cross-validation
ใช้ Agent อีกตัวตรวจสอบ:
```
Agent A: สรุปรายงาน
Agent B: "ตัวเลขในรายงาน: revenue 1.2M — ตรวจสอบจากแหล่งข้อมูล → ถูกต้อง"
Agent B: "วันที่ 25 พ.ค. — ไม่มีในแหล่งข้อมูล → falle? → mark for review"
```

---

## Test Cases สำหรับ Agent

### Unit Tests
ทดสอบแต่ละ component ทีละส่วน:
- Tool call ถูกต้องไหม?
- Tool result parsing ถูกต้องไหม?
- System prompt ทำงานถูกต้องไหม?

### Integration Tests
ทดสอบการทำงานร่วมกัน:
- Agent + Tools ทำงานด้วยกันได้ไหม?
- Memory system เก็บและเรียกค้นได้ไหม?
- Human-in-the-loop workflow ถูกต้องไหม?

### End-to-End Tests
ทดสอบ workflow ทั้งหมด:
- ตั้งแต่รับงาน → วางแผน → ใช้ tools → สรุปผล
- เปรียบเทียบ output กับ expected output

### Edge Case Tests
ทดสอบกรณีพิเศษ:
- Agent เจอ error จาก tool → รับมือยังไง?
- Agent เจอข้อมูล противоречивый → ทำยังไง?
- Context window ใกล้เต็ม → จัดการยังไง?

---

## Observability และ Logging

การมองเห็นสิ่งที่ Agent ทำคือพื้นฐานของการ verification:

### ข้อมูลที่ควร Log

| ข้อมูล | ประโยชน์ |
|--------|----------|
| **Thought** | Agent คิดอะไรอยู่? |
| **Tool Calls** | เรียกใช้ tool อะไร? parameters? |
| **Tool Results** | tool คืนค่าอะไร? |
| **Errors** | มี error อะไร? |
| **Latency** | แต่ละ step ใช้เวลาเท่าไหร่? |
| **Token Usage** | ใช้ token ไปเท่าไหร่? |
| **Human Decisions** | มนุษย์อนุมัติหรือ reject? |

---

## Production Readiness Checklist

ก่อนนำ Agent ไปใช้จริง:

- [ ] Evaluation metrics ผ่านเกณฑ์
- [ ] Test cases ครอบคลุม edge cases
- [ ] Error handling ทำงานถูกต้อง
- [ ] Human-in-the-loop workflow ทดสอบแล้ว
- [ ] Logging และ monitoring พร้อม
- [ ] Rate limiting และ cost control
- [ ] Fallback plan เมื่อ Agent ล้มเหลว
- [ ] Audit trail ถูกบันทึก
- [ ] Security review ผ่าน

---

## ความเข้าใจผิดที่พบบ่อย

> "Evaluation = ถามมนุษย์ว่าตอบดีไหม"

ไม่พอ — ต้องมี metrics ที่วัดได้ objective ด้วย เช่น task completion, cost, latency

> "Verification ทำให้ Agent ช้าลง"

Verification ที่ดีไม่เพิ่ม latency มาก — ทำให้มั่นใจว่าผลลัพธ์ถูกต้องก่อนส่ง user การ verify หลังจากจบ workflow ก่อนส่ง user เป็น trade-off ที่คุ้มค่า

> "Agent ผ่าน test แล้ว = พร้อม production"

Test ใน lab กับ world ต่างกัน — ต้อง monitor จริง และเตรียม rollback plan

---

## สรุป

- Evaluation วัดคุณภาพโดยรวม — Verification ตรวจสอบความถูกต้อง
- ใช้ metrics หลากหลาย: completion rate, accuracy, cost, latency
- Test หลายระดับ: unit → integration → e2e → edge case
- Observability คือพื้นฐาน — ถ้ามองไม่เห็นว่าทำอะไร จะ validate ไม่ได้
- Production readiness ต้อง checklist ชัดเจน
- ไม่มีระบบไหน perfect — ต้อง monitor และปรับปรุงต่อเนื่อง

---

**จบหลักสูตร AI Agent Fundamentals**

---

🎉 ขอแสดงความยินดี! คุณเรียนครบทั้ง 10 บทแล้ว

- [กลับไปที่ README](../README.md) เพื่อดูภาพรวมทั้งหมด
- [ดู Roadmap](../ROADMAP.md) เพื่อดูแผนการพัฒนา
- [ดูแนวทางการเรียนรู้เพิ่มเติม](../notes/course-outline.md)
