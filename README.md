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


As instruções para rodar o projeto contidas no item Setup deste documento, referem-se somente ao projeto inteiro, contendo todos os microsserviços.  
Caso queira rodar cada microsserviço individualmente, as instruções estão no repositórios de cada microsserviço.

## Docker
Esse projeto foi desenvolvido para utilização com contêineres. (Docker).  O projeto de orquestração considera o ambiente de Produção. As imagens oficiais utilizadas são :<br>
  - APIs: mcr.microsoft.com/dotnet/sdk:8.0 (Prod)
  - DBs:  postgres:16-alpine
  - PgAdmin: dpage/pgadmin4:latest
  - RabbitMq: rabbitmq:4.2-management-alpine
<br>
  
O projeto quando iniciado pelo comando pelo docker-compose up cria os seguintes contêineres:
  - **FCG-Fase-2**             - Agrupador de todos os contêineres
  - **fcg-users-db**           - Base de dados de cadastro de usuários PostgreSql
  - **fcg-users-api**          - USER.API com os endpoints para usuários em.NET 8.0
  - **fcg-catalog-db**         - Base de dados de catalogo de jogos em PostgreSql
  - **fcg-catalog-api**        - CATALOG.API com os endpoints para catalogo de jogos em.NET 8.0
  - **fcg-payments-db**        - Base de dados de pagamento de jogos PostgreSql
  - **fcg-payments-api**       - PAYMENTS.API com os endpoints para simulação de pagamento de jogos em.NET 8.0
  - **fcg-notifications-api**  - NOTIFICATIONS.API com os endpoints para um sistema de simulação de e-mail de notificação em.NET 8.0
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
4.  Configurar `.env` e `secrets`
    5.1. Acesse a pasta `Helps` e siga as instruções dos arquivos
        - `secrets.help.md`
        - `env.example` 
5. Mantenha a estrutura de pastas conforme o exemplo abaixo:

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

5.1 Visão do diretório dos repositórios:

<img width="627" 
    height="215" 
    alt="visão do diretório dos repositórios" 
    title="Visão do diretório dos repositórios dos microsserviços" 
    aria-label="imagem da visão do diretório dos repositórios dos microsserviços" 
    src="https://github.com/user-attachments/assets/766b2acf-aca0-4353-a5ee-099c11ff10cd" />
    
5.2 Executar via Docker-Compose
5.2.1 Acesse o diretório `pos-tech-fcg-infrastructure`.<br>
5.2.2 Clique com o botão esquerdo do mouse sobre a pasta "docker" e selecione a opção "Open in Terminal"<br>

   <img width="809" 
       height="532" 
       alt="menu de contexto" 
       title="menu de contexto do click direito do mouse"
       aria-label="menu de contexto do click direito do mouse"       
       src="https://github.com/user-attachments/assets/d95c5bbf-29cd-43a3-8fb4-87ce87bf5f22" />


5.2.3 Na janela do terminal digite o comando:

5.2.3.1: Monitorar no próprio console.
    Digite no console o seguinte comando:
    ```bash
    docker-compose up --build
    ```

   Se tudo estiver correto irá mostrar imagems semelhantes a estas: 
   
   <img width="1393" 
       height="498" 
       title="Imagem do docker compilando e iniciando os containers dos microsserviços"
       aria-label="Imagem do docker desktop com os containers dos microsserviços"
       alt="Docker Desktop com os microserviços iniciados" 
       src="https://github.com/user-attachments/assets/3d98649e-562e-4b82-be4c-c74151b09787" />

   
   <img width="1422" 
       height="922" 
       title="Imagem do teminal executando o docker exibindo os logs dos microsserviços"
       aria-label="Imagem do teminal executando o docker exibindo os logs dos microsserviços"
       alt="Imagem do teminal executando o docker exibindo os logs dos microsserviços" 
       src="https://github.com/user-attachments/assets/0fdc04e4-043a-402a-a647-a7450640bf1d" />       
   
5.2.3.2 - Visão - Docker Desktop
    <img width="2165" 
        height="713" 
        title="Imagem do docker descktop com os containers dos microsserviços"
        aria-label="Imagem do docker desktop com os containers dos microsserviços"
        alt="Docker Desktop com os microserviços iniciados" 
        src="https://github.com/user-attachments/assets/176cbcde-c0e8-43e8-ba64-388464c0ede2" />

5.2.3.3 - Não mostrar log no console:
   ```bash
   docker-compose up -d --build
   ```  

### ⬇️⬇️⬇️ ainda não construido.
  
6 **Para rodar o projeto com kubernetes**<br>
Caso as imagens já existam, inicie o projeto clicando com o botão esquerdo do mouse sobre a pasta raiz "fiap-cloud-games-infrastructure" e selecione a opção "Open in Terminal" e rode o seguinte comando:<br>
```bash
  kubectl apply -f k8s/
  ```
Aguarde até que todos os pods estejam iniciados:<br>
<img width="1247" height="489" alt="image" src="https://github.com/user-attachments/assets/6cedceff-8a71-4efc-b814-6d8cd010c5a8" />
