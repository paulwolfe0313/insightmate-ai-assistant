```mermaid
graph TD
    A[User Interface] -->|Slack Command<br>Web Dashboard| B[FastAPI Backend]
    B --> C[LangChain RAG Pipeline]
    C --> D[Vector Database (pgvector / Pinecone)]
    D --> E[Embedded Chunks from Docs]
    C --> F[LLM (LLaMA 3 / Mistral / OpenAI)]
    B --> G[Observability + Logs<br>(LangSmith, Traceloop)]

    subgraph Ingestion Layer
        H[PDF/CSV<br>SharePoint API<br>REST APIs] --> I[Ingestion Scripts]
        I --> D
    end

    subgraph Deployment Infrastructure
        B & C & F --> J[Docker Containers]
        J --> K[AWS ECS / Lambda]
        K --> L[Terraform IaC]
    end
