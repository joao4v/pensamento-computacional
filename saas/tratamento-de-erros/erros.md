# Koala FSM Platform — Erros Identificados

---

# 1. Introdução

Durante o desenvolvimento do projeto Koala FSM Platform foram identificados problemas relacionados à:

- modelagem de domínio
- organização arquitetural
- persistência de dados
- fluxo operacional
- estrutura de responsabilidades

Este documento registra os principais erros encontrados e os impactos observados.

---

# 2. Uso Inicial Incorreto de Relacionamentos

## Problema

Inicialmente, as ordens de serviço estavam sendo associadas diretamente ao agente responsável.

### Modelagem Inicial

```text
Order → Agent
```

---

## Impacto

Essa abordagem gerava limitações operacionais:

- baixa flexibilidade
- dificuldade de representar equipes
- inconsistência com operações reais
- acoplamento excessivo

---

## Problema Arquitetural

O modelo não representava corretamente o domínio do problema, já que ordens são executadas por equipes operacionais e não apenas por indivíduos.

---

# 3. Ausência de Camada de Regras Centralizadas

## Problema

Algumas regras estavam inicialmente sendo consideradas diretamente nos controllers ou entidades.

---

## Impacto

Isso poderia gerar:

- duplicação de lógica
- alto acoplamento
- baixa manutenibilidade
- inconsistência de regras

---

## Exemplo

Validações de status e permissões operacionais não possuíam local centralizado.

---

# 4. Uso Inadequado de Field Injection

## Problema

Inicialmente foi considerado o uso de:

```java
@Autowired
private Service service;
```

---

## Impacto

Essa abordagem dificulta:

- testes
- imutabilidade
- clareza de dependências
- manutenção futura

---

# 5. Persistência Inconsistente com Optional

## Problema

Durante implementação com Spring Data JPA, houve dificuldade no tratamento correto de:

```java
Optional<T>
```

---

## Exemplo Problemático

```java
if (tempOrder == null)
```

---

## Impacto

O método `findById()` nunca retorna `null`.

Isso poderia causar:

- falhas lógicas
- tratamento incorreto de ausência de dados

---

# 6. Uso Prematuro de Spring Data REST

## Problema

Foi considerado utilizar Spring Data REST para exposição automática de endpoints.

---

## Impacto

Apesar de simplificar CRUDs básicos, a abordagem reduziria:

- controle das regras de negócio
- clareza arquitetural
- personalização operacional
- controle do domínio

---

# 7. Modelagem Simplificada de Usuários

## Problema

Inicialmente foi considerado criar entidades separadas para:

- Agent
- Dispatcher

---

## Impacto

Isso aumentaria:

- duplicação estrutural
- complexidade desnecessária
- dificuldade de manutenção

---

## Problema Conceitual

Os usuários compartilham atributos comuns e se diferenciam principalmente por permissões.

---

# 8. Ausência Inicial de Controle de Fluxo

## Problema

O fluxo de estados das ordens não possuía validação formal.

---

## Impacto

Isso permitiria:

- transições inválidas
- inconsistência operacional
- quebra das regras de negócio

---

## Exemplo Inválido

```text
CRIADA → FINALIZADA
```

---

# 9. Acoplamento Excessivo de Responsabilidades

## Problema

No início do desenvolvimento, algumas entidades estavam acumulando responsabilidades excessivas.

---

## Impacto

Isso poderia gerar:

- baixa coesão
- dificuldade de evolução
- lógica espalhada
- código difícil de manter

---

# 10. Considerações

Os problemas identificados refletem desafios comuns encontrados durante o desenvolvimento de sistemas corporativos.

A identificação desses pontos permitiu evoluir a arquitetura da aplicação e aplicar melhores práticas de engenharia de software.

# Koala FSM Platform — Projeto Corrigido

---

# 1. Introdução

Após análise dos problemas identificados durante o desenvolvimento do sistema, foram aplicadas melhorias arquiteturais e estruturais visando:

- melhor representação do domínio
- redução de acoplamento
- centralização de regras
- aumento da clareza do sistema
- preparação para escalabilidade futura

---

# 2. Correção da Modelagem Operacional

## Problema Anterior

As ordens eram associadas diretamente aos agentes.

```text
Order → Agent
```

---

## Solução Aplicada

As ordens passaram a ser associadas às equipes operacionais.

```text
Order → WorkUnit → Users
```

---

## Justificativa

Essa abordagem representa melhor operações reais de Field Service Management.

---

## Benefícios

- maior flexibilidade
- representação mais realista
- suporte a equipes dinâmicas
- redução de acoplamento

---

# 3. Centralização de Regras na Camada Service

## Problema Anterior

Validações estavam dispersas entre controllers e entidades.

---

## Solução Aplicada

As regras de negócio foram centralizadas na camada:

```text
Service
```

---

## Justificativa

A camada Service possui responsabilidade adequada para:

- regras operacionais
- validações
- controle de fluxo
- permissões

---

## Benefícios

- maior organização
- menor duplicação
- melhor manutenção
- maior clareza arquitetural

---

# 4. Adoção de Constructor Injection

## Problema Anterior

Uso potencial de:

```java
@Autowired
private Service service;
```

---

## Solução Aplicada

Utilização de:

```java
public OrderService(OrderRepository repository)
```

---

## Justificativa

Constructor Injection melhora:

- imutabilidade
- clareza das dependências
- testabilidade

---

# 5. Correção do Tratamento com Optional

## Problema Anterior

Validação incorreta utilizando:

```java
if (tempOrder == null)
```

---

## Solução Aplicada

Uso correto de:

```java
Optional<Order>
```

com:

```java
orElseThrow()
```

---

## Justificativa

O Spring Data JPA retorna Optional para representar ausência segura de dados.

---

## Benefícios

- código mais seguro
- menor chance de NullPointerException
- maior clareza semântica

---

# 6. Controle Formal de Fluxo de Status

## Problema Anterior

As ordens não possuíam validação formal de transição.

---

## Solução Aplicada

Implementação de regras explícitas de fluxo operacional.

---

## Fluxo Implementado

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

## Benefícios

- consistência operacional
- previsibilidade
- integridade do domínio

---

# 7. Simplificação da Modelagem de Usuários

## Problema Anterior

Possibilidade de múltiplas entidades específicas.

---

## Solução Aplicada

Criação de uma entidade única:

```text
User
```

com controle por:

```text
Role
```

---

## Justificativa

Os usuários compartilham estrutura semelhante e diferem principalmente por permissões.

---

## Benefícios

- redução de duplicação
- maior simplicidade
- melhor manutenção

---

# 8. Controle Operacional de Equipes

## Solução Aplicada

Foram implementadas validações para:

- quantidade máxima de membros
- compatibilidade de veículos
- consistência operacional

---

## Exemplo

```text
Moto → apenas equipes UMPLA
```

---

# 9. Considerações Arquiteturais

O projeto corrigido passou a priorizar:

- baixo acoplamento
- separação de responsabilidades
- clareza estrutural
- modularidade
- escalabilidade futura

---

