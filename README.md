# Order Management API

Uma API de Gestão de Pedidos desenvolvida com o objetivo de estudar padrões e boas práticas amplamente utilizados no ecossistema .NET.

O projeto foi criado para aplicar na prática conceitos como **CQRS**, **MediatR**, **FluentValidation** e **Pipeline Behaviors**, mantendo uma arquitetura simples, organizada e de fácil entendimento.

---

## Objetivo

Este projeto tem como principal objetivo compreender o fluxo completo de uma requisição utilizando o **MediatR**, desde a entrada na API até o acesso aos dados, aplicando conceitos de separação de responsabilidades, baixo acoplamento e organização por funcionalidades.

Além de servir como material de estudo, o projeto também será evoluído continuamente para explorar novas práticas e padrões utilizados em aplicações .NET.

---

## Tecnologias

- ASP.NET Core
- C#
- MediatR
- FluentValidation
- Swagger / OpenAPI
- Docker
- PostgreSQL

---

## Conceitos abordados

### CQRS (Command Query Responsibility Segregation)

O projeto separa operações de escrita e leitura.

### Commands

Responsáveis por alterar o estado da aplicação.

- CreateOrder
- CancelOrder

### Queries

Responsáveis apenas pela leitura dos dados.

- GetOrderById
- ListOrders

Essa separação torna a aplicação mais organizada e facilita sua evolução conforme novas funcionalidades são adicionadas.

---

### MediatR

O MediatR atua como intermediador entre os Controllers e os Handlers.

Ao invés do Controller conhecer regras de negócio, serviços ou repositórios, ele apenas envia uma Request para o MediatR.

```
Controller
      │
      ▼
Mediator.Send()
      │
      ▼
Handler
```

Essa abordagem reduz o acoplamento entre as camadas da aplicação, melhora a organização do código e facilita testes e manutenção.

---

### FluentValidation

Toda Request passa por uma etapa de validação antes de chegar ao Handler.

Alguns exemplos de regras:

- Nome do cliente obrigatório
- Pedido deve possuir pelo menos um item
- Quantidade dos itens deve ser maior que zero

Caso alguma validação falhe, a execução é interrompida e o Handler não é executado.

---

### Pipeline Behaviors

Foram implementados três Pipeline Behaviors para demonstrar responsabilidades transversais da aplicação.

### ValidationBehavior

Executa automaticamente todos os Validators registrados para a Request antes que ela seja processada.

Caso alguma validação falhe, o fluxo é interrompido imediatamente.

---

### LoggingBehavior

Responsável por registrar informações importantes sobre cada Request, facilitando auditoria, monitoramento e depuração da aplicação.

---

### PerformanceBehavior

Mede o tempo de execução de cada Request, permitindo identificar possíveis gargalos de performance.

---

## Fluxo da Requisição

Toda requisição percorre o seguinte fluxo:

```
Cliente
      │
      ▼
Controller
      │
      ▼
MediatR
      │
      ▼
ValidationBehavior
      │
      ▼
LoggingBehavior
      │
      ▼
PerformanceBehavior
      │
      ▼
Handler
      │
      ▼
Repository
      │
      ▼
Banco de Dados
```

---

## Ordem de execução

Quando uma requisição chega à API, a seguinte sequência é executada:

1. O Controller recebe a requisição HTTP.
2. O Controller envia a Request para o MediatR.
3. O MediatR identifica o Handler responsável.
4. O ValidationBehavior executa todas as validações.
5. O LoggingBehavior registra informações da Request.
6. O PerformanceBehavior inicia a medição do tempo.
7. O Handler executa a regra de negócio.
8. O Repository realiza o acesso aos dados.
9. O resultado retorna ao Handler.
10. O PerformanceBehavior encerra a medição.
11. O LoggingBehavior registra a conclusão da Request.
12. O MediatR retorna a resposta ao Controller.
13. O Controller responde ao cliente.

---

## Estrutura do Projeto

```
OrderManagement.Api
│
├── Controllers/
│
├── Behaviors/
│
├── Features/
│   └── Orders/
│       ├── Commands/
│       └── Queries/
│
├── Domain/
│
├── Infrastructure/
│
├── Program.cs
└── appsettings.json
```

---

## Organização das Features

Cada funcionalidade possui sua própria pasta contendo todos os arquivos necessários para seu funcionamento.

Exemplo:

```
CreateOrder

├── CreateOrderCommand.cs
├── CreateOrderCommandHandler.cs
└── CreateOrderCommandValidator.cs
```

Essa organização segue o conceito de **Feature Folders**, onde cada funcionalidade possui sua própria estrutura, facilitando manutenção e escalabilidade.

---

## Funcionalidades

### Commands

- Criar Pedido
- Cancelar Pedido

### Queries

- Buscar Pedido por Id
- Listar Pedidos

---

## Benefícios da Arquitetura

- Separação clara entre leitura e escrita
- Baixo acoplamento entre as camadas
- Organização por funcionalidades
- Código mais limpo e de fácil manutenção
- Regras de validação centralizadas
- Behaviors reutilizáveis
- Facilidade para adicionar novas funcionalidades
- Arquitetura preparada para crescimento

---