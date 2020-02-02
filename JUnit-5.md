⚠️ You need JUnit 5.6.0.

You can use the follwing BOM file to configure your project with JUnit 5 and QuickPerf:
```xml
 <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.junit</groupId>
                <artifactId>junit-bom</artifactId>
                <version>5.6.0</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.quickperf</groupId>
                <artifactId>quick-perf-bom</artifactId>
                <version>1.0.0-RC6</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
```

Add the following QuickPerf dependency in your POM file:
```xml
 <dependency>
     <groupId>org.quickperf</groupId>
     <artifactId>quick-perf-junit5</artifactId>
     <scope>test</scope>
 </dependency>
```
By adding `@QuickPerfTest` on the test class, [core](https://github.com/quick-perf/doc/wiki/Core-annotations) and [JVM](https://github.com/quick-perf/doc/wiki/JVM-annotations) annotations should work.