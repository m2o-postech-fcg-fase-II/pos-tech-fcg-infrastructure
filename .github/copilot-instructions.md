# Copilot Instruction — Infrastructure / Orchestration

## Objetivo
Orientar o repositório de infraestrutura/orquestração contendo Docker Compose e manifestos Kubernetes.

---

## Escopo do Repositório
Centralizar:
- docker-compose.yml
- Manifestos Kubernetes
- Documentação principal (README.md)

---

## Docker Compose
O arquivo deve permitir subir toda a aplicação com:

Incluir:
- UsersAPI  
- CatalogAPI  
- PaymentsAPI  
- NotificationsAPI  
- RabbitMQ/Kafka  
- Banco(s) de dados  

---

## Kubernetes
Criar pasta `/k8s` contendo:
- Deployments (1 por serviço)
- Services (ClusterIP)
- ConfigMaps
- Secrets
- Deployment do RabbitMQ/Kafka
- Namespace dedicado (opcional)

### Nomenclatura de Pods
Quando o usuário especificar "era apenas o nome do pod", as alterações devem se limitar apenas ao nome do deployment/pod no Kubernetes, mantendo os nomes do banco de dados e outros componentes como estavam (singular para catalog).

---

## Boas Práticas
- Usar variáveis de ambiente.
- Usar nomes de serviços como:
  - `users-api`
  - `catalog-api`
  - `payments-api`
  - `notifications-api`

---

## Documentação
O README principal deve conter:
- Como rodar com Docker Compose.
	- docker-compose up
	
- Como aplicar no Kubernetes:
	- kubectl apply -f k8s/
	 
- Como verificar pods:
	- kubectl get pods

- O README deve ser em Markdown bruto, em um único arquivo, sem HTML, sem tags e sem imagens embutidas; sempre gerar documentação em Markdown puro para este repositório.

