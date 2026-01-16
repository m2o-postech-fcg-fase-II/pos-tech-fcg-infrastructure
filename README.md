# FCG Platform - FIAP Cloud Games - Tech Challenge - Fase II - Grupo 39

## Visão Geral
Este repositório contém o projeto de orquestração desenvolvido para o Tech Challenge - Parte 2 do curso  **Arquitetura de Sistemas .NET** com foco nos princípios de Domain-Driven Design (DDD).

O objetivo desta fase é refatorar a aplicação monolítica da Fase I do projeto em uma arquitetura de microsserviços orientada a eventos.  O projeto da Fase I encontra-se no repositorio: 
```bash
https://github.com/gmerendi/fiap-cloud-games
``` 
Quatro microsserviços independentes foram desenvolvidos: Usuários, Catálogo, Pagamentos e Notificações, além desse repositório para orquestração.  Os repositórios dos microsserviços são:
USER.API
```bash
https://github.com/gmerendi/fiap-cloud-games-users-api
```
CATALOG.API
```bash
https://github.com/gmerendi/fiap-cloud-games-catalog-api
```
NOTIFICATIONS.API
```bash
https://github.com/gmerendi/fiap-cloud-games-notifications-api
```
PAYMENTS.API
```bash
https://github.com/gmerendi/fiap-cloud-games-payments-api
```
As instruções para rodar o projeto contidas no item Setup deste documento, referem-se somente ao projeto inteiro, contendo todos os microsserviços.  Caso queira rodar cada microsserviço individualmente, as instruções estão no repositórios de cada microsserviço.

## Docker
Esse projeto foi desenvolvido para utilização com Conteineres. (Docker).  O projeto de orquestração considera o ambiente de Produção. As imagens oficiais utilizadas são :<br>
  - APIs: mcr.microsoft.com/dotnet/sdk:8.0 (Prod)
  - DBs:  postgres:15-alpine
  - RabbitMq: rabbitmq:4.1.6-management-alpine
<br>
  
O projeto quando iniciado pelo comando docker-compose up local, cria os seguintes conteineres:
- **fcg-users-api**          -  USERS.API com os endpoints para usuarios em.NET 8.0
- **fcg-users-db**           -  Base de dados de cadastro de usuarios PostgreSql
- **fcg-catalog-api**        -  CATALOG.API com os endpoints para catalogo de jogos em.NET 8.0
- **fcg-catalog-db**         -  Base de dados de catalogo de jogos em PostgreSql
- **fcg-notifications-api**  -  NOTIFICATIONS.API com os endpoints para um sistema de simulaçao de e-mail de notificação em.NET 8.0
- **fcg-payments-api**       -  PAYMENTS.API com os endpoints para simulação de pagamento de jogos em.NET 8.0
- **fcg-payments-db**        -  Base de dados de pagamento de jogos PostgreSql
- **fcg-rabbitmq**           -  RabbitMQ
- **fcg-ready**              - Somente para verificar se todos os conteineres subiram. Este conteiner mostra-se sempre parado no docker desktop

### Imagens Docker 
Quando iniciado pelo docker-compose up --build:
- **fcgusersapi:v2**         - Gerada pelo Dockerfile do repositório USERS.API
- **fcgusersdb:v2**          - Gerada pelo Dockerfile desse repositório de orquestração.
- **fcgcatalogapi:v2**       - Gerada pelo Dockerfile do repositório CATALOG.API
- **fcgusersdb:v2**          - Gerada pelo Dockerfile desse repositório de orquestração.
- **fcgnotificationsapi:v2** - Gerada pelo Dockerfile do repositório NOTIFICATIONS.API
- **fcgpaymentsapi:v2**      - Gerada pelo Dockerfile do repositório PAYMENTS.API
- **fcgpaymentsdb:v2**       - Gerada pelo Dockerfile desse repositório de orquestração.
- **fcgrabbitmq:v2**         - Gerada pelo Dockerfile desse repositório de orquestração.

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

A comunicação entre os serviços ocorre de forma **assíncrona**, utilizando **mensageria** (RabbitMQ + MAssTransit), seguindo os princípios de **arquitetura orientada a eventos**.


## Pré-requisitos
- Docker Desktop 4.52.0
- Kubernetes 1.31.1 (instalado no Docker Desktop) - cluster type: kind
- Visual Studio 2026

### Backend & Linguagens:

* **Linguagem Principal:**
  * C#
* **Framework:**
  * .Net Core 8
  * Swagger
  * LinQ
  * GrapQl
  * EFS Core 8
  * JWT Bearer
  * Dapper
  * Docker
  * Kubernetes
 
### Setup
1. Clone este repositório, bem como os repositórios dos microsserviços utilizando Visual Studio.  O Branch é o Develop para todos.  Utilize a hierarquia de pastas abaixo, mantendo os nomes.  Caso os nomes sejam substituidos, o arquivo docker-compose também deverá ser modificado.
repo/<br>
├── fiap-cloud-games-catalog-api/<br>
│   └── Clone do repositório: https://github.com/gmerendi/fiap-cloud-games-catalog-api<br>
│<br>
├── fiap-cloud-games-infrastructure/<br>
│   └── Este repositório.<br>
│<br>
├── fiap-cloud-games-notifications-api/<br>
│   └── Clone do repositório: https://github.com/gmerendi/fiap-cloud-games-notifications-api<br>
│<br>
├── fiap-cloud-games-payments-api/<br>
│   └── Clone do repositório: https://github.com/gmerendi/fiap-cloud-games-payments-api<br>
│<br>
└── fiap-cloud-games-users-api/<br>
    └── Clone do repositório: https://github.com/gmerendi/fiap-cloud-games-users-api<br>

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








