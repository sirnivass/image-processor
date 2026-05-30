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