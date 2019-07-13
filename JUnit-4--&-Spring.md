# Outline
[**QuickPerfSpringRunner**](#QuickPerfSpringRunner)<br>

[**SQL annotations**](#SQL-annotations)<br>

[**Spring Boot examples**](#Spring-Boot-examples)

# QuickPerfSpringRunner
**QuickPerfSpringRunner** adds QuickPerf features to *SpringRunner* (also called *SpringJUnit4ClassRunner*). <br>

To use it, following your Spring version, you have to add one of the dependencies below.

**Spring 5**
```xml
<dependency>
  <groupId>org.quickperf</groupId>
  <artifactId>quick-perf-junit4-spring5</artifactId>
  <version>1.0-SNAPSHOT</version>
</dependency>
```

**Spring 4**
```xml
<dependency>
  <groupId>org.quickperf</groupId>
  <artifactId>quick-perf-junit4-spring4</artifactId>
  <version>1.0-SNAPSHOT</version>
</dependency>
```

**Spring 3**
```xml
<dependency>
  <groupId>org.quickperf</groupId>
  <artifactId>quick-perf-junit4-spring3</artifactId>
  <version>1.0-SNAPSHOT</version>
</dependency>
```

***With one of these dependencies, you have also access to [JVM annotations](https://github.com/quick-perf/doc/wiki/JVM-annotations).***

_QuickPerf annotations are executed after the loading of the SpringContext._ <br>
So, for example, if you profile your JVM with [@ProfileJvm](https://github.com/quick-perf/doc/wiki/JVM-annotations#Profile-or-check-your-JVM), the profiling starts just after the loading of the Spring context.

*Java code example with QuickPerfSpringRunner*
```java
	import org.junit.runner.RunWith;
	import quickperf.spring.QuickPerfSpringRunner;

	@RunWith(QuickPerfSpringRunner.class)
	public class AccountTest {

	}
```

# SQL annotations

In addition to the dependency mentioned in the [QuickPerfSpringRunner](#QuickPerfSpringRunner) part, you have to add the dependency below to can use [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations).
```xml
<dependency>
  <groupId>org.quickperf</groupId>
  <artifactId>quick-perf-sql-annotations</artifactId>
  <version>1.0-SNAPSHOT</version>
</dependency>
```

You also have to suplly an instance of *QuickPerfProxyBeanPostProcessor* to your spring context.<br>
To do this, you can look at the Java code examples below or in [this Spring Boot project](https://github.com/quick-perf/springboot-junit4-examples).<br> 

After that, you can evaluate the SQL properties of the database repositories, the Spring services or the Spring controller.

*Java code examples with QuickPerfSpringRunner and a SQL annotation*
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
	
```java
	import quickperf.spring.QuickPerfProxyBeanPostProcessor;
        import quickperf.spring.QuickPerfSpringRunner;
	
         @SpringBootTest(classes = {FootballApplication.class}
                       , webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT
         )
	public class PlayerControllerTest {

           @ExpectSelect(1)
           @HeapSize(value = 50, unit = AllocationUnit.MEGA_BYTE)
           @Test
           public void should_find_all_players() {

	   }

       }
```

# Spring Boot examples
See the code of [this repository](https://github.com/quick-perf/springboot-junit4-examples).