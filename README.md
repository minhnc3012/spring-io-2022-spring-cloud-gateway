# Spring Cloud Gateway - Resilience, Security, and Observability (Spring I/O 2022)

> **Warning**
> This repository is not maintained anymore. For up-to-date examples, refer to [github.com/ThomasVitale/spring-cloud-gateway-resilience-security-observability](https://github.com/ThomasVitale/spring-cloud-gateway-resilience-security-observability).

Do you want to use a microservices architecture? Are you looking for a solution to manage access to single services
from clients? How can you ensure resilience and security for your entire system? Spring Cloud Gateway is a project
based on Reactor, Spring WebFlux, and Spring Boot which provides an effective way to route traffic to your APIs and
address cross-cutting concerns.

In this session, I'll show you how to configure an API gateway to route traffic to your microservices architecture and
implement solutions to improve the resilience of your system with patterns like circuit breakers, retries, fallbacks,
and rate limiters using Spring Cloud Circuit Breaker and Resilience4J. Since the gateway is the entry point of your
system, it’s also an excellent candidate to implement security concerns like user authentication. I'll show you how
to do that with Spring Security, OAuth2, and OpenID Connect, relying on Spring Redis Reactive to manage sessions.
Finally, I'll show you how to improve the observability of your system using Spring Boot Actuator
and Spring Cloud Sleuth and relying on the Grafana stack.

[Conference Session](https://2022.springio.net/sessions/spring-cloud-gateway-resilience-security-and-observability)

## Usage

You can use Docker Compose to set up the entire system, including applications, data services, and the Grafana observability stack.

First, package both the Edge Service and Book Service application as container images leveraging the Cloud Native Buildpacks integration
provided by Spring Boot. For each application, run the following task:

```bash
./gradlew bootBuildImage
```

Then, from the project root folder, run Docker Compose, after updating the docker-compose.yml file with your own images. If you prefer to use the ones provided with this project, you can use the current configuration without any changes and without the need to build the applications locally.

```bash
docker-compose up -d
```

The Edge Service application is exposed on port 9000 while Book Service on port 9001. The applications require authentication through
OAuth2/OpenID Connect. You can log in as Isabelle (isabelle/password) or Bjorn (bjorn/password).

## Observability Stack

Both Spring Boot applications are observable, as any cloud native application should. Prometheus metrics are exposed thanks to the
Spring Boot Actuator library. Distributed tracing is enabled through Spring Cloud Sleuth.

**Grafana** lets you query and visualize logs, metrics, and traces from your applications. After running the Docker Compose
configuration as explained in the previous section, you can access Grafana on port 3000. It provides already dashboards
to visualize metrics from Spring Boot, Spring Cloud Gateway, and Spring Cloud Load Balancer. In the "Explore" panel,
you can query logs from Loki, metrics from Prometheus, and traces from Tempo.

**Fluent Bit** collects logs from all containers and forwards them to Loki.

**Loki** is a log aggregation system part of the Grafana observability stack. "It's like Prometheus, but for logs."
Logs are available for inspecting from Grafana.

**Tempo** is a distributed tracing backend part of the Grafana observability stack. Spring Boot applications sends traces to Tempo,
which made them available for inspecting from Grafana. The traces follows the OpenTelemetry format and protocol.

**Prometheus** is a monitoring system part of the Grafana observability stack. It parses the metrics endpoints exposed by Spring Boot
applications (`/actuator/prometheus`). Metrics are available for inspecting and dashboarding from Grafana.

Video: 
1. https://www.youtube.com/watch?v=UXcCHX_ymag
2. https://www.youtube.com/watch?v=jkP199zzknw&t=144s
