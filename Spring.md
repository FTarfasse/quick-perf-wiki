# 🚩 Table of contents

[Spring and JUnit 4](#Spring-and-JUnit-4)<br><br>
[Spring and JUnit 5](#Spring-and-JUnit-5)<br><br>
[Spring and TestNG](#Spring-and-TestNG)<br><br>

# Spring and JUnit 4
## QuickPerfSpringRunner
**QuickPerfSpringRunner** adds QuickPerf features to *SpringRunner* (also called *SpringJUnit4ClassRunner*). <br>

To use it, following your Spring version, you have to add one of the dependencies below.

**Spring 5**
```xml
<dependency>
  <groupId>org.quickperf</groupId>
  <artifactId>quick-perf-junit4-spring5</artifactId>
  <version>1.0.0-RC6</version>
  <scope>test</scope>
</dependency>
```

**Spring 4**
```xml
<dependency>
  <groupId>org.quickperf</groupId>
  <artifactId>quick-perf-junit4-spring4</artifactId>
  <version>1.0.0-RC6</version>
  <scope>test</scope>
</dependency>
```

**Spring 3**
```xml
<dependency>
  <groupId>org.quickperf</groupId>
  <artifactId>quick-perf-junit4-spring3</artifactId>
  <version>1.0.0-RC6</version>
  <scope>test</scope>
</dependency>
```

***With one of these dependencies, you have access to [core](https://github.com/quick-perf/doc/wiki/Core-annotations), [JVM](https://github.com/quick-perf/doc/wiki/JVM-annotations) and [SQL](https://github.com/quick-perf/doc/wiki/SQL-annotations) annotations.***

⚠️ If you use Gradle, please read [this](https://github.com/quick-perf/doc/wiki/Gradle-users).

_QuickPerf annotations are executed after the loading of the SpringContext._ So, for example, if you profile your JVM with [@ProfileJvm](https://github.com/quick-perf/doc/wiki/JVM-annotations#Profile-or-check-your-JVM), the profiling starts just after the loading of the Spring context.

*Java code example with QuickPerfSpringRunner*
```java
	import org.junit.runner.RunWith;
	import quickperf.spring.QuickPerfSpringRunner;

	@RunWith(QuickPerfSpringRunner.class)
	public class AccountTest {

	}
```

## Configuration for SQL annotations

To can use [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations), you have to add a *QuickPerfProxyBeanPostProcessor* bean.<br>
To do this, you can look at the Java code examples below or [this Spring Boot project](https://github.com/quick-perf/quickperf-examples/tree/master/springboot-junit4).<br> 

After that, you can evaluate the SQL properties of the database repositories, the Spring services or the Spring controller.

*Addition of a QuickPerfProxyBeanPostProcessor bean with @Bean*
```java
	import org.springframework.context.annotation.Bean;
	import org.springframework.context.annotation.Configuration;
	import quickperf.spring.QuickPerfProxyBeanPostProcessor;

	@Configuration
	public class Configs {

		@Bean
		public QuickPerfProxyBeanPostProcessor dataSourceBeanPostProcessor() {
			return new QuickPerfProxyBeanPostProcessor();
		}

	}
```
	
*Addition of a QuickPerfProxyBeanPostProcessor bean in a Spring XML file*
```xml
<bean id="QuickPerfProxyBeanPostProcessor" class = "org.quickperf.spring.sql.QuickPerfProxyBeanPostProcessor" />
```

## Project example with Spring Boot and JUnit 4
See the code of [this repository](https://github.com/quick-perf/quickperf-examples/tree/master/springboot-junit4).

# Spring and JUnit 5

## Configure your project for QuickPerf JUnit 5

Please follow the instructions described in [this page](https://github.com/quick-perf/doc/wiki/JUnit-5) to can use [JVM](https://github.com/quick-perf/doc/wiki/JVM-annotations) and [core](https://github.com/quick-perf/doc/wiki/Core-annotations) annotations.

See the instructions described below to be able to use SQL annotations. 

## Configure your project to use SQL annotations

You have to add the following dependency:

```xml
        <dependency>
            <groupId>org.quickperf</groupId>
            <artifactId>quick-perf-sql-spring5</artifactId>
            <scope>test</scope>
        </dependency>
```

To can use [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations), you have to add a *QuickPerfProxyBeanPostProcessor* bean.<br>
To do this, you can look at the code examples below or [this Spring Boot project](https://github.com/quick-perf/quickperf-examples/tree/master/springboot-junit4).<br> 

After that, you can evaluate the SQL properties of the database repositories, the Spring services or the Spring controller.

*Addition of a QuickPerfProxyBeanPostProcessor bean with @Bean*
```java
        import org.quickperf.spring.sql.QuickPerfProxyBeanPostProcessor;
        import org.springframework.context.annotation.Bean;
        import org.springframework.context.annotation.Configuration;

        @Configuration
	public class Configs {

		@Bean
		public QuickPerfProxyBeanPostProcessor dataSourceBeanPostProcessor() {
			return new QuickPerfProxyBeanPostProcessor();
		}

	}
```

## Project example with Spring Boot and JUnit 5
See the code of [this repository](https://github.com/quick-perf/quickperf-examples/tree/master/springboot-junit5).

# Spring and TestNG
You can't yet use QuickPerf annotations when Spring is used together with TestNG. If you are interested by this feature, you can help us to develop it. Don't hesitate to leave a comment on [this issue](https://github.com/quick-perf/quickperf/issues/44).