---
name: product-manager-prd
description: Author comprehensive, actionable Product Requirement Documents (PRDs), user personas, user story maps, acceptance criteria (Gherkin syntax), edge-case matrices, and release milestone roadmaps. Use this skill when transforming vague product concepts into engineering-ready specifications, planning feature releases, scoping MVP vs. post-MVP boundaries, or prioritizing backlog items using RICE/MoSCoW frameworks.
---

# Product Manager & PRD Architect

An elite Product Management skill for transforming high-level business objectives and ambiguous feature requests into crisp, unambiguous, execution-ready Product Requirement Documents (PRDs) that bridge business strategy with engineering execution.

---

## 1. The Anatomy of a High-Impact PRD

A world-class PRD must address **Why**, **Who**, **What**, **What Not**, and **How We Measure Success**.

```text
1. Problem Statement & Executive Summary
2. Target Personas & User Scenarios
3. Success Metrics & Key Results (KPIs / OKRs)
4. Functional Scope (In-Scope vs. Out-of-Scope)
5. Detailed User Stories & Acceptance Criteria (Gherkin)
6. Edge Cases, Failure Modes & Error Handling
7. Non-Functional Requirements (Security, Performance, Scale)
8. Technical Dependencies & Open Questions
9. Rollout, Analytics & Go-to-Market Plan
```

---

## 2. Standard PRD Template Structure

When asked to generate a PRD, follow this structured format:

### Executive Summary & Problem Framing
- **The Problem**: What specific user pain point exists today? What data or customer feedback proves this problem is worth solving?
- **The Solution Vision**: How does this proposed feature solve the problem in an intuitive, differentiated manner?
- **Business Alignment**: How does this connect to overall company OKRs and revenue goals?

### Success Metrics (North Star & Guardrails)
- **Primary Metric**: The direct indicator of feature adoption/value (e.g., `% of active users completing checkout within 60s`).
- **Secondary Metrics**: Supporting business indicators (e.g., Conversion Rate $+15\%$, Day-30 Retention $+8\%$).
- **Guardrail Metrics**: Metrics that must NOT degrade (e.g., Page latency, customer support ticket volume, refund rate).

### Prioritization & Scoping (MoSCoW Matrix)
- **Must Have (P0 - MVP)**: The non-negotiable core functionality without which the feature cannot launch.
- **Should Have (P1 - Fast Follow)**: Important additions that significantly enhance user value.
- **Could Have (P2 - Backlog)**: Desirable polish or edge-case automation.
- **Won't Have (Explicitly Out of Scope)**: Specific features deliberately deferred to prevent scope creep.

---

## 3. User Stories & Acceptance Criteria (Gherkin Standard)

Structure functional requirements as user stories paired with unambiguous acceptance criteria:

```gherkin
User Story:
As a workspace admin
I want to invite team members via bulk CSV upload
So that I can onboard our 500-person department in minutes rather than typing individual emails.

Acceptance Criteria:
Scenario: Successful CSV upload with valid emails
  Given I am logged in as an Admin on the "Team Settings" page
  When I upload a valid CSV file containing up to 1,000 email addresses
  And I click "Send Invitations"
  Then the system creates pending invitation tokens for each valid email
  And displays a progress bar showing "X of Y invites sent"
  And displays a success notification with the count of successful invites.

Scenario: Handling duplicates and invalid email formats in CSV
  Given the uploaded CSV contains 5 invalid email formats and 3 existing members
  When the file is processed
  Then the valid emails receive invitations
  And a downloadable error report is provided listing the exact row numbers and error reasons.
```

---

## 4. Edge Cases & Risk Mitigation Matrix

Always include a dedicated analysis of boundary conditions:

| Scenario / Edge Case | Expected System Behavior | User Experience |
| :--- | :--- | :--- |
| **Network disconnection mid-action** | Client buffers action; retries idempotently upon reconnection. | Non-intrusive banner: *"Reconnecting... your changes are saved locally."* |
| **Concurrent edits by multiple users** | Last-write-wins with field-level merging or collaborative conflict prompt. | Highlight conflicting fields with visual diff comparison. |
| **Permission revocation during active session** | Immediate invalidation of privileged mutations with `403 Forbidden`. | Modal: *"Your permissions have changed. Please refresh."* |
| **Rate limit reached** | Request throttled with `429 Too Many Requests`. | Friendly countdown timer: *"You can try again in 30 seconds."* |
