# Livraria Ágora AI Agent

Welcome to the **Ágora Librarian** repository, an artificial intelligence-guided customer service and sales system for Livraria Ágora.

This project is not a demo chatbot. It is a product focused on solving a real consultative retail problem: customers searching for books based on life dilemmas, career transitions, or emotional needs, rather than by specific titles or authors.

## Documentation

Full project documentation (including Product Requirements, Specifications, and Architecture Decisions) is available via MkDocs. 

To view the documentation locally:
```bash
mkdocs serve
```
Then visit `http://127.0.0.1:8000/`. The documentation is available in both Portuguese and English.

## Project Architecture

The system is designed by separating conversational intelligence (LLM) from transactional truth (Database). The AI recommends, but the rigid system dictates price, stock, and approval.

- **Frontend:** React + Vite
- **Backend:** FastAPI
- **Agent:** PydanticAI
- **Vector DB (RAG):** Qdrant
- **Database:** Postgres
- **Observability:** Logfire
- **Payments:** AbacatePay

## Repository Structure

The project follows a structured monorepo pattern to facilitate local execution via Docker:

*   `docs/`: Live project documentation (MkDocs).
*   `backend/`: FastAPI API, PydanticAI agents, and instrumentation with Logfire.
*   `frontend/`: Customer chat interface and approval dashboard in React + Vite.
*   `infra/`: `docker-compose.yml`, Qdrant configurations, and fake database.
*   `.antigravity.md` / `CONTEXT.md`: Business rules and constraints for code agents.

## How to run it (Development)

To test the product locally without friction:

1. Clone the repository.
2. Configure your keys in the `.env` file (Groq API Key, Logfire Token, AbacatePay Dev Token).
3. Run the following command:

```bash
docker compose up -d
```

*Docker will spin up Qdrant, the FastAPI backend, and the React frontend. The database will be automatically populated with the fake library script in Python on the first initialization.*
