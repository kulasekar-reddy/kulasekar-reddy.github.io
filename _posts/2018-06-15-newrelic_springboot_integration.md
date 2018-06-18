---
layout: post
title: SpringBoot newlrelic integration through actuator
description: "Through the actuator plugin provided by spring boot,How we will connect to
NewRelic using metric registrey"
created: 2018-06-16
comments: true
share:    true
tags: [New Relic,Spring Boot,Metric Registrey,Actuator]
image:
  background: wood.jpg
---

In this post I am going to show you how to push insights to NewRelic using spring boot actuator plugin
 
**Gradle dependencies required**

```groovy
  compile('org.springframework.boot:spring-boot-starter-actuator')
  compile 'io.micrometer:micrometer-registry-new-relic:latest.release'
```
**Configurations need to add in application.yml**

```yaml
management:
metrics.export.newrelic:
  enabled: false
  account-id: <account_id>
  api-key: <api_key>
  step: 1m #time to push events to New Relic
```
**Craete a configuration class to customize MeterRegistry**

```java
import io.micrometer.core.instrument.MeterRegistry;
import io.micrometer.core.instrument.config.MeterFilter;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.actuate.autoconfigure.metrics.MeterRegistryCustomizer;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MetricsConfig {

  @Value("${spring.application.name}")
  private String applicationName;

  @Value("${spring.profiles.active}")
  private String applicationProfile;

  //Metering Config
  @Bean
  MeterRegistryCustomizer<MeterRegistry> metricsCommonTags() {
    return registry -> {
      registry.config().commonTags(applicationName, applicationProfile)
          .meterFilter(MeterFilter.denyNameStartsWith("jvm"))
          .meterFilter(MeterFilter.denyNameStartsWith("logback"))
          .meterFilter(MeterFilter.denyNameStartsWith("process"))
          .meterFilter(MeterFilter.denyNameStartsWith("tomcat"))
          .meterFilter(MeterFilter.denyNameStartsWith("system"));
    };
  }

}
```

**Autowire the MeterRegistry class**

*Required Imports*

```java
import io.micrometer.core.instrument.MeterRegistry;
```
*Autowire MeterRegistry*

```java
@Autowired
private MeterRegistry meterRegistry;
```
**Declare the counter values**

*Required Imports*

```java
import io.micrometer.core.instrument.Counter;
```

*Intialise Counters*

```java
private Counter successCounter;
private Counter failureCounter;
```
*Instansite  Counters using postconstruct*

```java
@PostConstruct
public void init(){
  successCounter = meterRegistry.counter("success.counter");
  failureCounter = meterRegistry.counter("failure.counter");
}
```
*Increment counters based on your business logic*

```java
successCounter.increment();
failureCounter.increment();
```

