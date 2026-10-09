# linux-lab

Laboratório de estudos de administração de servidores Linux. É um ambiente de estudo, montado para praticar as tarefas de um analista Linux júnior.

## Ambiente

| Item | Configuração |
|---|---|
| Host | MacBook Pro (Apple Silicon, M3 Pro) |
| Virtualização | UTM (QEMU, modo Virtualizar) |
| Sistema | Rocky Linux 9.8 Minimal, ARM64 (aarch64), sem interface gráfica |
| VM | 4 GB de RAM, 4 núcleos, disco de 20 GB com LVM |
| Acesso | SSH com chave ed25519 a partir do Terminal do macOS |

## Plano de estudos

| Semana | Tema | Situação |
|---|---|---|
| 1 | VM, terminal e SSH | Em andamento |
| 2 | Usuários, permissões, processos e serviços | A fazer |
| 3 | Discos, LVM, rede e firewall | A fazer |
| 4 | Logs, troubleshooting e causa raiz | A fazer |
| 5 | Backup, patches e hardening | A fazer |
| 6 | ITIL/GMUD, revisão e refazer tudo do zero | A fazer |

## Como as anotações estão organizadas

Uma pasta por semana e um arquivo por dia de estudo. Cada problema encontrado é registrado em três linhas: **sintoma**, **causa** e **correção**.

- [Semana 1, dia 1](semana-1/dia-01.md): instalação, primeiros comandos e SSH
