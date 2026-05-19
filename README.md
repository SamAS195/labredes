# 🐝 Laboratório com Containerlab

Um laboratório de Observação de Protocolo de Rede

> Laboratório prático de **filtragem de pacotes em velocidade de linha** usando ** ** em um ambiente de rede virtualizado com **Containerlab**.

[![Containerlab](https://img.shields.io/badge/Containerlab-v0.50+-blue?logo=linux)](https://containerlab.dev)
[![Docker](https://img.shields.io/badge/Docker-required-blue?logo=docker)](https://www.docker.com)
[![Licença](https://img.shields.io/badge/licença-GPL--2.0-green)](LICENSE)

---

## 📖 Visão Geral

Este laboratório demonstra um recurso muito poderoso do kernel Linux: o ** **. Aqui é anexado um pequeno programa  na interface de rede, que descarta pacotes **antes mesmo que eles cheguem à pilha de rede**, tornando a filtragem praticamente "gratuita" em termos de CPU.

**O que este laboratório demonstra:**
- Compilação de um programa  em C para bytecode  usando Docker como ambiente de build.
- Deploy de uma rede virtual com 2 nós usando Containerlab.
- Carregamento de um programa  em uma interface de rede com ` `.
- Bloqueio de tráfego ICMP (ping) em velocidade de linha.
- Leitura de contadores de pacotes descartados a partir de um ** ** em tempo real.
- O laboratório disponibiliza um script (ativation-test.md) para testar o deploy e comparar o desempenho do  com o iptables.
---

## Topologia

```
┌─────────────────────────────────────────┐
│               Máquina Host              │
│                                         │
│  ┌──────────┐ eth1   eth1 ┌──────────┐  │
│  │  node-a  ├─────────────┤  node-b  │  │
│  │10.0.0.1  │             │10.0.0.2  │  │
│  └──────────┘             └──────────┘  │
│    (emissor)            ( )    │
└─────────────────────────────────────────┘
```
- node-a: Máquina Linux usando a imagem nicolaka/netshoot (distro focada em ferramentas de rede).
- node-b: Máquina Linux nicolaka/netshoot com um bind, montando o arquivo  do host diretamente para a raiz do container (/ _drop.o).


| Nó     | Endereço IP  | Função                                      |
|--------|-------------|---------------------------------------------|
| node-a | `10.0.0.1`  | Emissor de pacotes (origem do ping)         |
| node-b | `10.0.0.2`  | Filtro  — descarta pacotes ICMP          |

---

## 🔧 Pré-requisitos


### 0. Requisitos do Sistema

Os seguintes requisitos devem ser atendidos para que a ferramenta containerlab seja executada com sucesso (https://containerlab.dev/install/):

- Um usuário com privilégios de sudo para executar o containerlab.

- Um servidor Linux, pode ser WSL2 (https://learn.microsoft.com/pt-br/windows/wsl/install).

### 1. Instalar o Docker

```bash
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
```

> Saia e entre novamente na sessão após adicionar seu usuário ao grupo `docker`.

### 2. Instalar o Containerlab

```bash
bash -c "$(curl -sL https://get.containerlab.dev)"
```

Verifique a instalação:

```bash
containerlab version
```

---

## ⏬ Obtendo o Laboratório

Clone o repositório e acesse o diretório do laboratório:

```bash
git clone https://github.com/DANIELVENTORIM/ebpf-lab.git
cd ebpf-lab
```

> 📁 Arquivos principais:
> - `lab-ebpf.clab.yml` — Definição da topologia Containerlab
> - `xdp_drop.c` — Código-fonte eBPF/XDP
> - `compile.sh` — Script de compilação via Docker

---
