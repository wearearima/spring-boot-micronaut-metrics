# Micronaut and Spring Boot comparison

We've seen many comparisons between Micronaut and Spring Boot, but we wanted to measure their performance with the most popular Java sample application: [Petclinic](https://spring-petclinic.github.io/). 

First, we took the [official Petclinic version](https://github.com/spring-projects/spring-petclinic) and measured some metrics explained in this repository: https://github.com/wearearima/spring-boot-docker-study 

Then, we repeated the same tests with @graemerocher's Petclinic version implemented with [Micronaut](https://micronaut.io/). The results we achieved with Micronaut are explained in this other repository: https://github.com/wearearima/micronaut-docker-study 

Generally, comparisons are not fair because there are always differences which may impact the results. For example, Spring Boot's official version has Spring Actuator enabled by default, which has an impact on startup time. To allow for this, we've made [some changes](https://github.com/wearearima/spring-boot-docker-study/commit/2a092a314a9635088f9c189dffcdbff3c874d96c) to Spring Boot's official version:
 - Use Mysql instead of an embedded database
 - Disable Spring Actuator
 - Disable JMX
 - Use `slf4j-jdk14` for logging

In the [`feature/optimized`](https://github.com/wearearima/spring-boot-docker-study/tree/feature/optimized) branch you can check out the changes we've made. 

Another difference is that Micronaut's Petclinic version is just a fork of the official version and it uses libraries that don't get the most out of Micronaut, for example Hibernate instead of [Micronaut Data](https://micronaut-projects.github.io/micronaut-data/1.0.x/guide/), but unfortunately we're unable to mitigate this.

This table summarizes the results with each framework:

| Feature                                           | Spring Boot 2.1.6 <sup>*</sup> | Micronaut 1.2.2   |
| ------------------------------------------------- | -----------------------------  | ----------------- |
| App disk usage                                    | 43M                            | 37M               |
| App disk usage (without dependencies)             | 371KB                          | 637KB             |
| Docker image disk usage                           | 136MB                          | 129MB             |
| Startup time                                      | 4,3 seconds                    | 2,31 seconds      | 
| Heap consumption                                  | 45MB                           | 60MB              |
| Memory consumption                                | 560MB                          | 560MB             |

> \* Spring Boot's results are based on the [`feature/optimized`](https://github.com/wearearima/spring-boot-docker-study/tree/feature/optimized) branch (mysql database, jmx and spring actuator disabled and `slf4j-jdk14` for logging). 

It's notable that Micronaut's startup time is better than Spring Boot's, mainly because Micronaut uses ahead-of-time (AOT) compilation. However, heap and memory consumption results are similar with both frameworks. Those figures should be taken with caution because heap and memory consumption vary significantly every time the tests are run. We decided to publish the best results we achieved. 

Additionally, Micronaut application with [Graal](https://guides.micronaut.io/micronaut-creating-first-graal-app/guide/index.html) would improve its results, but we can't compare with Spring Boot because [Spring Framework support for Graal is not available yet](https://github.com/spring-projects/spring-framework/wiki/GraalVM-native-image-support). 

The results may vary depending on many other factors (hw resources, java version, config params, etc). We obtained these results with:
 - 2018 MacBook Pro, 2,7 GHz Intel Core i7, 16 GB 2133 MHz
 - AdoptOpenJDK 1.8.0_222

Please feel free to send your feedback or suggest improvements. 

![ARIMA Software Design](https://arima.eu/arima-claim.png)