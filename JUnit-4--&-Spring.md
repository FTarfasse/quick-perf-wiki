## Dependency
Following your Spring version, you have to add one of the dependencies below.

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
## QuickPerfSpringRunner
**QuickPerfSpringRunner** adds QuickPerf features to SpringRunner (also called SpringJUnit4ClassRunner). <br>

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