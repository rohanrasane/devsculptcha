# ARB Review Template

Use this template to ensure every ARB review is consistent and centered on intentional architecture:

## 1. Context

* **Project/Service Name:**
* **Submission Date:**
* **Submitter/Team:**
* **Summary:** (What’s changing and why)
* **Business Alignment:** (Which strategic goal or capability)

## 2. Architectural Patterns & Frameworks

* **Patterns Applied:** (e.g., CQRS, Strangler Fig, Bulkhead, Fan‑out + Worker, Saga)
* **Framework Defaults:** (timeouts, retries, pagination, rate limits, telemetry hooks)

## 3. Design Artifacts

* **Diagrams/Docs:** (Links to architecture diagrams, ADRs)
* **AI-augmented Findings:** (Key flags from LLM checks)

## 4. Scale & Resilience Scorecard

* **Isolation:** \[Low/Medium/High]
* **Data Coupling:** \[Loose/Tight]
* **Concurrency Control:** \[Bulkheads/Fan‑out/Saga]
* **Failure Handling:** \[Circuit Breaker/Retry Policies]
* **Observability:** \[Logs/Tracing/Metrics]

## 5. Risk Assessment

* **Potential Risks:** (List scale or reliability concerns)
* **Mitigations:** (Proposed actions or patterns to address)

## 6. Decisions & Next Steps

* **ARB Decision:** \[Approved/Conditional/Rejected]
* **Action Items:** (Owners, deadlines)
* **Follow-up Review:** (If applicable)

---

**Usage:** Fill out sections 1–3 before review. ARB uses the scorecard and risks to guide discussion and documents decisions and next steps in section 6.
