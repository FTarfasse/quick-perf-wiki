⚠️ **_JUnit 5 >= 5.6.0 is required_**

You can use JUnit 5 and QuickPerf [BOM files](https://dzone.com/articles/the-bill-of-materials-in-maven).

In the case of Maven, you can use the following dependency management:

```xml
 <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.junit</groupId>
                <artifactId>junit-bom</artifactId>
                <version>5.6.2</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.quickperf</groupId>
                <artifactId>quick-perf-bom</artifactId>
                <version>1.0.0-RC7</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
```

Then, add the following QuickPerf dependency in your POM file:
```xml
 <dependency>
     <groupId>org.quickperf</groupId>
     <artifactId>quick-perf-junit5</artifactId>
     <scope>test</scope>
 </dependency>
```

You can use the [core](https://github.com/quick-perf/doc/wiki/Core-annotations) and [JVM](https://github.com/quick-perf/doc/wiki/JVM-annotations) annotations by adding `@QuickPerfTest` on the test class.

QuickPerf / JUnit 5 project examples:
* [jvm-junit5](https://github.com/quick-perf/quickperf-examples/tree/master/jvm-junit5)
* [hibernate-junit5](https://github.com/quick-perf/quickperf-examples/tree/master/hibernate-junit5)
* [springboot-junit5](https://github.com/quick-perf/quickperf-examples/tree/master/springboot-junit5)
* [testcontainers-springboot-junit5](https://github.com/quick-perf/quickperf-examples/tree/master/tc-springboot-junit5)
* [micronaut-hibernate-jpa](https://github.com/quick-perf/quickperf-examples/tree/master/micronaut-hibernate-jpa)
* [micronaut-data-jdbc](https://github.com/quick-perf/quickperf-examples/tree/master/micronaut-data-jdbc)
* [quarkus](https://github.com/quick-perf/quickperf-examples/tree/master/quarkus)