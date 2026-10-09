# alquiler-eventos-eureka

Servidor de registro y descubrimiento (**Eureka Server**) del sistema de alquiler de
equipos para eventos (FIUNI - Sistemas Web y Distribuidos).

Los microservicios (`ms-alquiler`, `ms-compras`) se registran aqui al arrancar, y el
API Gateway los descubre por nombre para enrutar las peticiones.

## Stack

| Tecnologia | Version |
|---|---|
| Java | 21 |
| Spring Boot | 3.4.5 |
| Spring Cloud (Netflix Eureka Server) | 2024.0.1 |
| Maven | 3.9+ |

## Como correr

```bash
mvn spring-boot:run
```

Consola de administracion de Eureka: <http://localhost:8761>

Ahi se ven los microservicios registrados en la seccion
**Instances currently registered with Eureka**.

> Con `enable-self-preservation: false` Eureka muestra un aviso rojo en el panel. Es
> esperado: sirve para que las instancias caidas desaparezcan rapido del registro.

## Variables de entorno (todo es configurable)

| Variable | Defecto | Descripcion |
|---|---|---|
| `SERVER_PORT` | `8761` | Puerto del servidor Eureka |
| `APP_NAME` | `eureka-server` | Nombre de la aplicacion |
| `EUREKA_HOSTNAME` | `localhost` | Hostname de la instancia |
| `EUREKA_DEFAULT_ZONE` | `http://localhost:8761/eureka/` | URL del registro |
| `EUREKA_SELF_PRESERVATION` | `false` | Modo de autoproteccion de Eureka |
| `EUREKA_EVICTION_INTERVAL_MS` | `5000` | Cada cuanto se limpian instancias caidas |
| `EUREKA_CACHE_UPDATE_MS` | `3000` | Refresco de la cache de respuestas |

## Como registrar un microservicio

En el `pom.xml` del microservicio:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

(y el `dependencyManagement` de `spring-cloud-dependencies` 2024.0.1), y en su `application.yml`:

```yaml
spring:
  application:
    name: ms-alquiler
eureka:
  client:
    service-url:
      defaultZone: ${EUREKA_DEFAULT_ZONE:http://localhost:8761/eureka/}
```
