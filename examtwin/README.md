# ExamTwin

ExamTwin was built for OpenAI Build Week. A student adds course materials, defines how the real exam is structured and sits a timed practice exam. Earlier attempts show which skills need more practice and inform the next mock.

![ExamTwin home with a practice recommendation](./assets/home-demo.jpg)

## Setting up and sitting a mock

An exam belongs to a subject. Its setup records the source material, question blueprint, timing and rules. A student can review these before starting a mock. During the session, the interface saves answers; the submitted attempt is kept for review.

![Exam setup with five steps](./assets/exam-setup-demo.jpg)

```mermaid
flowchart LR
  A[Subject] --> B[Exam workspace]
  B --> C[Context files and blueprint]
  C --> D[Scenario and rules]
  D --> E[Grounded mock generation]
  E --> F[Focused timed attempt]
  F --> G[Durable answers and result]
  G --> H[Statistics and adaptive next step]
  H -. evidence from prior attempts .-> E
```

The analytics screen shows previous attempts, evaluations and the evidence behind a suggested next step. Generated feedback is study guidance, not an official grade.

![Attempt history and skill analytics in the local demo](./assets/analytics-demo.jpg)

## System design

![ExamTwin application and worker architecture](./system-design-preview.svg)

[Editable diagram](./system-design.excalidraw)

The Next.js app keeps the session in an HTTP-only cookie and proxies requests to FastAPI. SQLAlchemy stores subjects, exams, attempts and answers in PostgreSQL. Alembic versions the schema. Redis and Dramatiq support background processing for uploaded materials; the source also has a pgvector retrieval layer, object storage and optional Vertex AI generation and evaluation.

```mermaid
flowchart LR
  U[Student browser] --> W[Next.js 16 + React 19]
  W -->|HTTP-only session / API proxy| A[FastAPI]
  A --> M[SQLAlchemy async domain layer]
  M --> P[(PostgreSQL + pgvector)]
  A --> R[(Redis)]
  R --> J[Dramatiq worker]
  J --> O[Object storage]
  J --> V[Optional Vertex AI generation and evaluation]
  O --> J
  V --> J
  A --> W
```

The screenshots come from the browser-only demo, which stores its fictional exam data locally. That run exercises the student interface and does not verify the external AI providers or the deployed backend. [Source repository](https://github.com/Rokki-Khazratov/OpenAI-buildweek).

[All case studies](../)
