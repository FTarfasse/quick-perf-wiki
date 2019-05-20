## QuickPerfSpringRunner
**QuickPerfSpringRunner** adds QuickPerf functionalities to SpringRunner (SpringJUnit4ClassRunner). <br>

_QuickPerf annotations are executed after the loading of the SpringContext._ <br>So, for example, if you profile your JVM with [@ProfileJvm](https://github.com/quick-perf/doc/wiki/JVM-annotations#Profile-or-check-your-JVM), the profiling starts just after the loading of the Spring context.



### Example
```java
	import org.junit.runner.RunWith;
	import quickperf.spring.QuickPerfSpringRunner;

	@RunWith(QuickPerfSpringRunner.class)
	public class AccountTest {

	}
```

## [JUnit 4 & Spring & SQL annotations](https://github.com/quick-perf/doc/wiki/JUnit-4-&-Spring-&-SQL-annotations)