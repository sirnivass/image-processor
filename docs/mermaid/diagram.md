https://mermaid.live

```mermaid
graph TD
    Client[Cliente] -->|POST /images com JWT| Cognito[Amazon Cognito]
    Cognito -->|Valida token| APILambda[Lambda API<br/>Spring Boot]

    APILambda -->|Upload da imagem| S3Upload[S3 Bucket<br/>Imagens originais]
    APILambda -->|Enfileira job| SQS[SQS Fila<br/>Geração de thumbnail]

    SQS -->|Trigger assíncrono| WorkerLambda[Lambda Worker<br/>Geração de thumbnail]
    WorkerLambda -->|Lê imagem original| S3Upload
    WorkerLambda -->|Gera e salva thumbnail| S3Thumb[S3 Bucket<br/>Thumbnails]

    subgraph CI/CD
        GitHub[GitHub Actions] -->|Build & push| ECR[Amazon ECR]
        ECR -->|Imagem Docker| APILambda
        ECR -->|Imagem Docker| WorkerLambda
    end

    subgraph Infra as Code
        Terraform[Terraform] -->|Provisiona| Cognito
        Terraform -->|Provisiona| S3Upload
        Terraform -->|Provisiona| S3Thumb
        Terraform -->|Provisiona| SQS
        Terraform -->|Provisiona| ECR
        Terraform -->|Provisiona| APILambda
        Terraform -->|Provisiona| WorkerLambda
        Terraform -->|Configura| IAM[IAM OIDC]
    end

    subgraph Desenvolvimento
        LocalStack[LocalStack] -->|Simula AWS local| APILambda
        LocalStack -->|Simula AWS local| WorkerLambda
    end
    ``