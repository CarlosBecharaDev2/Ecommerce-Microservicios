# E-commerce con Spring Boot y Docker

Microservicios (config-server, eureka-server, api-gateway, producto-service x2, pedido-service) y dos bases PostgreSQL, levantados con un solo comando.

## Arquitectura

```
Cliente --> api-gateway :8080 --> producto-service (x2) --> postgres-productos
                |                   |    --> pedido-service --> postgres-pedidos
                v
          eureka-server :8761  <-- todos se registran
                ^
          config-server :8888
```

Solo el Gateway (8080) y el panel de Eureka (8761) publican puertos al equipo.

## Requisitos

Docker Desktop / Engine y Docker Compose v2. (JDK y Maven no hacen falta: se compila dentro de Docker.) Recomendado 16 GB de RAM; en 8 GB poner `replicas: 1` en producto-service.

## Despliegue

```bash
cp .env.example .env
docker compose up -d --build
docker compose ps
```

## Uso

- Salud: `GET http://localhost:8080/actuator/health`
- Panel Eureka: http://localhost:8761
- `POST http://localhost:8080/api/productos` con `{"nombre":"Mouse","precio":25.5}`, `GET /api/productos`
- `POST http://localhost:8080/api/pedidos` con `{"productoId":1,"cantidad":2}`, `GET /api/pedidos`

Colecci\u00f3n Postman: [ecommerce.postman_collection.json](ecommerce.postman_collection.json)

## Estado (docker compose ps)

```
NAME                             STATUS                        PORTS
ecommerce-api-gateway-1          Up About a minute (healthy)   0.0.0.0:8080->8080/tcp, [::]:8080->8080/tcp
ecommerce-config-server-1        Up 2 minutes (healthy)        8888/tcp
ecommerce-eureka-server-1        Up 2 minutes (healthy)        0.0.0.0:8761->8761/tcp, [::]:8761->8761/tcp
ecommerce-pedido-service-1       Up About a minute (healthy)   8082/tcp
ecommerce-postgres-pedidos-1     Up 2 minutes (healthy)        5432/tcp
ecommerce-postgres-productos-1   Up 2 minutes (healthy)        5432/tcp
ecommerce-producto-service-1     Up About a minute (healthy)   8081/tcp
ecommerce-producto-service-2     Up About a minute (healthy)   8081/tcp
```
