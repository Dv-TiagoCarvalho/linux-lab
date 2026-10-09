# Semana 2, dia 2 (09/10/2026)

## O que foi feito

- Fechamento da semana 1: busca dentro do `man`.
- Criação de grupos e usuários.
- Uma pasta com três níveis de acesso, usando dono, grupo e permissões.
- Processos: listar, acompanhar ao vivo e encerrar.
- Serviço nginx instalado, ligado e configurado para subir no boot.

## Comandos usados

### Achar opções no manual

| Comando | O que faz |
|---|---|
| `man ls` | Abre o manual. `/palavra` procura, `n` vai para a próxima ocorrência, `q` sai |
| `ls --help` | Resumo curto das opções |
| `ls -lt /etc \| head` | Lista por data de modificação, do mais novo para o mais antigo, só as 10 primeiras linhas |

Opções de uma letra podem ser juntadas: `ls -l -t -a` é o mesmo que `ls -lta`.

### Usuários e grupos

| Comando | O que faz |
|---|---|
| `sudo groupadd suporte` | Cria um grupo |
| `sudo useradd -m -G suporte ana` | Cria o usuário com pasta pessoal (`-m`) e já no grupo (`-G`) |
| `sudo passwd ana` | Define a senha. Sem senha, o usuário não faz login |
| `sudo usermod -aG grupo usuario` | Adiciona um usuário existente a um grupo |
| `id ana` | UID, GID e grupos do usuário |
| `getent group suporte` | Quem está no grupo |
| `su - ana` | Entra como outro usuário (`exit` volta) |
| `sudo -u ana comando` | Roda um comando como outro usuário, sem a senha dele |

Faixas de UID:

| UID | Quem usa |
|---|---|
| 0 | root |
| 1 a 999 | Contas do sistema, que rodam serviços |
| 1000 em diante | Pessoas |

### Permissões

| Comando | O que faz |
|---|---|
| `sudo chown ana:suporte /srv/projeto` | Define o dono e o grupo |
| `sudo chmod 750 /srv/projeto` | Define as permissões |
| `ls -ld /srv/projeto` | Mostra as permissões da própria pasta |

Como ler `drwxr-x---`: o `d` indica diretório, e depois vêm três blocos de três letras.

| Bloco | Vale para | Número |
|---|---|---|
| `rwx` | Dono | 7 (4 + 2 + 1) |
| `r-x` | Grupo | 5 (4 + 1) |
| `---` | Outros | 0 |

`r` (ler) vale 4, `w` (escrever) vale 2 e `x` (entrar ou executar) vale 1.

### Processos

| Comando | O que faz |
|---|---|
| `ps aux` | Foto de todos os processos. A segunda coluna é o PID |
| `ps aux \| grep nome` | Procura um processo pelo nome (a linha do próprio `grep` também aparece) |
| `top` | Processos ao vivo, os que mais consomem no topo. Sai com `q` |
| `comando &` | Roda em segundo plano e mostra o PID |
| `kill PID` | Encerra o processo |

### Serviços

| Comando | O que faz |
|---|---|
| `sudo dnf install nginx -y` | Instala o servidor web |
| `sudo systemctl start nginx` | Liga agora |
| `sudo systemctl enable nginx` | Faz ligar em todo boot |
| `systemctl status nginx` | Estado detalhado |
| `systemctl is-active nginx` | Responde só `active` ou `inactive` |
| `systemctl is-enabled nginx` | Responde só `enabled` ou `disabled` |
| `curl -I localhost` | Testa o servidor web. `200 OK` quer dizer que respondeu |
| `sudo reboot` | Reinicia o servidor |

`start` e `enable` são independentes. Sem o `enable`, o serviço não volta depois de um reinício.

## Entregas

### 1. Uma pasta, três níveis de acesso

Pasta `/srv/projeto`, dona `ana`, grupo `suporte`, permissão `750`.

| Usuário | Situação | Teste | Resultado |
|---|---|---|---|
| ana | Dona (`rwx`) | `sudo -u ana touch /srv/projeto/relatorio.txt` | Criou o arquivo |
| bruno | Grupo suporte (`r-x`) | `sudo -u bruno ls /srv/projeto` | Listou |
| bruno | Grupo suporte (`r-x`) | `sudo -u bruno touch /srv/projeto/teste.txt` | Permissão negada |
| carla | Outros (`---`) | `sudo -u carla ls /srv/projeto` | Permissão negada |

### 2. nginx subindo sozinho no boot

Depois de `enable` e `reboot`, `systemctl is-enabled nginx` respondeu `enabled` e `systemctl is-active nginx` respondeu `active`, sem `start` manual.

## Problemas encontrados

### 1. Três erros seguidos ao instalar o nginx

- **Sintoma:** `Failed to start nginx.service: Unit nginx.service not found.`
- **Causa:** erro de digitação no comando anterior (`dnf intall`). O pacote não foi instalado, então o serviço não existia.
- **Correção:** `sudo dnf install nginx -y`. Quando há vários erros em sequência, o primeiro é a causa e os outros são consequência.

### 2. "Permissão negada" que não era problema

- **Sintoma:** um dos testes da pasta `/srv/projeto` foi negado.
- **Causa:** era o teste de escrita do Bruno, que só tem leitura. O resultado estava correto.
- **Diagnóstico usado:** `ls -ld /srv/projeto` (dono, grupo e permissões) e `id bruno` (grupos do usuário). São os dois comandos para qualquer queixa de acesso negado.

### 3. Comando digitado em uma tela que não era o prompt

- **Sintoma:** os comandos não respondiam.
- **Causa:** o `top` (ou o `man`) ainda estava aberto.
- **Correção:** sair com `q`. Só dá para rodar comando quando o prompt `[usuario@host ~]$` está na tela.

## Para repetir sem consulta

Criar um usuário `diego` no grupo `financeiro`, uma pasta `/srv/financas` onde só o financeiro entra, e provar com `sudo -u` que a Ana é barrada.
