# Koala FSM Platform — Desafios e Soluções

# 1. Introdução

O desenvolvimento do Koala FSM Platform envolve desafios comuns encontrados em sistemas corporativos de larga escala, especialmente aplicações voltadas para operações de campo, logística e gestão operacional.

Durante a modelagem e implementação do sistema, foram identificados desafios relacionados a:

- modelagem de domínio
- controle operacional
- escalabilidade
- organização arquitetural
- persistência de dados
- segurança
- consistência de regras de negócio

Este documento apresenta os principais desafios identificados durante o desenvolvimento do projeto e as estratégias adotadas para solucioná-los.

---

# 2. Modelagem de Regras de Negócio

## Desafio

Representar corretamente o fluxo operacional de ordens de serviço e as permissões associadas aos diferentes tipos de usuários do sistema.

O domínio do problema envolve múltiplas entidades interagindo simultaneamente:

- usuários
- equipes operacionais
- veículos
- ordens de serviço
- estados operacionais

Além disso, diferentes usuários possuem responsabilidades específicas dentro do fluxo do sistema.

---

## Solução

As regras de negócio foram centralizadas na camada:

```text
Service
```

Essa abordagem permite:

- evitar duplicação de lógica
- reduzir acoplamento
- manter regras organizadas
- facilitar manutenção
- garantir consistência operacional

---

## Estratégias Utilizadas

| Estratégia | Objetivo |
|---|---|
| Services centralizados | Concentrar regras de negócio |
| Validação de status | Garantir fluxo operacional válido |
| Controle por roles | Restringir ações por perfil |
| Separação em camadas | Melhor organização arquitetural |

---

## Exemplo

| Role | Responsabilidade |
|---|---|
| DISPATCHER | Criar e atribuir ordens |
| AGENT | Executar ordens |
| ADMIN | Administração geral |

---

# 3. Controle de Fluxo de Status

## Desafio

Garantir que as ordens de serviço sigam um fluxo operacional válido, impedindo mudanças inconsistentes de estado.

Sem controle de transições, o sistema poderia permitir cenários inválidos, como:

```text
CRIADA → FINALIZADA
```

o que quebraria a lógica operacional da aplicação.

---

## Solução

Foi implementado um sistema de validação de transições permitidas entre estados.

As transições são controladas na camada Service através de regras explícitas de fluxo.

---

## Fluxo Principal

```text
CRIADA
→ DESPACHADA
→ A_CAMINHO
→ NO_LOCAL
→ EM_EXECUCAO
→ CONCLUIDA
→ FINALIZADA
```

---

## Fluxo Alternativo

```text
→ CANCELADA
```

---

## Estratégia de Implementação

As validações verificam:

- estado atual da ordem
- próximo estado permitido
- permissões do usuário
- contexto operacional

---

## Benefícios

- Consistência operacional
- Integridade do domínio
- Redução de estados inválidos
- Maior previsibilidade do sistema

---

# 4. Controle de Equipes Operacionais

## Desafio

Garantir que equipes operacionais respeitem:

- quantidade máxima de membros
- compatibilidade com veículos
- consistência organizacional

O sistema precisava representar cenários reais de operação em campo.

---

## Solução

Foi criada uma modelagem baseada em:

- tipos de equipe
- restrições operacionais
- validações estruturais

---

## Tipos de Equipe

| Tipo | Quantidade Permitida |
|---|---|
| UMPLA | 1 membro |
| DUPLA | 2 membros |
| TRIO | 3 membros |
| SQUAD | 4 ou mais membros |

---

## Compatibilidade de Veículos

| Veículo | Restrições |
|---|---|
| MOTO | Apenas equipes UMPLA |
| PICKUP | Restrição para equipes grandes |
| CAMINHAO | Compatível com equipes amplas |

---

## Estratégias Utilizadas

- Validação de quantidade de membros
- Verificação de compatibilidade operacional
- Regras centralizadas na camada Service

---

# 5. Escalabilidade

## Desafio

Projetar o sistema considerando crescimento futuro e aumento de complexidade operacional.

Mesmo sendo um projeto acadêmico, a aplicação foi concebida simulando cenários de sistemas corporativos reais.

---

## Solução

A arquitetura foi planejada visando:

- modularidade
- separação de responsabilidades
- facilidade de manutenção
- escalabilidade futura

---

## Estratégias Utilizadas

| Estratégia | Objetivo |
|---|---|
| API Stateless | Escalabilidade horizontal |
| Paginação de dados | Redução de carga |
| Arquitetura em camadas | Organização do sistema |
| Services desacoplados | Facilidade de evolução |
| REST API | Integração simplificada |

---

## Evoluções Futuras Planejadas

- Multi-tenant
- Microsserviços
- Cache distribuído
- Filas assíncronas
- Observabilidade

---

# 6. Segurança

## Desafio

Controlar acesso a funcionalidades sensíveis da aplicação e impedir operações não autorizadas.

---

## Solução

O sistema utiliza controle de acesso baseado em roles através do:

```text
RBAC (Role-Based Access Control)
```

---

## Tecnologias Utilizadas

- Spring Security
- Controle de roles
- Validação de permissões
- Proteção de endpoints

---

## Regras de Segurança

| Operação | Permissão |
|---|---|
| Criar ordens | DISPATCHER |
| Executar ordens | AGENT |
| Administração do sistema | ADMIN |

---

## Benefícios

- Controle granular de acesso
- Maior segurança operacional
- Redução de ações indevidas

---

# 7. Organização Arquitetural

## Desafio

Evitar:

- acoplamento excessivo
- lógica espalhada
- código duplicado
- dependências desorganizadas

---

## Solução

A aplicação foi estruturada utilizando arquitetura em camadas.

---

## Estrutura Arquitetural

```text
Controller → Service → Repository → Database
```

---

## Responsabilidades

| Camada | Responsabilidade |
|---|---|
| Controller | Recebimento de requisições |
| Service | Regras de negócio |
| Repository | Persistência |
| Database | Armazenamento |

---

## Estratégia Principal

As regras de negócio permanecem centralizadas na camada:

```text
Service
```

---

## Benefícios

- Melhor manutenção
- Clareza arquitetural
- Reutilização de código
- Facilidade de testes

---

# 8. Persistência de Dados

## Desafio

Gerenciar relacionamentos complexos entre entidades do domínio.

O sistema possui múltiplas relações entre:

- usuários
- equipes
- ordens
- veículos

---

## Solução

Foi utilizada a stack de persistência do ecossistema Spring:

- Spring Data JPA
- Hibernate
- MySQL

---

## Relacionamentos Utilizados

| Relacionamento | Uso |
|---|---|
| @OneToMany | Relações de coleção |
| @ManyToOne | Associação principal |
| @ManyToMany | Associação entre equipes e membros |

---

## Benefícios

- Redução de código boilerplate
- Mapeamento objeto-relacional simplificado
- Facilidade de manutenção

---

# 9. Simulação de Ambiente Real

## Desafio

Construir um projeto que representasse cenários corporativos reais de operação em campo.

O objetivo não era apenas criar um CRUD tradicional, mas sim modelar comportamentos reais encontrados em plataformas FSM.

---

## Solução

A modelagem do sistema foi baseada em:

- experiências práticas em operações FSM
- estudo de fluxos corporativos
- análise de regras operacionais
- abstração de cenários reais

---

## Aspectos Simulados

- despacho operacional
- execução em campo
- controle de equipes
- gestão de veículos
- fluxo de ordens
- permissões organizacionais

---

# 10. Considerações Finais

O desenvolvimento do Koala FSM Platform permitiu aplicar conceitos fundamentais de:

- pensamento computacional
- engenharia de software
- modelagem de domínio
- arquitetura backend
- APIs RESTful
- persistência de dados
- segurança

Os desafios encontrados durante o projeto contribuíram diretamente para a construção de uma aplicação mais próxima de sistemas corporativos reais, priorizando organização arquitetural, clareza estrutural e evolução incremental.
