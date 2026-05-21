# Koala FSM Platform — Design

## 1. Visão Geral

O **Koala FSM Platform** é uma plataforma SaaS de *Field Service Management* (FSM) desenvolvida para simular operações reais de gerenciamento de equipes de campo.

O projeto foi concebido utilizando conceitos de:

- Engenharia de Software
- Arquitetura em Camadas
- Pensamento Computacional
- Modelagem de Domínio
- APIs RESTful
- Controle de Fluxo Operacional

O objetivo principal é representar um ambiente corporativo realista, contendo regras de negócio, fluxo operacional controlado, gestão de usuários e execução de ordens de serviço por equipes distribuídas.

---

## 2. Objetivos Arquiteturais

O sistema foi projetado com foco nos seguintes objetivos:

| Objetivo | Descrição |
|---|---|
| Escalabilidade | Estrutura preparada para crescimento modular |
| Clareza Arquitetural | Separação explícita de responsabilidades |
| Baixo Acoplamento | Redução de dependências entre módulos |
| Regras Centralizadas | Regras de negócio concentradas na camada Service |
| Facilidade de Evolução | Possibilidade de expansão futura |
| Simulação Realista | Representação próxima de sistemas corporativos reais |

---

# 3. Decomposição do Sistema

O sistema foi dividido em módulos independentes para facilitar:

- manutenção
- escalabilidade
- reutilização
- separação de responsabilidades
- evolução incremental

---

## 3.1 Autenticação e Autorização

Responsável pelo controle de acesso ao sistema.

### Responsabilidades

- Login de usuários
- Autenticação
- Autorização baseada em roles
- Proteção de endpoints
- Controle de permissões

### Roles Existentes

| Role | Responsabilidade |
|---|---|
| ADMIN | Administração geral do sistema |
| DISPATCHER | Planejamento e despacho operacional |
| AGENT | Execução de ordens de serviço |

### Tecnologias Relacionadas

- Spring Security
- JWT (planejado)
- RBAC

---

## 3.2 Gestão de Usuários

Responsável pela administração de usuários da plataforma.

### Funcionalidades

- Cadastro de usuários
- Associação de usuários às equipes
- Controle de permissões
- Consulta de usuários
- Gerenciamento de roles

### Entidade Principal

```text
User
```

---

## 3.3 Gestão de Ordens de Serviço

Representa o núcleo operacional do sistema.

### Funcionalidades

- Criação de ordens
- Atualização de status
- Atribuição para equipes
- Reatribuição operacional
- Cancelamento
- Encerramento
- Controle do fluxo operacional

### Entidade Principal

```text
Order
```

---

## 3.4 Gestão de Equipes (WorkUnit)

Responsável pela modelagem das equipes operacionais.

### Funcionalidades

- Criação de equipes
- Associação de agentes
- Associação de veículos
- Controle de capacidade operacional
- Organização da execução em campo

### Tipos de Equipe

| Tipo | Quantidade de Membros |
|---|---|
| UMPLA | 1 membro |
| DUPLA | 2 membros |
| TRIO | 3 membros |
| SQUAD | 4 ou mais membros |

### Entidade Principal

```text
WorkUnit
```

---

## 3.5 Gestão de Veículos

Responsável pelo controle operacional de veículos utilizados pelas equipes.

### Funcionalidades

- Cadastro de veículos
- Associação com equipes
- Controle de compatibilidade operacional
- Organização logística

### Tipos de Veículo

| Tipo | Observação |
|---|---|
| MOTO | Apenas equipes UMPLA |
| PADRAO | Compatível com múltiplos tipos |
| PICKUP | Restrição para equipes grandes |
| CAMINHAO | Compatível com equipes amplas |

### Entidade Principal

```text
Vehicle
```

---

# 4. Abstração

O sistema abstrai operações reais de empresas de serviço em campo.

A modelagem foi construída buscando representar entidades e comportamentos comuns encontrados em sistemas corporativos de operação logística e manutenção externa.

---

## 4.1 Entidades Principais

### User

Representa usuários autenticados da plataforma.

#### Atributos Principais

| Campo | Tipo | Descrição |
|---|---|---|
| id | Long | Identificador único |
| name | String | Nome do usuário |
| email | String | E-mail de acesso |
| role | Enum | Papel do usuário |

---

### Order

Representa uma ordem de serviço operacional.

#### Atributos Principais

| Campo | Tipo | Descrição |
|---|---|---|
| id | Long | Identificador da ordem |
| customerName | String | Nome do cliente |
| customerPhone | String | Telefone do cliente |
| address | String | Endereço de atendimento |
| status | Enum | Estado atual da ordem |
| priority | Enum | Prioridade operacional |
| type | Enum | Tipo da ordem |

---

### WorkUnit

Representa uma equipe operacional.

#### Atributos Principais

| Campo | Tipo | Descrição |
|---|---|---|
| id | Long | Identificador da equipe |
| name | String | Nome da equipe |
| type | Enum | Tipo operacional |
| members | List<User> | Integrantes da equipe |
| vehicle | Vehicle | Veículo associado |

---

### Vehicle

Representa um veículo operacional.

#### Atributos Principais

| Campo | Tipo | Descrição |
|---|---|---|
| id | Long | Identificador do veículo |
| model | String | Modelo |
| plate | String | Placa |
| color | String | Cor |
| type | Enum | Tipo do veículo |

---

# 5. Reconhecimento de Padrões

O projeto utiliza padrões comuns encontrados em aplicações corporativas modernas.

---

## 5.1 Arquitetura em Camadas

A aplicação segue o padrão:

```text
Controller → Service → Repository → Database
```

### Responsabilidades por Camada

| Camada | Responsabilidade |
|---|---|
| Controller | Recebimento de requisições HTTP |
| Service | Regras de negócio |
| Repository | Persistência de dados |
| Database | Armazenamento |

---

## 5.2 REST API

O backend foi construído utilizando princípios RESTful.

### Exemplos de Endpoints

```http
GET    /orders
POST   /orders
PATCH  /orders/{id}/assign
PATCH  /orders/{id}/cancel
PATCH  /orders/{id}/status
```

### Características

- Stateless
- JSON como formato principal
- Separação semântica de endpoints
- Padronização de respostas

---

## 5.3 Dependency Injection

Utilização do Spring Framework para gerenciamento de dependências através de:

- IoC (*Inversion of Control*)
- Dependency Injection
- Gerenciamento de Beans

### Objetivos

- Redução de acoplamento
- Facilidade de testes
- Maior modularidade

---

## 5.4 RBAC (Role-Based Access Control)

Controle de permissões baseado em papéis de usuário.

### Regras de Permissão

| Role | Permissões |
|---|---|
| Dispatcher | Criar e atribuir ordens |
| Agent | Executar ordens |
| Admin | Administração geral |

---

## 5.5 Workflow de Estados

O sistema utiliza fluxo controlado de estados para ordens de serviço.

### Fluxo Principal

```text
CRIADA
→ DESPACHADA
→ A_CAMINHO
→ NO_LOCAL
→ EM_EXECUCAO
→ CONCLUIDA
→ FINALIZADA
```

### Fluxo Alternativo

```text
→ CANCELADA
```

### Objetivo

Garantir consistência operacional e impedir transições inválidas.

---

# 6. Algoritmos e Regras de Negócio

As regras de negócio representam o núcleo funcional da aplicação.

---

## 6.1 Validação de Status

A aplicação impede transições inválidas de estados.

### Exemplo

```text
CRIADA → FINALIZADA
```

A transição acima é considerada inválida.

### Estratégia

Validação centralizada na camada Service.

---

## 6.2 Controle de Equipes

O sistema valida:

- Quantidade máxima de membros
- Compatibilidade entre equipe e veículo
- Consistência operacional

### Exemplo

```text
Uma equipe SQUAD não pode utilizar uma motocicleta.
```

---

## 6.3 Controle de Edição

Ordens só podem ser editadas enquanto estiverem no estado:

```text
CRIADA
```

Após despacho ou execução, alterações são bloqueadas.

---

## 6.4 Controle de Reatribuição

Apenas usuários do tipo:

```text
DISPATCHER
```

podem reatribuir ordens para outras equipes.

---

# 7. Escalabilidade

O sistema foi projetado considerando crescimento futuro.

---

## Estratégias Utilizadas

| Estratégia | Objetivo |
|---|---|
| API Stateless | Escalabilidade horizontal |
| Paginação | Redução de carga |
| Arquitetura em Camadas | Organização e manutenção |
| Separação de Responsabilidades | Modularidade |
| Services Centralizados | Facilidade de evolução |

---

## Evoluções Futuras Planejadas

- Multi-tenant
- Filas assíncronas
- Observabilidade
- Microsserviços
- Cache distribuído

---

# 8. Tecnologias Utilizadas

| Tecnologia | Finalidade |
|---|---|
| Java 21 | Linguagem principal |
| Spring Boot | Backend |
| Spring Data JPA | Persistência |
| Hibernate | ORM |
| MySQL | Banco de dados |
| Swagger/OpenAPI | Documentação |
| Docker | Containerização |
| Angular | Frontend (planejado) |

---

# 9. Considerações Arquiteturais

O projeto prioriza:

- Clareza arquitetural
- Regras de negócio centralizadas
- Separação de responsabilidades
- Baixo acoplamento
- Facilidade de manutenção
- Escalabilidade futura

---

## Estratégia de Desenvolvimento

O sistema está sendo desenvolvido incrementalmente, priorizando:

1. Modelagem de domínio
2. Regras de negócio
3. API REST
4. Segurança
5. Integração frontend
6. Evoluções futuras

---

# 10. Considerações Finais

O Koala FSM Platform busca representar um sistema corporativo realista de gerenciamento de operações em campo, indo além de um CRUD tradicional.

A aplicação foi concebida com foco em:

- regras reais de negócio
- arquitetura organizada
- clareza estrutural
- evolução incremental
- boas práticas de engenharia de software

O projeto também serve como ambiente de estudo e aplicação prática de tecnologias modernas do ecossistema Java e Spring.
