To use [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations) in Spring tests, you need first of all to add a QuickPerf Spring dependency (following your Spring version: *quick-perf-junit4-spring3*, *quick-perf-junit4-spring4*, *quick-perf-junit4-spring5*) and a *quick-perf-sql* dependency:<br>
...

You also need to:
* annotate your test class with @RunWith(QuickPerfSpringRunner.class) 
* suplly an instance of *QuickPerfProxyBeanPostProcessor* to your spring context. 

Look at the example below.<br> 

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