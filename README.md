# REST CRUD Serverless on AWS (Node.js + React)

Backend Serverless Framework (API Gateway + Lambda + DynamoDB) y Frontend React (Amplify Hosting) con CI/CD en CodePipeline + CodeBuild para **multi-stage (dev/prod)**.

---

## 📐 Arquitectura

> Diagrama Mermaid

```mermaid
flowchart LR
  A[GitHub (main)] -->|Webhook/App| P[CodePipeline]
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
    D[(DynamoDB<br/>ItemsTable-${stage})]
  end

  %% Los builds despliegan hacia el API Gateway (nodo G)
  B1 -->|serverless deploy --stage dev| G
  B2 -->|serverless deploy --stage prod| G

  %% Flujo API <-> Lambdas
  G <-->|REST| L1
  G <-->|REST| L2
  G <-->|REST| L3
  G <-->|REST| L4
  G <-->|REST| L5

  %% Lambdas -> DynamoDB
  L1 --> D
  L2 --> D
  L3 --> D
  L4 --> D
  L5 --> D

  %% Frontend
  subgraph Frontend
    R[React App (Amplify)]
  end

  R <-->|HTTP| G

