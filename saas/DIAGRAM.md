# Koala FSM Platform — Diagramas e Modelagem

# 1. Introdução

Este documento apresenta os principais diagramas arquiteturais e conceituais do projeto **Koala FSM Platform**.

Os diagramas foram elaborados com o objetivo de representar:

- estrutura do domínio
- entidades principais
- relacionamentos
- fluxo operacional
- arquitetura da aplicação
- responsabilidades do sistema

A modelagem busca representar um sistema corporativo moderno de *Field Service Management (FSM)* utilizando conceitos de engenharia de software e arquitetura backend.

---

# 2. Visão Geral do Domínio

O sistema é composto por quatro entidades centrais:

| Entidade | Responsabilidade |
|---|---|
| User | Usuários do sistema |
| Order | Ordens de serviço |
| WorkUnit | Equipes operacionais |
| Vehicle | Veículos utilizados pelas equipes |

---

# 3. Diagrama de Domínio

## Objetivo

Representar os relacionamentos principais entre as entidades do sistema.

---

## Diagrama UML Simplificado

<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/a3b24b28-d4c8-4eb7-9f5b-8eeb89342e44" />

---

## Interpretação

### User

Representa usuários autenticados do sistema.

Papéis disponíveis:

- ADMIN
- DISPATCHER
- AGENT

---

### Order

Representa ordens de serviço executadas pelas equipes operacionais.

---

### WorkUnit

Representa equipes responsáveis pela execução operacional.

As equipes possuem:

- membros
- veículo associado
- ordens atribuídas

---

### Vehicle

Representa veículos utilizados nas operações de campo.

---

# 4. Fluxo Operacional de Ordens

## Objetivo

Representar o ciclo de vida operacional de uma ordem de serviço.

---

## Fluxo Principal

<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/c8b5a043-c4a7-465c-a5a0-17a9de1aec8b" />

---

## Interpretação do Fluxo

| Status | Descrição |
|---|---|
| CRIADA | Ordem criada no sistema |
| DESPACHADA | Ordem atribuída a uma equipe |
| A_CAMINHO | Equipe deslocando para atendimento |
| NO_LOCAL | Equipe presente no local |
| EM_EXECUCAO | Serviço em andamento |
| CONCLUIDA | Serviço executado |
| FINALIZADA | Ordem encerrada |
| CANCELADA | Ordem cancelada |

---

# 5. Arquitetura da Aplicação

## Objetivo

Representar a arquitetura em camadas utilizada no backend.

---

## Arquitetura em Camadas

<img width="767" height="459" alt="image" src="https://github.com/user-attachments/assets/a7120680-fd70-47c3-aa9d-613a9fe7a164" />

---

## Responsabilidades das Camadas

| Camada | Responsabilidade |
|---|---|
| Controller | Recebimento das requisições HTTP |
| Service | Regras de negócio |
| Repository | Persistência de dados |
| Database | Armazenamento |

---

# 6. Fluxo de Responsabilidades

## Objetivo

Representar como os diferentes tipos de usuários interagem com o sistema.

---

## Fluxo de Papéis

flowchart LR

A[Dispatcher] --> B[Criar Ordem]
A --> C[Atribuir Ordem]

D[Agent] --> E[Executar Ordem]
D --> F[Atualizar Status]

G[Admin] --> H[Gerenciar Sistema]

---

# 7. Relacionamentos do Sistema

## Relacionamento entre Entidades

| Relacionamento | Tipo |
|---|---|
| User ↔ WorkUnit | Many-to-Many |
| WorkUnit → Vehicle | Many-to-One |
| WorkUnit → Order | One-to-Many |

---

# 8. Regras Estruturais

## Equipes Operacionais

| Tipo de Equipe | Quantidade Permitida |
|---|---|
| UMPLA | 1 membro |
| DUPLA | 2 membros |
| TRIO | 3 membros |
| SQUAD | 4 ou mais membros |

---

## Compatibilidade de Veículos

| Veículo | Restrição |
|---|---|
| MOTO | Apenas UMPLA |
| PICKUP | Restrição para equipes grandes |
| CAMINHAO | Compatível com múltiplas equipes |
