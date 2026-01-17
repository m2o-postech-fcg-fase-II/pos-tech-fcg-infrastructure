## FIAP - Arquitetura em Sistemas .NET
- Tech Challenge - FIAP Cloud Games - Fase II - Grupo 57
    - Marcelo Mendes de Oliveira - __RM: 367563__
    - Eduardo Martins Oliveira - __RM: 368703__
---
### Sobre:
Este repositório é parte integrante da proposta da **FIAP - Pós-Tech em Arquitetura de Sistemas .NET _com foco nos princípios de Domain-Driven Design  (DDD)._ - Fase II**.
O propósito desta fase é refatorar a solução da **fase I** - refatorar o monolito da Plataforma **Fiap Cloud Games - FCG** e "quebrar" em microsserviços orquestrados via k8s.

---
#### Visão Geral
 Este repositório tem como objetivo centralizar as configurações referentes ao _docker-compose_ e _orquestração do Kubernates_.

Projeto da fase I a ser refatorado:
```
https://github.com/fiap-tech-challenge-fgc/fiap-cloud-game
```

## Fase II
A proposta é refatorar o monolito da **Fase I** em 4 microsservicos distintos, sendo eles:
- USER.API
```
https://github.com/m2o-postech-fcg-fase-II/pos-tech-fcg-user-api
```
- CATALOG.API
```
https://github.com/m2o-postech-fcg-fase-II/pos-tech-fcg-catalog-api
```
- NOTIFICATIONS.API
```
https://github.com/m2o-postech-fcg-fase-II/pos-tech-fcg-notification-api
```
- PAYMENTS.API
```
https://github.com/m2o-postech-fcg-fase-II/pos-tech-fcg-payment-api
```

> Para compartilhar determinadas entidades que seriam reutilizadas e algumas regras de negócio foi tambem foi criado um repositório contendo um projeto nuget.
> - FCG.Generics
> ```
> https://github.com/m2o-postech-fcg-fase-II/pos-tech-fcg-generics
>```
> Nuget: [FCG.10NETT](https://www.nuget.org/packages/FCG.10NETT)


As instruções para rodar o projeto contidas no item Setup deste documento, referem-se somente ao projeto inteiro, contendo todos os microsserviços.  Caso queira rodar cada microsserviço individualmente, as instruções estão no repositórios de cada microsserviço.

## Docker
Esse projeto foi desenvolvido para utilização com contêineres. (Docker).  O projeto de orquestração considera o ambiente de Produção. As imagens oficiais utilizadas são :<br>
  - APIs: mcr.microsoft.com/dotnet/sdk:8.0 (Prod)
  - DBs:  postgres:16-alpine
  - PgAdmin: dpage/pgadmin4:latest
  - RabbitMq: rabbitmq:4.2-management-alpine
<br>
  
O projeto quando iniciado pelo comando pelo docker-compose up cria os seguintes contêineres:
  - **FCG-Fase-2**             - Agrupador de todos os contêineres
  - **fcg-users-api**          - USER.API com os endpoints para usuários em.NET 8.0
  - **fcg-users-db**           - Base de dados de cadastro de usuários PostgreSql
  - **fcg-catalog-api**        - CATALOG.API com os endpoints para catalogo de jogos em.NET 8.0
  - **fcg-catalog-db**         - Base de dados de catalogo de jogos em PostgreSql
  - **fcg-payments-db**        - Base de dados de pagamento de jogos PostgreSql
  - **fcg-notifications-api**  - NOTIFICATIONS.API com os endpoints para um sistema de simulação de e-mail de notificação em.NET 8.0
  - **fcg-payments-api**       - PAYMENTS.API com os endpoints para simulação de pagamento de jogos em.NET 8.0
  - **fcg-pgadmin**            - PgAdmin
  - **fcg-rabbitmq**           - RabbitMQ


## Kubernetes
Os arquivos para deployment do projeto estão na pasta root/k8s:
- fcg-configmap.yaml
- fcg-deployments.yaml
- fcg-secrets.yaml
- fcg-services.yaml
- fcg-volumes.yaml


## 🏗 Contexto da Arquitetura geral

A plataforma FCG é composta pelos seguintes microsserviços independentes:

| Serviço | Responsabilidade |
|-------|------------------|
| Users.API | Gestão de usuários, autenticação e autorização |
| Catalog.API | Catálogo de jogos e início do fluxo de compra |
| Payments.API | Processamento de pagamentos (simulado) |
| Notifications.API | Envio de notificações (simulado via logs) |
| FCG.Generics (FCG.10NETT) | Nugget contendo entidades de reuso |

A comunicação entre os serviços ocorre de forma **assíncrona**, utilizando **mensageria** (RabbitMQ + MassTransit), seguindo os princípios de **arquitetura orientada a eventos**.


## Pré-requisitos
- Docker Desktop 4.52.0
- Kubernetes 1.31.1 (instalado no Docker Desktop) - cluster type: kind
- Visual Studio 2026

### Backend & Linguagens:

* **Linguagem Principal:**
  * C#
* **Framework:**
  * .Net Core 8
  * EF Core 8
  * Identity Framework Core (UserAPI)
  * Swagger
  * LinQ
  * JWT Bearer
  * Docker
  * Kubernetes
 
### Setup
1. Clone este repositório, bem como os demais repositórios dos microsserviços. 
2. Mantenha os nomes originais dos repositórios ao cloná-los (caso mude é necessário rever o docker-compose).
3. Branch é o Develop para todos. 
4. Mantenha a estrutura de pastas conforme o exemplo abaixo:

```repo/<br>
├── fiap-cloud-games-infrastructure/
│   └── Este repositório.
│
├── pos-tech-fcg-user-api/
│   └── ```bash git clone https://github.com/m2o-postech-fcg-fase-II/pos-tech-fcg-user-api.git```
│
├── pos-tech-fcg-catalog-api/
│   └── ```bash git clone https://github.com/m2o-postech-fcg-fase-II/pos-tech-fcg-catalog-api.git```
│
├── pos-tech-fcg-payment-api/
│   └── ```bash git clone https://github.com/m2o-postech-fcg-fase-II/pos-tech-fcg-payment-api.git```
│
├── pos-tech-fcg-notification-api/
│   └── ```https://github.com/m2o-postech-fcg-fase-II/pos-tech-fcg-notification-api.git```
│
└── pos-tech-fcg-generics/
    └── ```https://github.com/m2o-postech-fcg-fase-II/pos-tech-fcg-generics```
```

#### Edição Pendente:

<img width="530" height="190" alt="image" src="https://github.com/user-attachments/assets/5bfaf29f-58a0-4334-a5f4-a7ac8c326ea1" />

2. Acesse a pasta contendo este repositório de orquestração.<br>
2.1 **Para rodar o projeto com docker-compose**<br>
   2.1.1 Clique com o botão esquerdo do mouse sobre a pasta "docker" e selecione a opção "Open in Terminal"<br>
   <img width="328" height="415" alt="image" src="https://github.com/user-attachments/assets/ed6fa237-4a9e-413d-a21c-57a8d4d58941" /><br><br>

  2.1.2 Digite no powershell aberto o comando:
  ```bash
  docker-compose up --build
  ```
  2.1.3 Aguarde até que os conteineres tenham iniciado. O container fcg-ready não permanece iniciado.  Ele é utilizado apenas para prover uma mensagem indicando que o ambiente está pronto no console.<br>
  <img width="1269" height="633" alt="image" src="https://github.com/user-attachments/assets/b19fb09c-6513-42d7-83de-389b1ad93cce" /><br>
  <img width="1581" height="214" alt="image" src="https://github.com/user-attachments/assets/a37f1173-a656-4581-b570-939ac99d3ac2" /><br>

  
2.2 **Para rodar o projeto com kubernetes**<br>
2.2.1  Procure pelas imagens docker v2 no docker desktop.  Caso as imagens não existam, clique com o botão esquerdo do mouse sobre a pasta "docker" e selecione a opção "Open in Terminal" e rode o seguinte comando para gerá-las:<br>
```bash
  docker-compose build
  ```
<img width="1296" height="573" alt="image" src="https://github.com/user-attachments/assets/a5abf4a5-ebb2-4e48-b952-dbc9438bef2b" /><br>
Caso as imagens já existam, inicie o projeto clicando com o botão esquerdo do mouse sobre a pasta raiz "fiap-cloud-games-infrastructure" e selecione a opção "Open in Terminal" e rode o seguinte comando:<br>
```bash
  kubectl apply -f k8s/
  ```
Aguarde até que todos os pods estejam iniciados:<br>
<img width="1247" height="489" alt="image" src="https://github.com/user-attachments/assets/6cedceff-8a71-4efc-b814-6d8cd010c5a8" />
