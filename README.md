# REST CRUD Serverless on AWS (Node.js + React)

Backend con **Serverless Framework** (API Gateway + Lambda + DynamoDB) y frontend **React** (hosting en Amplify).  
CI/CD con **CodePipeline + CodeBuild** (multi-stage **dev/prod**) y despliegue automático por push a `main`.

---

## ✅ Entregables / Requisitos

- [x] Código **Node.js** (backend) y **React** (frontend)
- [x] Infra automatizada con **Serverless Framework (IaC)**
- [x] API REST de **API Gateway** persiste en **DynamoDB**
- [x] **4–5 Lambdas**: create, get, list, update, delete (sin proxy directo de APIGW a DynamoDB)
- [x] **CI/CD multi-stage** (dev/prod) con CodePipeline/CodeBuild (y frontend con Amplify Hosting)
- [x] **Template funcional y documentado**
- [x] **Repositorio público** con commits frecuentes
- [x] **Capturas** de Source/Build/Artifacts/Variables (incluidas abajo)

---

## 🧱 Arquitectura


```mermaid
flowchart LR
  A[GitHub (main)] -->|Webhook/App| P[CodePipeline]

  subgraph Backend (AWS)
    direction TB
    G[API Gateway REST]
    L1[Lambda createItem]
    L2[Lambda getItem]
    L3[Lambda listItems]
    L4[Lambda updateItem]
    L5[Lambda deleteItem]
    D[(DynamoDB: ItemsTable-${stage})]

    %% Flujo API <-> Lambdas
    G <--> L1
    G <--> L2
    G <--> L3
    G <--> L4
    G <--> L5

    %% Lambdas -> DynamoDB
    L1 --> D
    L2 --> D
    L3 --> D
    L4 --> D
    L5 --> D
  end

  %% CI/CD stages
  B1[Serverless deploy --stage dev] --> G
  B2[Serverless deploy --stage prod] --> G

  %% Frontend
  R[React App (Amplify)] <--> G

