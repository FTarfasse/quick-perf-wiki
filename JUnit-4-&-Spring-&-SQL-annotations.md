## Outline
[**Dependencies**](#Dependencies)<br>

[**Java configuration**](#Java-configuration)<br>

[**Spring Boot examples**](#Spring-Boot-examples)

## Dependencies
To use [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations) in Spring tests, you need first of all to add a QuickPerf Spring dependency (following your Spring version: *quick-perf-junit4-spring3*, *quick-perf-junit4-spring4*, *quick-perf-junit4-spring5*) and a *quick-perf-sql* dependency:<br>
...

## Java configuration
You need to:
* annotate your test class with @RunWith(QuickPerfSpringRunner.class) 
* suplly an instance of *QuickPerfProxyBeanPostProcessor* to your spring context

Look at the example below or in [this Spring Boot project](https://github.com/quick-perf/springboot-junit4-examples).<br> 

After that, you can evaluate the SQL properties of your database repositories, of your services or of your web services.

### Example
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

## [Spring Boot examples](https://github.com/quick-perf/springboot-junit4-examples)