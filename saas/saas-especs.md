# Koala FSM Platform  
Plataforma SaaS de Field Service Management

---

## Descrição

O Koala FSM Platform é uma aplicação backend desenvolvida com Java e Spring Boot que simula um sistema SaaS de gestão de operações de campo (Field Service Management).

O sistema permite o gerenciamento completo do ciclo de vida de ordens de serviço, desde a criação até a execução em campo por equipes, incluindo regras de negócio como atribuição, fluxo de status, controle de equipes (WorkUnit) e validações operacionais.

Este projeto foi desenvolvido como parte da disciplina Pensamento Computacional, com foco na aplicação de conceitos de engenharia de software em sistemas de larga escala.

---

## Objetivos do Projeto

- Aplicar conceitos de pensamento computacional em um sistema realista  
- Relacionar teoria de engenharia de software com prática  
- Modelar um sistema com regras de negócio  
- Desenvolver uma API REST estruturada  
- Simular um sistema SaaS escalável  

---

## Pensamento Computacional Aplicado

### Decomposição

O sistema foi dividido nos seguintes módulos:

- Autenticação e autorização  
- Gestão de usuários (Admin, Dispatcher, Agent)  
- Gestão de ordens de serviço  
- Gestão de equipes (WorkUnit)  
- Gestão de veículos  
- Execução de ordens  
- Documentação da API  

---

### Reconhecimento de Padrões

- Arquitetura em camadas (Controller → Service → Repository)  
- Controle de acesso baseado em papéis (RBAC)  
- API RESTful  
- Paginação e ordenação de dados  
- Workflow de estados  
- Injeção de dependência  

---

### Abstração

Principais entidades do sistema:

- User: representa usuários com papéis definidos  
- Order: representa ordens de serviço  
- WorkUnit: equipes responsáveis pela execução  
- Vehicle: veículos utilizados pelas equipes  

---

### Algoritmos

O sistema implementa regras como:

- Validação de transição de status  
- Controle de permissões por tipo de usuário  
- Validação de composição de equipes  
- Compatibilidade entre equipe e veículo  
- Fluxo de execução de ordens  

---

## Funcionalidades

### Ordens de Serviço

- Criação de ordens (Dispatcher)  
- Atribuição para equipes  
- Atualização de status  
- Cancelamento com restrições  
- Edição limitada por estado  

---

### Usuários

- Admin  
- Dispatcher  
- Agent  

---

### WorkUnit

- Criação de equipes  
- Controle de quantidade de membros  
- Associação com veículos  
- Atribuição de ordens  

---

### Veículos

- Cadastro de veículos  
- Tipos: Moto, Padrão, Pickup, Caminhão  
- Validação de compatibilidade com equipes  

---

## Fluxo de Ordens

→ CRIADA
→ DESPACHADA
→ A_CAMINHO
→ NO_LOCAL
→ EM_EXECUCAO
→ CONCLUIDA
→ FINALIZADA
→ CANCELADA


---

## Regras de Negócio

- Apenas Dispatcher pode criar e atribuir ordens  
- Apenas Agent executa ordens  
- Ordem só pode ser editada em estado CRIADA  
- Cancelamento permitido antes da conclusão  
- Reatribuição controlada por estado  
- Equipes devem respeitar limites de membros  
- Veículos devem ser compatíveis com o tipo de equipe  

---

## Arquitetura

O sistema segue arquitetura em camadas:
Controller → Service → Repository → Database


---

## Tecnologias

- Java 21  
- Spring Boot  
- Spring Data JPA  
- MySQL  
- Docker  
- Swagger (OpenAPI)  

---

## Escalabilidade

O sistema foi pensado para suportar crescimento através de:

- API stateless  
- Paginação de dados  
- Separação de responsabilidades  
- Possibilidade de evolução para multi-tenant  
- Integração com serviços externos  

---

## Segurança

- Autenticação com Spring Security (em desenvolvimento)  
- Controle de acesso por roles  
- Validação de dados  
- Aplicação de princípios de segurança  

---

## Estrutura do Projeto

koala-fsm/
├── README.md
├── DESIGN.md
├── DIAGRAMS/
│
└── backend/
    └── src/


---

## Metodologia

- Scrum  
- Desenvolvimento incremental  
- Aprendizado baseado em projeto  

---

## Desafios

- Modelagem de regras de negócio  
- Controle de estados e transições  
- Consistência de dados  
- Simulação de ambiente SaaS  
- Escalabilidade  

---

## Próximos Passos de Escalabilidade

- Implementação de segurança completa  
- Integração com frontend (Angular)  
- Relatórios  
- Multi-tenant  
- Testes automatizados  

---

## Entrega

Projeto desenvolvido para a disciplina Pensamento Computacional  
Curso de Ciência da Computação 

---

## Considerações Finais

Este projeto busca ir além de um CRUD simples, focando na construção de um sistema com regras reais de negócio, arquitetura organizada e potencial de evolução para um produto completo.
