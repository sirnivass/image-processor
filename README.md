# Fluxo de trabalho do repositório

Este repositório possui regras de proteção na branch padrão.

## Regras da branch principal

| Regra | Descrição |
|---|---|
| **Pull Request obrigatório** | Nenhum commit vai direto para a branch principal |
| **Commits assinados** | Todo commit deve ter assinatura verificada |
| **Sem force push** | `git push --force` é bloqueado |
| **Branch protegida contra deleção** | A branch principal não pode ser deletada |

## Como contribuir

1. Crie uma branch a partir da principal
2. Faça seus commits **assinados** (veja abaixo)
3. Abra um Pull Request

# Assinando commits 

## Assinar com chave SSH 
### 1. Gere uma chave SSH (se já não tiver uma):
> ssh-keygen -t ed25519 -C "seu@email.com"
### 2. Configure o Git para usar SSH como formato de assinatura:
> git config --global gpg.format ssh

> git config --global user.signingkey ~/.ssh/id_ed25519.pub
### 3. Ative assinatura automática em todos os commits:
> git config --global commit.gpgsign true
### 4. Adicione a chave no GitHub:

- Vá em `Settings` → `SSH and GPG keys` → `New SSH key`
- Em `Key type`, selecione `Signing Key`
- Cole o conteúdo de `~/.ssh/id_ed25519.pub`

# Ambiente Local

## Pré-requisitos
Antes de iniciar, certifique-se que possui instalado:
- Docker
- AWS CLI


### Defina variaveis de ambiente DUMMY

Os valores são genericos apenas para utilizar localstack
```
setx AWS_ACCESS_KEY_ID test
setx AWS_SECRET_ACCESS_KEY test
setx AWS_DEFAULT_REGION us-east-1
```

## Rodando Ambiente local
Na pasta raiz do projeto execute `docker-compose up -d` para subir o container localstack.

Devo a limitações da versão gratuita do localstack a persistencia de dados do serviço esta sendo feita localmente em `/my-localstack-data/*`.

**TODO:** Avaliar se existe alguma forma de forçar os dados para o volume do container ou incluir no gitignore as pastas referentes ao localstack


### Criando bucket S3 local

Para criar um bucket pode se utilizar o seguinte comando
`aws --endpoint-url=http://localhost:4566 s3 mb s3://meu-bucket-local`
