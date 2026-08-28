---
name: technical-writer-docs
description: Author clear, concise, developer-centric technical documentation, API reference manuals, SDK onboarding guides, architecture decision records (ADRs), and troubleshooting walkthroughs using the Diátaxis documentation framework (Tutorials, How-To Guides, Reference, Explanation). Use this skill when writing READMEs, drafting API documentation with curl/SDK code examples, documenting architecture patterns, creating developer onboarding guides, or standardizing engineering docs.
---

# Technical Documentation & API Writer

An elite technical documentation skill for writing accurate, elegant, developer-first documentation, interactive tutorials, comprehensive API references, and architecture blueprints based on the proven Diátaxis framework.

---

## 1. The Diátaxis Documentation Framework

Every technical document must serve one of four distinct user intents:

```text
               Practical Direction    |    Theoretical Knowledge
            --------------------------|--------------------------
Learning-   |  1. TUTORIALS           |  4. EXPLANATION
Oriented    |  (Learning by doing)    |  (Understanding context)
            |-------------------------|--------------------------
Task-       |  2. HOW-TO GUIDES       |  3. REFERENCE
Oriented    |  (Solving specific      |  (Information lookup)
            |   problems)             |
```

1. **Tutorials (Learning-Oriented)**: Step-by-step onboarding journeys for beginners to achieve an initial success state ("Hello World" to working prototype). No extraneous theory.
2. **How-To Guides (Problem-Oriented)**: Focused recipes for real-world tasks (e.g., "How to configure OAuth with GitHub in Next.js"). Assumes basic competence; focuses on specific outcomes.
3. **Reference (Information-Oriented)**: Precise, exhaustive technical descriptions of APIs, schemas, CLI parameters, and error codes. Fact-driven, authoritative, structured.
4. **Explanation (Understanding-Oriented)**: Architectural deep dives, design trade-offs, and background reasoning (e.g., "Why we chose SQLite over PostgreSQL for local replication").

---

## 2. Standard API Reference Format

When documenting REST or GraphQL endpoints, follow this developer-friendly standard:

```markdown
## `POST /v1/payments/charges`

Creates a credit card or digital wallet charge for a customer transaction.

### Authentication
`Bearer <api_key>` (Requires `payments:write` scope)

### Request Parameters

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `amount` | `integer` | Yes | The amount to charge in cents (e.g., `2000` for $20.00). |
| `currency` | `string` | Yes | Three-letter ISO currency code (`usd`, `eur`, `gbp`). |
| `customer_id` | `string` | Yes | Unique ID of the customer (e.g., `cus_987654321`). |
| `metadata` | `object` | No | Key-value pairs for custom application tracking. |

### Code Examples

#### cURL
```bash
curl -X POST https://api.example.com/v1/payments/charges \
  -H "Authorization: Bearer sk_live_abc123" \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 2000,
    "currency": "usd",
    "customer_id": "cus_987654321"
  }'
```

#### TypeScript / Node.js
```typescript
import { ExampleClient } from '@example/sdk';

const client = new ExampleClient({ apiKey: process.env.API_KEY });

const charge = await client.charges.create({
  amount: 2000,
  currency: 'usd',
  customerId: 'cus_987654321',
});

console.log(`Charge succeeded: ${charge.id}`);
```

### Response (`201 Created`)
```json
{
  "id": "ch_123456789",
  "status": "succeeded",
  "amount": 2000,
  "currency": "usd",
  "customer_id": "cus_987654321",
  "created_at": "2026-08-29T00:00:00Z"
}
```

### Error Responses
- `400 Bad Request`: `invalid_amount` - The amount must be at least 50 cents.
- `402 Payment Required`: `card_declined` - The card was declined by the issuing bank.
```

---

## 3. Style & Tone Guidelines

1. **Active Voice & Direct Imperatives**:
   - Write *"Run `npm test` to verify your changes"* instead of *"The user should run the tests to ensure functionality"*.
2. **Clarity Over Jargon**:
   - Explain non-standard acronyms on first mention.
   - Use copyable code snippets with clear placeholders (`<YOUR_API_KEY>`, `<WORKSPACE_ID>`).
3. **Prevent Staleness**:
   - Link to centralized types/schemas where possible.
   - Avoid hardcoding dynamic version numbers into explanatory text.
