# REST CRUD Serverless on AWS (Node.js + React)

Backend con **Serverless Framework** (API Gateway + Lambda + DynamoDB) y frontend **React** (Amplify Hosting).  
CI/CD con **GitHub → CodePipeline → CodeBuild** para **multi-stage**: `dev` y `prod`.

---

## 🚀 Arquitectura

> Si tu GitHub no renderiza Mermaid, exporta el gráfico desde https://mermaid.live y súbelo como imagen (ver sección “Capturas”).

```mermaid
flowchart LR
  A[GitHub (main)] -->|Webhook| P[CodePipeline]
  P -->|Source| S[Source (GitHub)]
  P -->|Stage 1| B1[CodeBuild (STAGE=dev)]
  P -->|Stage 2| B2[CodeBuild (STAGE=prod)]

  subgraph "AWS Backend"
    subgraph API
      G[API Gateway REST]
      L1[Lambda createItem]
      L2[Lambda getItem]
      L3[Lambda listItems]
      L4[Lambda updateItem]
      L5[Lambda deleteItem]
    end

    %% Flujo API -> Lambdas
    G --> L1 & L2 & L3 & L4 & L5

    %% DynamoDB
    D[(DynamoDB<br/>ItemsTable-${stage})]
    L1 --> D
    L2 --> D
    L3 --> D
    L4 --> D
    L5 --> D
  end
