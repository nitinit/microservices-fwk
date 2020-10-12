|Attributes    | Spring boot webflux | Micronaut     |
| -------------  | -------------       | ------------  |
| Community      | 5        | 4  |
|    DI          | Content Cell        | Content Cell  |
|    Observability / monitoring	          | Content Cell        | Content Cell  |
|    Reactive support	          | Content Cell        | Content Cell  |
|    Swagger integration	          | Content Cell        | Content Cell  |
|    Simplicity          | Content Cell        | Content Cell  |
|    Configuration management	          | Content Cell        | Content Cell  |
|    Resilience features	          | Content Cell        | Content Cell  |
|    Language support	          | Content Cell        | Content Cell  |
|    Current knowledge		          | Content Cell        | Content Cell  |
|    TOTAL POINTS		          | Content Cell        | Content Cell  |


# Point to consider
Besides some play around with Micronaut to check how suitable and easy to start with is for our needs, and the check on their main characteristics to be sure that it adapts to what are we looking for. We need to consider some main points:
1. Reactive framework support
2. Web client support and client for Redis
3. Performance benchmark (including Memory/CPU footprint)
4. Dependency injection
5. Logging and Tracebility - can we trace requests end to end
6. Code maintainability considerations
7. Ease of deployment 

## 1) Reactive framework support
Micronaut uses RxJava2 as the default implementation for Reactive Streams API, but RxJava3 and Reactor are supported (it also support Legacy RxJava1)

See details at: 
[Micronaut](https://docs.micronaut.io/snapshot/guide/index.html#reactiveConfigs)

For RxJava3 see:
[Micronaut][Micronaut RxJava 3](https://micronaut-projects.github.io/micronaut-rxjava3/latest/guide/)

For Reactor:
[Micronaut Reactor](https://micronaut-projects.github.io/micronaut-reactor/snapshot/guide/index.html)

also small article about getting started with reactor, as the guide above is very brief: 
[Micronaut Mastery: Using Reactor Mono and Flux - DZone Microservices](https://dzone.com/articles/micronaut-mastery-using-reactor-mono-and-flux)

We are using RxJava2 at the moment so we can keep using that for the time being, and move to RxJava3 when needed.

## 2) Web client support and client for Redis

### Http Client support:
The Micronaut built in client has both a low-level API and a higher level AOP-driven API. Hystric is supported in its own module, so we can still use it to implement the circuit breaker. There is also some native support to things like Retry and Fallback. For more information check:

[Micronaut](https://docs.micronaut.io/snapshot/guide/index.html#httpClient)


### Client for Redis:
Micronaut features automatic configuration of the Lettuce driver for Redis.


For more information, check: [Micronaut Redis](https://micronaut-projects.github.io/micronaut-redis/snapshot/guide/) 


## 3) Performance benchmark (including Memory/CPU footprint)

Compared to other frameworks like Spring Boot and Quarkus, Micronaut seems to perform better than this 2 mainstream ones.

![Micronaut vs Quarkus vs Spring Boot](https://micronaut.io/blog/2020-04-07-img02.jpeg)

Check this recent comparison (image above is taking from here) to have more detail about it (althougth it comes from Micronaut):
https://micronaut.io/blog/2020-04-07-micronaut-vs-quarkus-vs-spring-boot-performance-jdk-14.html
that includes a video on the topic.

Also, an independent benchmark for frameworks comparison can be seen at: 
[Micronaut vs Quarkus vs Spring Boot Performance on JDK 14][TechEmpower Framework Benchmarks](https://www.techempower.com/benchmarks/#section=data-r19&hw=cl&test=composite)

Checking the one above, it looks like Micronaut should performs better.


## 4) Dependency injection

This is probably one of the other places where the use of Micronaut will bring more benefits to us. Micronaut comes with its own IoC container. It's very "springy", so anyone familiar with Spring won't have any issue picking the basics. Besides the inspiration from Spring, it comes with several improvements, most notably the limited use of Reflection and that it avoid proxies. Which means faster starter time, reduced memory footprint, smaller stack traces and plenty of benefits.

A quick example that can be done:
[Creating your first Micronaut app \| Micronaut Guides | Micronaut Framework](https://guides.micronaut.io/creating-your-first-micronaut-app/guide/index.html)

Also, this is a good video about some Micronaut basics, it explains the approach to design Micronaut and why the Dependency Injection does not become an issue as in Spring and some similar older frameworks. It includes some interesting features too:
[Introduction to Micronaut: Lightweight Microservices with Ahead of Time Compilation by Graeme Rocher - YouTube](https://www.youtube.com/watch?v=P1qp_l5EFic)


## 5) Logging and Tracebility - can we trace requests end to end

### Logging:

_(Copied from Micronaut Documentation)_
The loggers endpoint is composed of two customizable parts: a loggers manager and a logging system.

The loggers manager (LoggersManager) is responsible for returning a Publisher that will return data collected and transformed for the response, and it is also responsible for updating a logger with a new log level.

To override the default behavior for the loggers manager, either extend the default implementation (DefaultLoggersManager) or implement the LoggersManager interface directly. To ensure your implementation is used instead of the default, add the @Replaces annotation to your class with the value being the default implementation.

The logging system (LoggingSystem) is responsible for processing requests from the loggers manager against a particular logging library (e.g. logback, log4j, etc.)

The current default implementation for the logging system is LogbackLoggingSystem, which works with the logback logging framework. Additional logging systems will be implemented in future revisions of Micronaut. For custom logging system behavior, implement the LoggingSystem interface directly. To ensure your implementation is used instead of the default, add the @Replaces annotation to your class with the value being the default implementation.


### Tracebility.

Micronaut supports traceability with Jaeger and Zipkin.

[Microservices distributed tracing with Jaeger and Micronaut \| Micronaut Guides | Micronaut Framework](https://guides.micronaut.io/micronaut-microservices-distributed-tracing-jaeger/guide/index.html)

[Microservices distributed tracing with Zipkin and Micronaut \| Micronaut Guides | Micronaut Framework](https://guides.micronaut.io/micronaut-microservices-distributed-tracing-zipkin/guide/index.html)


## 6) Code maintainability considerations


## 7) Ease of deployment 

No issues seen here. A runnable jar can be easily generated, and then packed within a Docker container. Micronaut CLI can generate a default Dockerfile to make this easier. 
Also a GraalVM native image can be created ([Native Image](https://www.graalvm.org/docs/reference-manual/native-image/))

Check section 8.3 in this guide: [Creating your first Micronaut app \| Micronaut Guides | Micronaut Framework](https://guides.micronaut.io/creating-your-first-micronaut-app/guide/index.html)

# Conclusion
As seen in the points above, nothing really worrying has been found in the spike, as long as Micronaut performs similar (hopefully better).
