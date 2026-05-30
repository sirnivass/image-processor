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