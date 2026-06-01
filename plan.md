# Architects 
É um projeto de estudo e prova de casos, o objetivo é utilizar serviços cloud e infraestrutura como código.


### 1. Autenticação JWT (opcional: Amazon Cognito)
- **Cliente** → **POST** (junto com um token JWT).
- Existe um serviço da Amazon, o **Cognito** que podemos estudar a viabilidade de implantar.  

### 2. Lambda API
- Após validação, a requisição chega à **Lambda** - Uma aplicação que pode ser em Java ou Python.


### 3. Fila SQS e Worker Assíncrono
- O **SQS** atua como um buffer desacoplador.
- A fila dispara (trigger) a **Lambda Worker** assim que uma nova mensagem chega.  

### 4. CI/CD com GitHub Actions
O repositório GitHub contém três workflows principais:
- **tests.yml** (executado em PRs):  
  - Roda testes unitários   
  - Roda testes de integração 
- **build-and-push.yml**:  
  - Constrói a imagem Docker da aplicação (Spring Boot).  
  - Utiliza um Dockerfile multi-stage otimizado para Lambda.  
- **deploy.yml**:  
  - Atualiza as duas funções Lambda (API e Worker) com a nova imagem do ECR.  

### 5. Infraestrutura como Código com Terraform
- Toda a infraestrutura AWS é provisionada via **Terraform** (declarativo e versionado).
- O Terraform também configura permissões granulares.

### 6. Desenvolvimento Local com [LocalStack](https://github.com/localstack/localstack)
Para evitar custos durante o desenvolvimento, o projeto inclui um **docker-compose.yml** com:
- [**LocalStack**](https://github.com/localstack/localstack) (emula S3, SQS, Lambda, Cognito localmente)
- Containers auxiliares (para testes)

Dessa forma, o desenvolvedor pode rodar a aplicação Spring Boot apontando para `localhost:4566` (LocalStack) e validar o fluxo completo sem tocar em recursos reais da AWS.

---

## Considerações sobre a Free Tier e Custos
Pensando em manter a camada gratuita da AWS segundo uma IA temos:
- **SQS**: 1 milhão de requisições por mês grátis. (Para um projeto que não se pretende ser produto tá ótimo.)
- **Cognito**: 10 mil usuários ativos mensais (MAU) grátis.(Confesso que não pesquisei o suficiente)
- **Lambda**: 1 milhão de requisições por mês grátis.(isso é mais que o suficiente)
- **S3**: 5 GB de armazenamento grátis (primeiros 12 meses).(esse me preocupa, acho que eu e vc já temos conta...)

O uso de **LocalStack** no ambiente de desenvolvimento evita custos desnecessários.

---

## Resumo Visual do Fluxo

| Ordem | Ação                                             | Ator/Componente          |
|-------|--------------------------------------------------|--------------------------|
| 1     | Enviar imagem + JWT                             | Cliente → Cognito        |
| 2     | Validar token e chamar Lambda API               | Cognito → Lambda API     |
| 3     | Armazenar original no S3 e publicar na SQS      | Lambda API               |
| 4     | Disparar Lambda Worker (mensagem na fila)       | SQS → Lambda Worker      |
| 5     | Ler original, gerar thumbnail, salvar no S3     | Lambda Worker → S3       |
| 6     | (CI/CD) Build, teste, push imagem para ECR      | GitHub Actions → ECR     |
| 7     | (CI/CD) Deploy atualizando as Lambdas           | GitHub Actions → Lambda  |
| 8     | Provisionar tudo via Terraform                  | Terraform → AWS          |
| 9     | Desenvolvimento local com LocalStack            | Dev → docker-compose.yml |