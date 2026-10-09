# Semana 1, dia 1 (08/10/2026)

## O que foi feito

- VM criada no UTM e Rocky Linux 9.8 Minimal instalado.
- Primeiros comandos no terminal: navegação, arquivos e leitura do estado do servidor.
- Usuário comum liberado para `sudo`.
- Acesso por SSH a partir do Mac, primeiro com senha e depois com chave.

## Comandos usados

### Navegar e mexer em arquivos

| Comando | O que faz |
|---|---|
| `pwd` | Mostra a pasta atual |
| `ls -la` | Lista tudo, com detalhes e arquivos ocultos (`.` é a pasta atual, `..` é a de cima) |
| `cd /etc`, `cd ..`, `cd ~` | Entra em uma pasta, sobe um nível, volta para a pasta pessoal |
| `mkdir -p lab/semana1` | Cria pastas, incluindo as intermediárias |
| `cp`, `mv`, `rm` | Copia, move ou renomeia, apaga |
| `cat arquivo` | Mostra o conteúdo de um arquivo |
| `comando > arquivo` | Grava a saída de um comando em um arquivo |
| `grep palavra arquivo` | Mostra só as linhas que contêm a palavra |
| `wc -l arquivo` | Conta as linhas |
| `nano arquivo` | Editor de texto. Salvar: Control + O. Sair: Control + X |

### Conhecer um servidor

| Comando | O que responde |
|---|---|
| `cat /etc/os-release` | Qual é o sistema e a versão |
| `df -h` | Espaço em disco. A linha que termina em `/` é o disco principal |
| `free -h` | Memória total, usada e disponível |
| `uptime` | Há quanto tempo está ligado e a carga (load average) |
| `cat /etc/passwd` | Usuários do sistema (20 linhas, uma só de pessoa) |
| `id usuario` | UID, GID e grupos de um usuário |
| `hostname -I` | IP da máquina na rede (I maiúsculo) |

Formato de uma linha do `/etc/passwd`:

```
usuario:x:UID:GID:descricao:/home/usuario:/bin/bash
```

O `x` indica que a senha fica em `/etc/shadow`.

### Administração

| Comando | O que faz |
|---|---|
| `su -` | Vira root (pede a senha do root) |
| `usermod -aG wheel usuario` | Coloca o usuário no grupo `wheel`, que libera o `sudo` no Rocky |
| `sudo dnf install nano -y` | Instala um pacote |
| `sudo poweroff` | Desliga o servidor corretamente |

### SSH (rodados no Mac)

| Comando | O que faz |
|---|---|
| `ssh usuario@IP` | Conecta ao servidor |
| `ssh-keygen -t ed25519` | Cria o par de chaves (a privada fica no Mac, a pública vai para o servidor) |
| `ssh-copy-id usuario@IP` | Copia a chave pública para o servidor |

## Problemas encontrados

### 1. A VM voltava para o instalador depois de instalada

- **Sintoma:** depois de instalar e reiniciar, a VM abria o instalador do Rocky de novo.
- **Causa:** a ISO continuava no drive de CD, e a VM dava boot por ela em vez do disco.
- **Correção:** desligar a VM, remover a ISO do drive de CD nas configurações do UTM e ligar de novo.

### 2. `usuario não está no arquivo sudoers`

- **Sintoma:** qualquer comando com `sudo` era recusado com essa mensagem.
- **Causa:** o usuário foi criado na instalação sem a opção de administrador, então não estava no grupo `wheel`.
- **Correção:** virar root com `su -`, rodar `usermod -aG wheel usuario`, sair e entrar de novo. `sudo whoami` passou a responder `root`.

### 3. `cat/etc/passwd: Arquivo ou diretório inexistente`

- **Sintoma:** o comando `cat` não encontrava um arquivo que existe.
- **Causa:** faltou o espaço entre o comando e o arquivo. O shell procurou um programa chamado `cat/etc/passwd`.
- **Correção:** `cat /etc/passwd`. A tecla Tab completa o caminho e evita erro de digitação.

### 4. Teclado da VM com símbolos trocados

- **Sintoma:** teclas como `/`, `-` e `:` saíam erradas na tela da VM.
- **Causa:** o instalador configurou o layout `br` (ABNT2), diferente do teclado do MacBook.
- **Correção:** `sudo localectl set-keymap us`. Pelo SSH o problema não aparece, porque o teclado é o do próprio Mac.

### 5. `ssh: Could not resolve hostname`

- **Sintoma:** o `ssh` no Mac não conectava.
- **Causa:** faltou o `@` entre usuário e endereço, e o IP usado era o do Mac (`192.168.64.1`), não o da VM.
- **Correção:** descobrir o IP da VM com `hostname -I` (rodado na VM) e usar o formato `ssh usuario@IP`.

## Falta fazer na semana 1

- Praticar `man` e `--help` para achar opções sozinho.
- Repetir os comandos deste dia sem consultar as anotações.
