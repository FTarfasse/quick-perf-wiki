Use **QuickPerfSpringRunner** which adds QuickPerf functionalities to SpringRunner (SpringJUnit4ClassRunner). <br>

### Example
```java
	import org.junit.runner.RunWith;
	import quickperf.spring.QuickPerfSpringRunner;

	@RunWith(QuickPerfSpringRunner.class)
	public class AccountTest {

	}
```
*Each test is executed in a specific JVM with the QuickPerfSpringRunner.* <br>
You can use some [JVM annotations](https://github.com/quick-perf/doc/wiki/JVM-annotations) to configure the JVM.

## [QuickPerfSpringRunner & SQL annotations](https://github.com/quick-perf/doc/wiki/QuickPerfSpringRunner-&-SQL-annotations)