# REST CRUD AWS (Serverless + API Gateway + Lambda + DynamoDB + Amplify)

Infraestructura con **Serverless Framework** (IaC), CI/CD con **CodePipeline/CodeBuild**, y frontend **React (Vite) + Chakra UI**.

## Estructura
rest-crud-aws/
├─ backend/ # API + Lambdas + DynamoDB via Serverless
│ ├─ serverless.yml
│ ├─ package.json
│ └─ src/handler.js
├─ .aws/
│ └─ buildspec.yml # CodeBuild: despliegue serverless
├─ frontend/ # UI React
│ ├─ package.json
│ ├─ vite.config.js
│ ├─ index.html
│ └─ src/
│ ├─ main.jsx
│ └─ App.jsx
└─ README.md

## Variables
- Front: `VITE_API_URL` → URL del Stage `dev` de API Gateway.

