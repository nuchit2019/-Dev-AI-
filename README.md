# เป็น Dev ที่ใช้ AI เป็นจริง ๆ ไม่ใช่แค่ใช้ AI เขียน Code

ทุกวันนี้ Developer จำนวนมากเริ่มใช้ AI ในงานพัฒนา Software แล้ว ไม่ว่าจะเป็นการ Generate Code, Refactor, Debug, เขียน Unit Test, อธิบาย Code หรือช่วย Review

แต่การ **“ใช้ AI เขียน Code ได้”** กับการ **“ใช้ AI พัฒนาระบบเป็น”** เป็นคนละระดับกัน

<img width="1024" height="1536" alt="4c3c25d3-c967-4ee5-84c2-69a891a44b07" src="https://github.com/user-attachments/assets/bd74970e-491b-4bf0-b221-96cee32c22b8" />

> รูปจาก ChatGPT

ถ้าเป้าหมายคือการเป็น Dev ที่ใช้ AI เป็นจริง ๆ สิ่งสำคัญไม่ใช่แค่การเขียน Prompt ให้เก่งขึ้น แต่คือการเปลี่ยนวิธีคิดจาก

```text
Prompt AI
↓
Control AI
```

จากคำถามว่า

> “ต้อง Prompt อย่างไรให้ AI เขียน Code ดี?”

ไปเป็น

> “ต้องกำหนด Context, Requirement, Architecture, Task และ Quality Gate อย่างไร เพื่อให้ AI ทำงานได้ถูกต้องตั้งแต่ต้นจนจบ?”

นี่คือจุดเปลี่ยนสำคัญจาก **AI Coding Assistant** ไปสู่ **AI-Driven Development**

---

# จาก AI Coding Assistant สู่ AI-Driven Developer

ช่วงแรก Developer ส่วนใหญ่มักเริ่มจากการใช้ AI เป็น Coding Assistant

เช่น

* Generate Code
* Refactor Code
* Debug
* Explain Existing Code
* Generate Unit Test
* Generate SQL
* Review Pull Request
* Generate Documentation

ตัวอย่างง่ายที่สุดคือ

```text
Create Product API
```

AI สามารถสร้าง Code ได้ทันที

แต่ปัญหาคือ AI ต้องเดาหลายเรื่อง

```text
Architecture แบบไหน?
ใช้ ORM อะไร?
Error Handling แบบไหน?
Naming Convention เป็นอย่างไร?
ใช้ Repository Pattern หรือไม่?
Logging ใช้อะไร?
Authentication / Authorization แบบไหน?
Existing Project ใช้ Pattern อะไร?
```

ผลลัพธ์อาจ Compile ได้

แต่ไม่ได้หมายความว่า Code นั้นเหมาะกับ Production System

Developer ที่ใช้ AI ได้ดีจึงต้องเริ่มจากการลดสิ่งที่ AI ต้องเดา

แทนที่จะบอกเพียงว่า

```text
Create Product API
```

อาจกำหนดว่า

```text
Create Product API using:

- .NET 8
- Clean Architecture
- Dapper
- PostgreSQL
- Existing Repository Pattern
- Result Pattern
- FluentValidation
- Serilog

Constraints:
- Follow existing project structure
- Do not introduce a new abstraction
- Business logic must not be placed in Controller
- Use async APIs
- Include unit tests
```

สิ่งสำคัญตรงนี้ไม่ใช่ Prompt ที่ยาวขึ้น

แต่คือการ **ควบคุม Environment ที่ AI ใช้ในการตัดสินใจ**

และตรงนี้นำไปสู่ Skill ที่สำคัญที่สุดในยุค AI Development

---

# 1. Context Engineering

Context Engineering คือการทำให้ AI เห็นข้อมูลที่ถูกต้องก่อนเริ่มทำงาน

ปัญหาของ AI ในการพัฒนา Software ไม่ได้มีแค่เรื่องความสามารถในการเขียน Code

แต่คือ AI ไม่รู้บริบทของระบบทั้งหมดโดยอัตโนมัติ

ถ้าเราไม่ให้ Context ที่เพียงพอ AI จะเติมช่องว่างด้วย Assumption

และทุก Assumption คือ Risk

## ตัวอย่าง

สมมติเรามี Insurance Core System และต้องพัฒนา Feature ใหม่เกี่ยวกับ Claim

ถ้าสั่งเพียงว่า

```text
Create API for updating repair price
```

AI อาจสร้าง

```text
PUT /claims/{claimId}/repair-price
```

แล้ว Update Database ตรง ๆ

แต่ระบบจริงอาจมีกฎว่า

* Claim ที่ Approved แล้วห้ามแก้ราคา
* Garage แก้ได้เฉพาะ Labor Cost
* Claim Officer แก้ Parts Cost ได้
* ถ้าเกินวงเงินต้อง Supervisor Approve
* ทุกการเปลี่ยนราคาต้องมี Audit Trail
* ต้องเก็บ Previous Value
* ต้อง Publish Event ไป Pricing Service

ถ้า Context เหล่านี้ไม่ถูกส่งให้ AI

Code ที่สร้างขึ้นอาจ **ถูกทาง Technical แต่ผิดทาง Business**

ดังนั้นควรสร้าง Project Context เช่น

```text
/docs
├── architecture/
│   ├── system-overview.md
│   ├── clean-architecture.md
│   └── integration-pattern.md
│
├── domain/
│   ├── claim-domain.md
│   ├── pricing-rules.md
│   └── approval-rules.md
│
├── standards/
│   ├── coding-standard.md
│   ├── api-standard.md
│   ├── security-standard.md
│   └── testing-standard.md
│
└── requirements/
    └── REQ-2125.md
```

จากนั้นกำหนด Source of Truth ให้ชัดเจน

```text
Priority of truth:

1. Approved Requirement
2. Domain Rules
3. Architecture Decision
4. Existing Code Pattern
5. Coding Standard

Do not introduce a new pattern
unless explicitly approved.
```

นี่คือ Context Engineering

ไม่ใช่เพียงการ “Prompt ให้ละเอียด”

แต่คือการออกแบบ Environment ที่ทำให้ AI มีโอกาสตัดสินใจผิดน้อยลง

---

# 2. Requirement Analysis

AI เขียน Code ได้เร็วมาก

ดังนั้น Requirement ที่ผิดสามารถกลายเป็น **Wrong Code Faster**

ก่อน Implementation Developer จึงควรใช้ AI ช่วยวิเคราะห์ Requirement

ไม่ใช่ให้ AI Code ทันที

## ตัวอย่าง Requirement

Business เขียนมาเพียงว่า

> ผู้ใช้สามารถแก้ไขราคาค่าซ่อมได้

ถ้าเราเริ่ม Coding ทันที จะมีคำถามจำนวนมาก

```text
ใครแก้ราคาได้?

แก้ได้ตอน Claim Status ไหน?

แก้อะไรได้บ้าง?
Labor?
Parts?
Other Cost?

ราคาที่แก้ต้อง Approve ใหม่หรือไม่?

แก้ได้กี่ครั้ง?

ต้องเก็บ Old Value หรือไม่?

ต้องเก็บ Reason หรือไม่?

มี Effective Date หรือไม่?

ต้องส่ง Notification หรือไม่?

มี Integration Impact หรือไม่?
```

AI สามารถช่วยค้นหา Requirement Gap ได้ดีมาก

ตัวอย่างคำสั่ง

```text
Analyze this requirement.

Identify:

- ambiguity
- missing requirement
- business rules
- validation rules
- edge cases
- permission rules
- state transition
- audit requirements
- integration impact
- security impact
- non-functional requirements

Do not implement code yet.
```

AI อาจสรุปออกมาเป็น

```text
Missing Information

1. Allowed Roles
2. Editable Claim Status
3. Maximum Price Change
4. Approval Threshold
5. Audit Requirement
6. Change Reason
7. Notification Requirement
8. Integration Event
```

จากนั้น Developer / BA / Business จึงค่อย Resolve

สมมติได้ Requirement สุดท้ายว่า

```text
REQ-2125

Claim Officer สามารถแก้ไข Labor Price ได้
เมื่อ Claim อยู่ใน Draft หรือ EstimateReview

ถ้ายอดเปลี่ยนมากกว่า 10%
ต้องส่ง Supervisor Approval

ทุกการเปลี่ยนแปลงต้องเก็บ:
- Old Price
- New Price
- Reason
- Updated By
- Updated At

เมื่อแก้สำเร็จต้อง Publish:
RepairPriceChanged
```

ตอนนี้ Requirement พร้อมสำหรับ Design มากกว่าเดิมมาก

---

# 3. เปลี่ยน Requirement เป็น Acceptance Criteria

ก่อน Code ควรทำ Requirement ให้ Testable

ตัวอย่าง

```text
AC-01

Given Claim Status = Draft
And User Role = ClaimOfficer

When Labor Price is updated

Then system updates the price successfully
```

```text
AC-02

Given Claim Status = Approved

When Claim Officer updates Labor Price

Then system must reject the request
```

```text
AC-03

Given Old Price = 1,000
And New Price = 1,200

When price is updated

Then Supervisor Approval is required
```

```text
AC-04

When price is changed

Then Audit Log must contain:
- Old Price
- New Price
- Reason
- User
- Timestamp
```

ตรงนี้สำคัญมาก

เพราะ Acceptance Criteria คือ Contract ระหว่าง

```text
Business
Developer
AI
QA
```

และจะถูกนำไปใช้ตอน Verification ภายหลัง

---

# 4. System Design

Requirement บอกว่า **ต้องสร้างอะไร**

System Design บอกว่า **จะสร้างอย่างไร**

AI สามารถช่วยเสนอ Solution ได้

แต่ Developer ต้อง Control Architecture Decision

ตัวอย่างระบบมีข้อกำหนด

```text
Backend
.NET 8

Architecture
Clean Architecture

Database
PostgreSQL

Data Access
Dapper

Messaging
Kafka

Deployment
AKS

Observability
OpenTelemetry + Serilog
```

จาก Requirement ก่อนหน้า เราอาจ Design Flow เป็น

```text
Client
  ↓
UpdateRepairPrice API
  ↓
Application Service
  ↓
Pricing Domain Rule
  ↓
Repository
  ↓
PostgreSQL
  ↓
Outbox
  ↓
Kafka
  ↓
RepairPriceChanged
```

และกำหนด Responsibility ชัดเจน

```text
Controller
- Request / Response
- Authentication
- Validation entry point

Application Layer
- Use Case orchestration
- Transaction boundary

Domain
- Price change rules
- Approval rule

Infrastructure
- Dapper
- PostgreSQL
- Kafka
```

ตรงนี้ Developer ต้องตัดสินใจในเรื่องที่สำคัญ เช่น

```text
Transaction Boundary อยู่ตรงไหน?

Event publish ต้องใช้ Outbox หรือไม่?

Approval เป็น Sync หรือ Async?

Business Rule อยู่ Domain หรือ Application?

ราคาใช้ decimal precision เท่าไร?

Concurrency ป้องกันอย่างไร?
```

AI ช่วยวิเคราะห์ Option ได้

แต่ไม่ควรเป็นผู้ตัดสิน Architecture โดยไม่มี Constraint

---

# 5. Model First ก่อน API First

อีกแนวทางหนึ่งที่ช่วยให้ AI ทำงานแม่นขึ้นคือ

**อย่าเริ่มจาก Controller**

เริ่มจาก Domain ก่อน

ตัวอย่าง

```text
Claim
 ├── Status
 ├── Estimate
 │    └── RepairItems
 │          ├── LaborPrice
 │          └── PartsPrice
 │
 └── Approvals
```

Business Rule อาจเป็น

```text
Claim.CanModifyRepairPrice()

RepairItem.ChangeLaborPrice()

PriceChange.RequiresApproval()
```

แทนที่จะเขียน Logic กระจายอยู่ใน Controller

```csharp
if (claim.Status == "Approved")
{
    return BadRequest();
}
```

เราต้องการให้ Domain Rule มีความหมาย

```csharp
claim.EnsureRepairPriceCanBeModified();
```

AI จะสร้าง Code ที่ Maintainable กว่า เมื่อ Domain Model ถูกกำหนดก่อน

---

# 6. Task Decomposition

หนึ่งใน Skill สำคัญที่สุดของ Agentic Development คือ

**การแตกงานให้เล็กพอที่ AI จะทำได้แม่น**

Prompt แบบนี้มีความเสี่ยงสูง

```text
Implement Claim Pricing Module
```

Scope ใหญ่เกินไป

AI ต้องตัดสินใจหลายเรื่องพร้อมกัน

```text
Database
Domain
API
Validation
Security
UI
Events
Tests
```

ยิ่ง Task ใหญ่

ยิ่งมี Hidden Assumption มากขึ้น

ควรแตกงานเป็น

```text
EPIC
Claim Repair Price Change

TASK-001
Create database migration

TASK-002
Implement domain rules

TASK-003
Implement repository

TASK-004
Implement application use case

TASK-005
Create API endpoint

TASK-006
Implement audit log

TASK-007
Implement approval workflow

TASK-008
Publish RepairPriceChanged event

TASK-009
Create unit tests

TASK-010
Create integration tests

TASK-011
Create E2E tests
```

แต่ละ Task ควรมี

```text
Goal
Scope
Input
Output
Constraints
Acceptance Criteria
Dependencies
Tests
```

ตัวอย่าง

```text
TASK-002
Implement Repair Price Domain Rules

Scope:
- Claim status validation
- Price difference calculation
- Approval requirement

Constraints:
- Domain layer only
- No database dependency
- No infrastructure dependency

Acceptance Criteria:
- Draft can change price
- Approved cannot change price
- >10% change requires approval

Tests:
- Allowed status
- Forbidden status
- Exactly 10%
- Greater than 10%
```

นี่คือการ Control AI ด้วย Task Boundary

---

# 7. ให้ AI Implement ทีละ Task

เมื่อ Task ถูกแบ่งดีแล้ว

ไม่จำเป็นต้องให้ AI ทำ Feature ทั้งหมดในครั้งเดียว

Flow ที่ปลอดภัยกว่าคือ

```text
Task
↓
AI Implement
↓
Build
↓
Test
↓
Review
↓
Commit
↓
Next Task
```

เช่นให้ AI ทำ TASK-002

AI อาจสร้าง

```csharp
public bool RequiresApproval(
    decimal oldPrice,
    decimal newPrice)
{
    if (oldPrice <= 0)
        return true;

    var percentage =
        Math.Abs(newPrice - oldPrice) / oldPrice;

    return percentage > 0.10m;
}
```

แต่ก่อน Accept เราต้อง Verify ว่า Business หมายถึง

```text
มากกว่า 10%
```

หรือ

```text
ตั้งแต่ 10% ขึ้นไป
```

ความแตกต่างเพียงหนึ่ง Operator

```text
> 0.10
```

กับ

```text
>= 0.10
```

อาจกลายเป็น Production Bug ได้

นี่คือเหตุผลที่ Prompt ที่ดีอย่างเดียวไม่พอ

ต้องมี Verification

---

# 8. Verification — อย่าเชื่อว่า “AI บอกว่าเสร็จแล้ว”

หนึ่งในกับดักของการใช้ AI คือถามว่า

```text
Does this implementation fully satisfy the requirement?
```

AI อาจตอบว่า

```text
Yes.
```

แต่คำตอบนั้นไม่ใช่ Evidence

Developer ต้องเปลี่ยนจาก

```text
AI says it's correct
```

ไปเป็น

```text
We can prove it's correct
```

เช่น Requirement

```text
REQ-2125
Repair Price Change
```

Traceability ต้องเป็น

```text
REQ-2125
   ↓
AC-01
   ↓
TC-001
   ↓
Domain Test

REQ-2125
   ↓
AC-02
   ↓
TC-002
   ↓
Integration Test

REQ-2125
   ↓
AC-04
   ↓
TC-004
   ↓
Audit Verification
```

จากนั้นสร้าง Traceability Matrix

| Requirement | Acceptance Criteria    | Test   | Result |
| ----------- | ---------------------- | ------ | ------ |
| REQ-2125    | Draft can update       | TC-001 | Pass   |
| REQ-2125    | Approved cannot update | TC-002 | Pass   |
| REQ-2125    | >10% needs approval    | TC-003 | Pass   |
| REQ-2125    | Audit required         | TC-004 | Pass   |
| REQ-2125    | Publish event          | TC-005 | Pass   |

นี่คือ **Evidence-Based Development**

---

# 9. ใช้ Test เป็น Control Mechanism

Test ไม่ได้มีไว้ตรวจ Code หลังเขียนเสร็จเท่านั้น

ใน AI Development Test สามารถใช้เป็น Constraint ให้ AI ทำงานได้

ตัวอย่าง

```text
Given
OldPrice = 1,000

When
NewPrice = 1,050

Then
ApprovalRequired = false
```

```text
Given
OldPrice = 1,000

When
NewPrice = 1,101

Then
ApprovalRequired = true
```

จากนั้นให้ AI Implement เพื่อทำให้ Test ผ่าน

Flow จะกลายเป็น

```text
Requirement
↓
Acceptance Criteria
↓
Test
↓
AI Implementation
↓
Test Result
```

แทนที่จะเป็น

```text
Requirement
↓
AI Generate Code
↓
Hope it works
```

---

# 10. Review ต้องแยกหลายมิติ

AI สามารถสร้าง Code ได้เร็วขึ้นมาก

แต่ความเร็วในการ Generate Code ทำให้ Review สำคัญกว่าเดิม

Review ไม่ควรมีแค่

```text
Code compile ไหม?
```

แต่ต้องมีหลาย Dimension

## Functional Review

```text
Business Rule ครบไหม?
Acceptance Criteria ครบไหม?
Edge Case มีไหม?
```

## Architecture Review

```text
Dependency Direction ถูกไหม?
Domain Logic อยู่ถูก Layer หรือไม่?
มี Infrastructure Leak หรือไม่?
เกิด Coupling ใหม่หรือไม่?
```

## Code Review

```text
Readable?
Maintainable?
Duplicate?
Over-engineering?
Exception Handling ถูกไหม?
Logging พอไหม?
```

## Security Review

```text
Authentication
Authorization
BOLA / IDOR
Input Validation
Injection
Sensitive Data
Secrets
```

## Performance Review

```text
N+1 Query?
Missing Index?
Pagination?
Large Payload?
Blocking I/O?
Retry Storm?
```

## Test Review

```text
Happy Path
Negative Case
Boundary Case
Concurrency
Integration
Regression
```

ตรงนี้สามารถใช้ AI หลาย Role มาช่วย Review ได้

---

# 11. อย่าให้ AI ตัวเดียวเขียนและอนุมัติงานตัวเอง

Pattern ที่ควรระวังคือ

```text
Developer Agent
↓
Write Code
↓
Review Own Code
↓
Looks Good
```

เพราะ Agent อาจมี Blind Spot เดิม

แนวทางที่ดีกว่าคือแยก Role

```text
Developer Agent
↓
Code Reviewer Agent
↓
Security Reviewer
↓
QA Agent
```

แต่ละ Role มี Objective ต่างกัน

```text
Developer Agent
→ Implement Requirement

Code Reviewer
→ Maintainability / Architecture

Security Agent
→ Security Risk

QA Agent
→ Requirement Coverage
```

นี่คือพื้นฐานของ Multi-Agent Development

---

# 12. Quality Gate

AI สามารถสร้าง Code ปริมาณมากได้เร็ว

ดังนั้นทีมต้องมี Quality Gate ที่ชัดเจน

ตัวอย่าง Pipeline

```text
Build
↓
Unit Test
↓
Integration Test
↓
Contract Test
↓
E2E Test
↓
Static Analysis
↓
Security Scan
↓
Architecture Review
↓
Human Approval
↓
Merge
```

สำหรับ Enterprise System อาจเพิ่ม

```text
DB Migration Validation
API Compatibility
Event Compatibility
Performance Threshold
Audit Requirement
Observability Check
```

Quality Gate ทำหน้าที่เหมือน Guardrail

AI สามารถทำงานเร็วได้

แต่ไม่สามารถข้าม Quality Standard ได้

---

# 13. Repository-Aware Development

อีกขั้นหนึ่งที่สำคัญคือ AI ต้องเข้าใจ Existing Codebase

AI ไม่ควรเห็นแค่ Requirement

แต่ควรเห็น

```text
Existing Code
Git Diff
Tests
Architecture
Dependencies
Coding Convention
Relevant Domain
```

ก่อนแก้ Code ควรให้ AI ทำ

```text
Read
↓
Understand
↓
Plan
↓
Modify
```

ไม่ใช่

```text
Prompt
↓
Generate New Code
```

ตัวอย่างคำสั่งที่ดี

```text
Before implementing:

1. Inspect the existing Claim module.
2. Identify the current update pattern.
3. Identify relevant domain services.
4. Find existing audit implementation.
5. Find event publishing pattern.
6. Propose the minimal change.

Do not modify code yet.
```

จากนั้นค่อย Approve Plan

---

# 14. เปลี่ยนจาก Prompt-Driven เป็น Plan-Driven

การใช้ AI ที่ Mature มากขึ้นไม่ควรเริ่มจาก

```text
Implement this
```

แต่ควรเริ่มจาก

```text
Analyze
↓
Plan
↓
Approve
↓
Execute
```

ตัวอย่าง

```text
Requirement
↓
AI Analyze
↓
AI proposes plan
↓
Human reviews plan
↓
AI executes task 1
↓
Verify
↓
AI executes task 2
```

จุดที่ Human ต้องเข้ามา Control อาจเรียกว่า **Human Approval Gate**

ตัวอย่าง

```text
Gate 1
Requirement Approved

Gate 2
Architecture Approved

Gate 3
Implementation Plan Approved

Gate 4
Production Deployment Approved
```

AI สามารถ Automate งานจำนวนมากระหว่างแต่ละ Gate

---

# 15. จาก Single Agent ไป Multi-Agent

เมื่อ Process เริ่มนิ่ง สามารถแบ่ง Agent ตาม Role

ตัวอย่าง

```text
BA Agent
→ Requirement Analysis

SA Agent
→ Solution Design

Developer Agent
→ Implementation

Database Agent
→ Schema / Query

QA Agent
→ Test Design

Security Agent
→ Security Review

Reviewer Agent
→ Code / Architecture Review
```

แต่ถ้า Developer ต้องคอยสั่งทุก Agent เอง

```text
BA ทำงานนี้
SA ทำต่อ
Dev ทำต่อ
QA ตรวจต่อ
```

Developer จะกลายเป็น Human Orchestrator

ขั้นต่อไปจึงควรสร้าง Orchestrator

---

# 16. Orchestrator — Single Point of Command

เป้าหมายคือ Developer สั่งระดับ Business Intent

เช่น

```text
Implement REQ-2125
```

แล้ว Orchestrator ทำ Flow ให้

```text
Load Project Context
↓
Analyze Requirement
↓
Find Gaps
↓
Generate Acceptance Criteria
↓
Create Solution Design
↓
Human Approval
↓
Break Down Tasks
↓
Execute Tasks
↓
Run Tests
↓
Review
↓
Generate Report
```

Developer ไม่จำเป็นต้องคอย Prompt ทุก Step

แต่เปลี่ยนไปควบคุม

```text
Intent
Constraints
Approval
Quality
```

นี่คือจุดเริ่มต้นของ Agentic Development

---

# 17. Skill ของ Developer จะเปลี่ยน

เมื่อ AI Coding เก่งขึ้น

มูลค่าของ Developer ไม่ได้หายไป

แต่ Skill ที่มีมูลค่าจะเปลี่ยน

จาก

```text
Syntax
Framework API
Boilerplate Coding
```

ไปสู่

```text
Context Engineering
Requirement Analysis
System Design
Task Decomposition
Verification
Review
Domain Knowledge
Decision Making
```

Developer จะใช้เวลาน้อยลงกับคำถาม

```text
เขียน Code นี้อย่างไร?
```

และใช้เวลามากขึ้นกับคำถาม

```text
เรากำลังสร้างอะไร?

Requirement ถูกต้องหรือยัง?

AI ต้องรู้อะไร?

อะไรคือ Constraint?

Solution นี้เหมาะกับ Architecture หรือไม่?

งานควรแตกอย่างไร?

เราพิสูจน์ได้อย่างไรว่างานเสร็จจริง?
```

---

# Prompt Engineering ยังสำคัญไหม?

สำคัญ

แต่เป็นเพียงส่วนหนึ่งเท่านั้น

สามารถมองแบบนี้ได้

```text
Prompt Engineering
=
How to communicate with AI

Context Engineering
=
What AI should know

System Design
=
What AI is allowed to build

Task Decomposition
=
How much AI should do at once

Verification
=
How we prove AI is correct

Review
=
How we detect what tests missed
```

ดังนั้น Dev ที่ใช้ AI ได้ดีไม่จำเป็นต้องเป็นคนเขียน Prompt ยาวที่สุด

แต่ต้องเป็นคนที่ **ควบคุม Decision Space ของ AI ได้ดีที่สุด**

---

# ตัวอย่าง End-to-End จริง

สมมติมี Requirement

> Claim Officer สามารถแก้ Labor Price ได้ และถ้าราคาเพิ่มเกิน 10% ต้อง Supervisor Approve

Dev ที่ใช้ AI แบบพื้นฐานอาจทำ

```text
Prompt:
Create API to update labor price.
```

แล้ว AI Generate API ออกมา

แต่ Dev ที่ใช้ AI แบบ Control จะทำดังนี้

## Step 1 — Load Context

```text
Read:
- claim-domain.md
- approval-rules.md
- api-standard.md
- architecture.md
- existing Claim module
```

## Step 2 — Requirement Analysis

AI พบคำถาม

```text
Editable Status?
Audit required?
Decrease price requires approval?
Exactly 10% requires approval?
Concurrency handling?
```

Human Resolve

```text
Draft / EstimateReview only

Increase >10% requires approval

Decrease does not require approval

Exactly 10% does not require approval

Audit required

Optimistic concurrency required
```

## Step 3 — Acceptance Criteria

สร้าง

```text
AC-01
Draft claim can update labor price

AC-02
Approved claim cannot update

AC-03
Increase >10% requires approval

AC-04
Exactly 10% does not require approval

AC-05
Audit log must be created

AC-06
Concurrency conflict returns 409
```

## Step 4 — Design

```text
PUT /claims/{claimId}/repair-items/{itemId}/labor-price
```

Flow

```text
API
↓
Command Handler
↓
Claim Domain
↓
Repository
↓
Outbox
↓
RepairPriceChanged Event
```

## Step 5 — Task Breakdown

```text
TASK-001 Domain Rule
TASK-002 Repository
TASK-003 Application Handler
TASK-004 API
TASK-005 Audit
TASK-006 Event
TASK-007 Unit Test
TASK-008 Integration Test
```

## Step 6 — Implement One Task

AI Implement Domain Rule

## Step 7 — Verify

Run

```text
Build
Unit Test
Architecture Test
```

## Step 8 — Review

Reviewer Agent ตรวจ

```text
Domain correctness
Boundary conditions
Dependency violation
Code readability
```

## Step 9 — Continue

ทำ TASK ถัดไปจนจบ

## Step 10 — Final Verification

สร้าง Report

```text
REQ-2125

AC-01 PASS
AC-02 PASS
AC-03 PASS
AC-04 PASS
AC-05 PASS
AC-06 PASS

Unit Tests: 24/24
Integration Tests: 8/8
Security Review: PASS
Architecture Review: PASS
```

ตอนนี้คำว่า **Done** ไม่ได้มาจาก AI บอกว่าเสร็จแล้ว

แต่มี Evidence รองรับ

---

# AI Developer Maturity

สามารถแบ่งระดับการใช้ AI ได้ประมาณนี้

```text
Level 1
AI Chat

↓
ถามคำถามทั่วไป

Level 2
Coding Assistant

↓
Generate / Refactor / Debug Code

Level 3
Context-Aware Development

↓
AI เข้าใจ Project และ Existing Code

Level 4
Spec-Driven Development

↓
Requirement → Spec → Plan → Code → Test

Level 5
Agentic Development

↓
Multiple Agents + Orchestration + Quality Gates

Level 6
AI Software Factory

↓
End-to-End Software Delivery Automation
```

สำหรับ Enterprise Development เป้าหมายที่เหมาะสมไม่จำเป็นต้องกระโดดไป Level 6 ทันที

Level 4–5 มักเป็นจุดที่ Balance ระหว่าง

```text
Speed
Quality
Governance
Human Control
```

ได้ดี

---
<img width="1024" height="559" alt="7338dd92-3a0a-409c-ba55-1f7f8b2333cb" src="https://github.com/user-attachments/assets/c9042fc8-f0a9-4367-ae8f-05fdf9d7ba90" />

> รูปจาก Gemini

# เป้าหมายสุดท้าย: AI Software Factory

เมื่อทุกส่วนเชื่อมกัน Workflow อาจกลายเป็น

```text
Business Requirement
↓
AI Requirement Analysis
↓
Acceptance Criteria
↓
Solution Design
↓
Human Approval
↓
Task Breakdown
↓
AI Agents
↓
Implementation
↓
Automated Tests
↓
Security Review
↓
Architecture Review
↓
Verification
↓
Human Approval
↓
Merge
↓
CI/CD
↓
Deploy
```

Developer ไม่ได้หายไปจาก Process

แต่บทบาทเปลี่ยนจากผู้เขียน Code เป็นหลัก

ไปเป็นผู้ควบคุม

```text
Requirement
Context
Architecture
Decision
Quality
Risk
```

---

# สรุป

การเป็น Dev ที่ใช้ AI เป็นจริง ๆ ไม่ได้วัดจากว่า

```text
ใช้ ChatGPT บ่อยแค่ไหน
```

หรือ

```text
ให้ AI Generate Code ได้เยอะแค่ไหน
```

แต่ควรวัดจากว่าเราสามารถสร้าง Process ที่ทำให้ AI

```text
รู้สิ่งที่ควรรู้

ไม่ต้องเดาสิ่งที่ไม่ควรเดา

ทำงานใน Scope ที่ชัดเจน

ทำตาม Architecture

ถูกตรวจสอบด้วย Test

ถูก Review จากหลายมุม

และมี Evidence ว่างานถูกต้อง
```

ได้หรือไม่

ดังนั้น Skill สำคัญที่สุดของ Developer ในยุค AI จะไม่ใช่เพียง Prompt Engineering

แต่คือ

```text
Context Engineering
Requirement Analysis
System Design
Task Decomposition
Verification
Review
```

ทั้งหมดนี้รวมกันคือความสามารถในการ **Control AI**

และนี่คือความแตกต่างระหว่าง

> **Dev ที่ใช้ AI ช่วยเขียน Code**

กับ

> **Dev ที่ใช้ AI พัฒนาระบบเป็น**

---

> **ขอบคุณ AI**
