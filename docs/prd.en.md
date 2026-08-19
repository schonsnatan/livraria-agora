# PRD - Ágora Librarian (Livraria Ágora)

## 1. Context and Product Vision
Livraria Ágora is a curation space focused on personal development, philosophy, psychology, business, and complex literature. The current e-commerce model fails by forcing the customer to search by "title" or "author".

Ágora's customer often arrives with an existential dilemma, a career transition, or a moment of grief, seeking guidance that unites language and logic without falling into self-help clichés.

The **Ágora Librarian** is an autonomous customer service system designed to fill this gap. It acts as an empathetic curator who understands the underlying need and guides the customer from their initial venting to closing the sale, ensuring stock and price accuracy.

## 2. Personas

### The Contracting Client: Catarina (Bookstore Owner)
Literary curator and former psychologist. She understands that recommending the wrong book to someone in a state of vulnerability can cause harm.
**Her main pains and fears:**

- **Financial Hallucination:** She is terrified that the AI will invent promotions, give unrealistic discounts, or sell out-of-stock books.
- **Lack of Control (The fear of "Magic"):** Needs to be able to audit conversations to understand why a recommendation was made.
- **Autonomy without Supervision:** Does not want the AI to close irreversible transactions without someone from the team doing a final sanity check.
- **Reputational Risk:** Fear that the agent will try to act as a therapist rather than a curator, crossing the line between a reading suggestion and clinical advice.

### The End User: Reader in Search of Answers
Professionals, students, and avid readers. They look for profound and well-founded works. They usually formulate requests in natural language focused on their pain (e.g., "I need something to deal with the anxiety of leading a new team").

## 3. Product Requirements (What we want to build)

### 3.1. Intelligence and Curation (Agentic AI & RAG)
*   The agent must capture emotional and philosophical nuances, translating feelings into semantic search parameters.
*   The recommendation must be purely based on the store's catalog, using enriched metadata (Catarina's reviews, thematic tags).
*   The AI must maintain a consultative posture, offering insights through the works, but never providing direct advice.

### 3.2. Transactional Shielding
*   The agent is not allowed to calculate prices off the top of its head. All value and availability information must be extracted in real-time from the relational database.
*   The system must prevent the completion of a sale without validating stock at the exact moment of checkout.

### 3.3. Approval Flow (Human-in-the-Loop)
*   The intelligence conducts the negotiation and assembles the "cart" (Draft Order).
*   Before generating the payment link (AbacatePay), the system issues an alert to the store's dashboard.
*   Catarina or a team member quickly reviews the history and approves the billing issuance.

### 3.4. Traceability and Costs
*   Every interaction must be monitored so that it is possible to trace the AI's train of thought (Logfire integration).
*   The cost per token/session must be visible to ensure the system's financial viability.

## 4. Customer Journey (Happy Path)
1.  **Discovery:** Customer enters the chat and describes their current life moment.
2.  **Curation:** Agent investigates (if necessary) and proposes 1 to 2 books, explaining the *why* of the recommendation based on the reported pain.
3.  **Decision:** Customer agrees with the suggestion and asks to buy.
4.  **Draft:** Agent confirms the data, informs the real price (queried in the DB) and creates the order with `PENDING` status.
5.  **Approval:** Catarina approves on the dashboard.
6.  **Payment:** Customer receives the link, pays, and the system issues the receipt.

## 5. Scope (V1)

**What GOES INTO the first version:**

- Fluid chat interface for the customer (React).
- Orchestrated main agent (PydanticAI) with hybrid search for recommendation (Qdrant).
- Simple dashboard for order approval.
- Integration with simulated payment gateway (AbacatePay).
- *Fake* relational database generated via Python script to represent the catalog, prices, and stock.

**What is LEFT OUT of the first version:**

- Real shipping calculation integrated with carriers.
- Automated return flow.
- Recommendations based on the customer's past purchase history (focus on present pain).
- Production deployment (focus on running locally via Docker Compose).

## 6. Success Metrics

- **Assisted Conversion Rate:** % of conversations that result in an order sent for approval.
- **Zero Price Hallucination:** 100% accuracy in the prices informed in the chat compared to the database.
- **Catarina's Rejection Rate:** % of orders blocked on the dashboard (if it's too high, the AI is recommending poorly).