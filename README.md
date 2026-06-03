# ☁️ Projeto Archtects

[![Java](https://img.shields.io/badge/Java-25-blue.svg)](https://openjdk.org/projects/jdk/25/)
[![Docker](https://img.shields.io/badge/Docker-required-2496ED?logo=docker)](https://www.docker.com/)
[![LocalStack](https://img.shields.io/badge/LocalStack-cloud--emulator-orange)](https://localstack.cloud/)

## 📑 Sumário

- [Descrição](#descrição)
- [Tecnologias](#tecnologias)
- [Instalação](#instalação)
  - [Java 25](#java-25)
  - [Docker](#docker)
  - [LocalStack](#localstack)
- [Fluxo de trabalho do repositório](#fluxo-de-trabalho-do-repositório)
- [Como contribuir](#como-contribuir)
- [Assinando commits](#assinando-commits)

---

## Descrição

Este repositório é um projeto prático feito por desenvolvedores e para desenvolvedores aplicarem conhecimentos em **serviços cloud** e **infraestrutura como código** (IaC).  

## Tecnologias

- **Java 25** – linguagem principal do projeto
- **Docker** (Linux ou WSL) – containerização
- **LocalStack** – emulação local de serviços AWS (S3, SQS, Lambda, etc.)

## Instalação

### Java 25

#### No Linux/WSL (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install openjdk-25-jdk
```
Para outras distribuições ou instalação manual, consulte a [documentação oficial do JDK 25.](https://docs.oracle.com/en/java/javase/25/index.html)

### Docker 

Esse projeto usa a tecnologia Docker, caso seja usuário de linux, basta executar o comando de instalação da sua distro. Para maiores informações acesse a [documentação oficial aqui.](https://docs.docker.com/engine/install/)

#### Docker + WSL

Caso seja usuário de Windows, você pode [usar o WSL.](https://learn.microsoft.com/pt-br/windows/wsl/install)

Para instalar e usar o Docker em uma distribuição Linux rodando no WSL 2, o recomendado é instalar o Docker Desktop no Windows e habilitar a integração com o WSL 2. Não é necessário instalar o Docker Engine diretamente dentro da sua distribuição Linux no WSL. Veja o passo a passo:

1.  Desinstale qualquer Docker Engine/CLI instalado diretamente no WSL
Antes de instalar o Docker Desktop, remova qualquer instalação do Docker feita manualmente dentro da sua distribuição WSL para evitar conflitos.
2. Instale o Docker Desktop no Windows:
    - Baixe o instalador do Docker Desktop para Windows.
    - Execute o instalador e, durante a instalação, selecione a opção Use WSL 2 instead of Hyper-V (caso disponível).
    - Siga as instruções até concluir a instalação.
3. Habilite o WSL 2 e a integração com sua distribuição
    - Certifique-se de que sua distribuição está rodando em modo WSL 2. Para verificar, execute:
        > wsl.exe -l -v
    - Para converter para WSL 2, use:
        > wsl.exe --set-version <nome-da-distro> 2
    - Abra o Docker Desktop, vá em Settings > Resources > WSL Integration e habilite a integração para sua distribuição Linux.
4. Use o Docker na sua distribuição WSL
    - Agora, ao abrir o terminal da sua distribuição Linux no WSL, você pode usar comandos Docker normalmente, como:
        ```bash
        docker ps
        docker run hello-world
        ```
5. Observações importantes:

    - O Docker Desktop cria e gerencia um ambiente Docker próprio, acessível de todas as distribuições WSL integradas.
    - Não é necessário (nem recomendado) instalar o Docker Engine manualmente dentro do WSL.
    - Para melhor desempenho, armazene seus projetos dentro do sistema de arquivos da distribuição Linux (por exemplo, em /home/seu-usuario/projeto).

    - 📚 Fontes:
        - https://docs.docker.com/desktop/features/wsl/
        - https://docs.docker.com/desktop/setup/install/windows-install/
        - https://docs.docker.com/desktop/features/wsl/use-wsl/

## LocalStack

## Fluxo de trabalho do repositório

### Nosso Git Flow:
![Git-Flow](./docs/Git%20Flow.png)

### Regras da branch main

| Regra | Descrição |
|---|---|
| **Pull Request obrigatório** | Nenhum commit vai direto para a branch principal |
| **Commits assinados** | Todo commit deve ter assinatura verificada |
| **Sem force push** | `git push --force` é bloqueado |
| **Branch protegida contra deleção** | A branch principal não pode ser deletada |

## Como contribuir

1. Crie uma branch a partir da release atual:
```bash
git checkout -b feature/nova-funcionalidade
```
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
