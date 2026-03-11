# Arquitetura de Software

## Plataforma de Roadmaps de Carreira Baseados em IA

---

# 1. Objetivo deste Documento

Este documento define a **arquitetura técnica do sistema**, incluindo:

* divisão de responsabilidades
* microserviços do backend
* comunicação entre serviços
* estrutura de repositórios
* princípios de design utilizados

A arquitetura foi projetada para garantir:

* **escalabilidade**
* **manutenção simplificada**
* **baixo acoplamento**
* **alta coesão**
* aderência aos **princípios SOLID**

---

# 2. Visão Geral da Arquitetura

A arquitetura do sistema será baseada em:

**Frontend desacoplado + Backend orientado a microserviços**

Estrutura geral:

```
Frontend (Web App)
        │
        │
API Gateway
        │
        │
Microserviços independentes
        │
        ├── User Service
        ├── Career Analysis Service
        ├── Roadmap Service
        ├── Project Generator Service
        ├── Progress Tracking Service
        ├── AI Orchestrator Service
        └── Notification Service
```

Cada serviço possui **responsabilidade única**, seguindo o **Single Responsibility Principle**.

---

# 3. Frontend

O frontend será responsável por:

* autenticação do usuário
* interface de roadmap visual
* interação com projetos
* dashboard de progresso

Tecnologias recomendadas:

* Next.js
* React
* Tailwind CSS

Para visualização de roadmaps:

* React Flow

---

# 4. API Gateway

O **API Gateway** funciona como ponto central de entrada.

Responsabilidades:

* autenticação
* rate limiting
* roteamento para microserviços
* padronização de respostas

Ele reduz o acoplamento entre frontend e serviços internos.

---

# 5. Comunicação Entre Microserviços

Comunicação híbrida:

### Comunicação síncrona

Utilizada para operações que precisam de resposta imediata.

Exemplo:

```
Frontend → API Gateway → User Service
```

Protocolos possíveis:

* REST
* GraphQL

---

### Comunicação assíncrona

Utilizada para tarefas pesadas ou demoradas.

Exemplo:

```
Career Analysis → AI Orchestrator
```

Tecnologias possíveis:

* message queues
* event streaming

Benefícios:

* desacoplamento
* melhor escalabilidade

---

# 6. Estrutura de Microserviços

A seguir estão os microserviços principais da plataforma.

---

# 6.1 User Service

Repositório:

```
career-platform-user-service
```

Responsabilidade:

Gerenciar dados de usuários.

Funções:

* cadastro
* autenticação
* gerenciamento de perfil
* armazenamento de currículo

Princípio SOLID aplicado:

Single Responsibility.

Esse serviço lida exclusivamente com **dados do usuário**.

---

# 6.2 Career Analysis Service

Repositório:

```
career-platform-career-analysis
```

Responsabilidade:

Analisar objetivos de carreira e identificar lacunas de habilidades.

Funções:

* interpretar vaga ou objetivo profissional
* extrair habilidades exigidas
* comparar com perfil do usuário
* gerar lista de skill gaps

Esse serviço não gera roadmaps, apenas **analisa dados de carreira**.

---

# 6.3 Roadmap Service

Repositório:

```
career-platform-roadmap-service
```

Responsabilidade:

Gerar e gerenciar roadmaps de aprendizado.

Funções:

* criar roadmaps
* organizar habilidades em ordem lógica
* armazenar roadmap
* fornecer roadmap ao frontend

Esse serviço não realiza análise de carreira nem gera projetos.

---

# 6.4 Project Generator Service

Repositório:

```
career-platform-project-generator
```

Responsabilidade:

Gerar projetos práticos para cada etapa do roadmap.

Funções:

* sugerir projetos
* definir requisitos
* gerar checklist de desenvolvimento

Esse serviço ajuda a validar aprendizado prático.

---

# 6.5 Progress Tracking Service

Repositório:

```
career-platform-progress-service
```

Responsabilidade:

Gerenciar progresso do usuário.

Funções:

* registrar habilidades concluídas
* registrar projetos concluídos
* calcular progresso total
* fornecer dados para dashboard

Esse serviço não cria conteúdo, apenas **monitora progresso**.

---

# 6.6 AI Orchestrator Service

Repositório:

```
career-platform-ai-orchestrator
```

Responsabilidade:

Centralizar chamadas para modelos de IA.

Funções:

* enviar prompts
* gerenciar respostas
* otimizar chamadas de IA
* controlar custos

Esse serviço evita que cada microserviço chame a IA diretamente.

Benefícios:

* melhor controle de custo
* padronização de prompts
* logging centralizado

---

# 6.7 Notification Service

Repositório:

```
career-platform-notification-service
```

Responsabilidade:

Gerenciar comunicação com usuários.

Funções:

* envio de emails
* notificações de progresso
* alertas de novos projetos
* lembretes de estudo

---

# 7. Banco de Dados

Cada microserviço pode possuir seu próprio banco de dados.

Benefícios:

* isolamento de dados
* menor acoplamento
* maior escalabilidade

Tecnologia recomendada:

* PostgreSQL

---

# 8. Estrutura de Repositórios

Organização sugerida:

```
career-platform-frontend

career-platform-api-gateway

career-platform-user-service

career-platform-career-analysis

career-platform-roadmap-service

career-platform-project-generator

career-platform-progress-service

career-platform-ai-orchestrator

career-platform-notification-service
```

Cada repositório deve possuir:

* documentação
* testes
* pipelines de CI/CD

---

# 9. Princípios SOLID Aplicados

## Single Responsibility

Cada microserviço possui uma única responsabilidade.

---

## Open/Closed Principle

Serviços podem ser estendidos sem modificar sua lógica central.

Exemplo:

Adicionar novos tipos de análise de carreira sem alterar serviços existentes.

---

## Liskov Substitution

Interfaces entre serviços permitem substituição de implementações.

---

## Interface Segregation

Serviços expõem apenas APIs necessárias.

---

## Dependency Inversion

Serviços dependem de abstrações.

Exemplo:

Serviços dependem de uma interface de IA, não de um provedor específico.

---

# 10. Escalabilidade

Essa arquitetura permite escalar serviços individualmente.

Exemplo:

Se a geração de roadmaps crescer muito:

```
roadmap-service
```

pode escalar independentemente.

---

# 11. Segurança

Práticas recomendadas:

* autenticação centralizada
* tokens JWT
* rate limiting
* logs de auditoria

---

# 12. Observabilidade

Ferramentas de monitoramento devem ser implementadas para:

* logs
* métricas
* rastreamento de requisições

Isso facilita manutenção e diagnóstico.

---

# 13. Benefícios da Arquitetura

Essa arquitetura oferece:

* modularidade
* escalabilidade
* manutenção simplificada
* separação clara de responsabilidades
* facilidade para adicionar novas funcionalidades

---

# 14. Conclusão

A divisão em microserviços permite que o sistema evolua de forma modular e escalável.

Cada serviço possui um domínio claro e responsabilidades bem definidas, o que reduz complexidade e facilita o desenvolvimento colaborativo.

Essa arquitetura também prepara o sistema para crescimento futuro, permitindo integração com novos serviços e funcionalidades sem comprometer a estabilidade da plataforma.
