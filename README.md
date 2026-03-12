# EInvoice Microservices Platform

A scalable e-invoice platform built with NestJS microservices, following an Nx monorepo architecture.

### Core capabilities
- Authentication & authorization via Keycloak
- Invoice creation and processing
- Payment integration
- PDF generation
- Email delivery
- Product and user management
- Event-driven extensibility with Kafka

## Architecture Overview

The system is organized around a BFF layer and multiple domain-driven microservices.

- **BFF**: entry point for client applications
- **Authorizer**: access control and Keycloak integration
- **Invoice**: core invoice domain logic
- **Mail**: email delivery and templates
- **PDF Generator**: invoice PDF generation
- **Product**: product domain management
- **User Access**: user domain and permission-related data
- **Media**: media/file handling

<img width="2402" height="1725" alt="mermaid-diagram" src="https://github.com/user-attachments/assets/852b41f4-e691-4c9a-803f-dfe9ba604c63" />

## Communication Patterns

The platform uses multiple communication styles depending on the use case:

- **HTTP/REST**: client to BFF
- **gRPC**: synchronous internal service-to-service communication
- **Kafka**: asynchronous event-driven workflows
- **Redis**: caching and shared fast-access data

## Key Business Flow
<img width="3570" height="1798" alt="flow-mermaid-diagram (2)" src="https://github.com/user-attachments/assets/ba9e5d48-7ebb-43e7-8578-e1271de06bc4" />

## Monorepo Structure

apps/
  authorizer/
  bff/
  invoice/
  mail/
  media/
  pdf-generator/
  product/
  user-access/
  einvoice-e2e/

libs/
  configuration/
  constants/
  decorators/
  entities/
  guards/
  interfaces/
  kafka/
  observability/
  saga-orchestration/
  utils/

docker/
tools/

## Architecture Overview

flowchart LR
    Client[Web / Mobile Client] --> BFF[BFF]

    BFF --> Authorizer[Authorizer Service]
    BFF --> Invoice[Invoice Service]
    BFF --> Product[Product Service]
    BFF --> UserAccess[User Access Service]

    Authorizer --> Keycloak[Keycloak]
    Invoice --> Payment[Payment / Stripe]
    Invoice --> PDF[PDF Generator]
    Invoice --> Mail[Mail Service]
    Invoice --> Media[Media Service]

    Invoice -. async events .-> Kafka[Kafka]
    Mail -. async events .-> Kafka

    BFF --> Redis[Redis]
    Authorizer --> Redis
## Main Business Flow: Create Invoice

sequenceDiagram
    participant Client
    participant BFF
    participant Authorizer
    participant Keycloak
    participant Invoice
    participant PDF
    participant Mail

    Client->>BFF: Create invoice request
    BFF->>Authorizer: Validate access
    Authorizer->>Keycloak: Verify token
    Keycloak-->>Authorizer: Valid
    Authorizer-->>BFF: Authorized

    BFF->>Invoice: Create invoice
    Invoice->>PDF: Generate invoice PDF
    PDF-->>Invoice: PDF generated
    Invoice->>Mail: Send invoice email
    Mail-->>Invoice: Email sent
    Invoice-->>BFF: Invoice result
    BFF-->>Client: Success response
## 🌐 Tech Stack:
![NestJS](https://img.shields.io/badge/nestjs-%23E0234E.svg?style=for-the-badge&logo=nestjs&logoColor=white)
![Node.js](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)
![Redis](https://img.shields.io/badge/redis-%23DD0031.svg?style=for-the-badge&logo=redis&logoColor=white)
![gRPC](https://img.shields.io/badge/gRPC-%234F5B93.svg?style=for-the-badge&logo=grpc&logoColor=white)

## ⚙️ Installation

1. Clone the repository:
    ```bash
    git clone https://github.com/ThanhThan123/tutorial-nest-microservices.git
    ```

2. Navigate to the project directory:
    ```bash
    cd tutorial-nest-microservices
    ```

3. Install dependencies:
    ```bash
    pnpm i
    ```

4. Run the application with 8 services:
    ```bash
    pnpm dev-lite
    ```

## Monorepo Structure

Einvoice-App/
├── apps/
│   ├── authorizer/       # Auth + Keycloak integration
│   ├── bff/              # API gateway / BFF
│   ├── invoice/          # Invoice domain service
│   ├── mail/             # Mail service + templates
│   ├── media/            # Media/file service
│   ├── pdf-generator/    # PDF generation service
│   ├── product/          # Product service
│   ├── user-access/      # User & permission service
│   └── einvoice-e2e/     # End-to-end tests
│
├── libs/
│   ├── configuration/    # Shared config
│   ├── constants/        # Shared constants
│   ├── decorators/       # Custom decorators
│   ├── entities/         # Shared entities
│   ├── guards/           # Auth/permission guards
│   ├── interceptors/     # Shared interceptors
│   ├── interfaces/       # Shared interfaces / contracts
│   ├── kafka/            # Kafka integration
│   ├── middlewares/      # Shared middlewares
│   ├── observability/    # Logging / tracing / metrics
│   ├── saga-orchestration/ # Saga orchestration
│   ├── schemas/          # Shared schemas
│   └── utils/            # Utilities
│
├── docker/               # Local infra & observability config
├── tools/                # Scripts / seeders
├── .husky/
├── .nx/
├── .env
├── docker-compose.provider.yaml
├── eslint.config.mjs
├── jest.config.ts
├── nx.json
├── package.json
└── README.md

## 📜 Documentation

For detailed information on how to set up and extend the microservices, please refer to the [Documentation](docs/).


## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 💻 Contributors

If you want to contribute, please fork this repository and submit a **pull request**.
