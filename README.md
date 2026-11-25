# TP8 – Introduction aux microservices avec ASP.NET Core

Solution `MicroservicesApp` composée de 3 projets Web API et d'une bibliothèque partagée :

| Projet | Rôle | Port (https) |
|---|---|---|
| **ProductsAPI** | Microservice produits (émetteur des messages RabbitMQ) | 7066 |
| **OrdersAPI** | Microservice commandes (consommateur des messages RabbitMQ) | 7065 |
| **ApiGateway** | API Gateway **Ocelot** (point d'entrée unique) | 7162 |
| **Shared** | Classe `ProductCreated` partagée (message RabbitMQ) | – |

## Vidéo de démonstration

[`demo/TP8_demo.webm`](demo/TP8_demo.webm) (2 min 41, sans son, sous-titrée) : interface RabbitMQ
(Overview, Connections, Channels, Exchanges, Queues), ajout d'un produit dans Swagger, synchronisation
ProductsAPI → OrdersAPI via RabbitMQ, appels à travers la gateway Ocelot.

## Technologies

- ASP.NET Core Web API (.NET 10), Entity Framework Core + SQL Server LocalDB
- Ocelot 18.0.0 (API Gateway) – routes dans `ApiGateway/Ocelot.json`
- RabbitMQ + MassTransit (communication asynchrone)

## Lancer le projet

1. RabbitMQ dans Docker (port AMQP exposé sur **4001**) :
   ```
   docker run -d --hostname my-rabbit --name ecomm -p 15672:15672 -p 4001:5672 rabbitmq:3-management
   ```
   Interface d'administration : http://localhost:15672 (guest / guest)
2. Créer les bases (Console du Gestionnaire de package, pour **ProductsAPI** puis **OrdersAPI**) :
   ```
   Update-Database
   ```
3. Démarrer les 3 projets (Plusieurs projets de démarrage : ProductsAPI, OrdersAPI, ApiGateway).

## Routes de la gateway

| Gateway | Microservice |
|---|---|
| `GET/POST/PUT /apigateway/Products` | `ProductsAPI /api/Products` |
| `GET/POST /apigateway/products/{id}` | `ProductsAPI /api/Products/{id}` |
| `GET/POST/PUT /apigateway/Orders` | `OrdersAPI /api/Orders` |
| `GET/POST /apigateway/Orders/{id}` | `OrdersAPI /api/Orders/{id}` |
