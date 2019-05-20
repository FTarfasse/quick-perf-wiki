To use [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations) in Spring tests, you need *[QuickPerfSpringRunner](https://github.com/quick-perf/doc/wiki/JUnit-4--&-Spring)* and an instance of *QuickPerfProxyBeanPostProcessor* in your spring context. Look at the example below.<br> 

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
	
	@RunWith(QuickPerfSpringRunner.class)
	@SpringBootTest(classes = { //...,
                                    QuickPerfProxyBeanPostProcessor.class
                          }
                )
	public class PlayerControllerTest {

            @SelectNumber(1)
            @Test
            public void should_find_one_player() {
                //...
            }
		
	}
```