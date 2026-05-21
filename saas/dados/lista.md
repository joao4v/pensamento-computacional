# Representação de Dados — Estrutura de Lista

# Koala FSM Platform — Representação Linear de Dados

## Objetivo

Representar estruturas de dados do tipo lista utilizadas no contexto operacional do sistema Koala FSM Platform.

A estrutura de lista é utilizada para armazenar coleções sequenciais de elementos relacionados às operações do sistema.

---

# 1. Lista de Ordens de Serviço

## Representação Conceitual

```text
[
  Ordem #1001,
  Ordem #1002,
  Ordem #1003,
  Ordem #1004
]
```

---

## Exemplo com Dados do Sistema

```text
[
  {
    id: 1001,
    cliente: "Carlos Henrique",
    status: "CRIADA"
  },

  {
    id: 1002,
    cliente: "Marina Alves",
    status: "DESPACHADA"
  },

  {
    id: 1003,
    cliente: "Fernanda Souza",
    status: "EM_EXECUCAO"
  }
]
```

---

# 2. Lista de Agentes em uma Equipe

## Representação

```text
WorkUnit Alpha Team

[
  João Victor,
  Lucas Martins,
  Rafael Costa
]
```

---

## Aplicação

A lista representa os membros associados a uma equipe operacional.

---

# 3. Lista de Ordens Atribuídas a uma Equipe

## Representação

```text
WorkUnit Bravo Team

[
  Ordem #2001,
  Ordem #2002,
  Ordem #2003
]
```

---

## Aplicação

Permite controlar as ordens atualmente planejadas para execução por determinada equipe.

---

# 4. Lista de Veículos Disponíveis

## Representação

```text
[
  Moto - ABC1D23,
  Pickup - FGH4I56,
  Caminhão - XYZ9K87
]
```

---

# 5. Lista de Status Permitidos

## Representação

```text
[
  CRIADA,
  DESPACHADA,
  A_CAMINHO,
  NO_LOCAL,
  EM_EXECUCAO,
  CONCLUIDA,
  FINALIZADA,
  CANCELADA
]
```

---
