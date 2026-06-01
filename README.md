# Sumário

- [Instalação](#Instalação)
- [Fluxo de trabalho do repositório](#Fluxo-de-trabalho-do-repositório)
- [Como Contribuir](#Assinando-commits)
- [Configurar LocalStack](#Configurando-ambiente-localstack)

# Instalação

Esse projeto usa a tecnologia Docker, caso seja usuário de linux, basta executar o comando de instalação da sua distro. Para maiores informações acesse a [documentação oficial aqui.](https://docs.docker.com/engine/install/)

Caso seja usuário de Windows, você pode [usar o WSL.](https://learn.microsoft.com/pt-br/windows/wsl/install)

### Docker + WSL

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
6. Fontes:
    - https://docs.docker.com/desktop/features/wsl/
    - https://docs.docker.com/desktop/setup/install/windows-install/
    - https://docs.docker.com/desktop/features/wsl/use-wsl/

# Fluxo de trabalho do repositório

## Nosso Git Flow:
![Git-Flow](./docs/Git%20Flow.png)

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

# Configurando ambiente localstack

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
