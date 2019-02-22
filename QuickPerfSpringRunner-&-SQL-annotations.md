If you use the [QuickPerfSpringRunner](https://github.com/quick-perf/doc/wiki/JUnit-4--&-Spring) and [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations), you need to have an instance of **QuickPerfProxyBeanPostProcessor** in your spring context.

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
	
	@RunWith(QuickPerfSpringRunner.class)
	@SpringBootTest(classes = { //...,
                           QuickPerfProxyBeanPostProcessor.class
                          }
                )
	public class PlayerControllerTest {
		
	}
```