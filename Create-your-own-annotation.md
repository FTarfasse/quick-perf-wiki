In this page, you will learn the principles to create a QuickPerf annotation from scratch. So, after reading it, you should be able to create a QuickPerf annotation in the QuickPerf project or in your own project. In addition, you will learn how to create your own Maven module defining your custom QuickPerf annotations.

# 🚩 Table of contents
[Principles to create a QuickPerf annotation](#Principles-to-create-a-QuickPerf-annotation)
* [Declare your SPI implementation](#Declare-your-SPI-implementation)
* [QuickPerfConfigLoader implementation](#QuickPerfConfigLoader-implementation)
* [Configuration of an annotation](#Configuration-of-an-annotation)

[Define your custom annotations in a specific Maven module](#Define-your-custom-annotations-in-a-specific-Maven-module)

[Test your new annotation](#Test-your-new-annotation)
* [Test your new annotation with JUnit 4](#Test-your-new-annotation-with-JUnit-4)

[Debug an annotation](#Debug-an-annotation)

# Principles to create a QuickPerf annotation

*The code examples below come from [sql-annotations Maven module](https://github.com/quick-perf/quickperf/tree/master/sql-annotations).*

## Declare your SPI implementation
QuickPerf uses the Service Provider Interface ([SPI](https://docs.oracle.com/javase/tutorial/sound/SPI-intro.html)) to dynamically load the configurations of annotations.

A file named *org.quickperf.config.library.QuickPerfConfigLoader* has to be in src/main/resources/META-INF/services:

<img src="https://github.com/quick-perf/doc/blob/master/doc/images/QuickPerfConfigLoader.png">
<br><br>

The Content of org.quickperf.config.library.QuickPerfConfigLoader file is here: ```org.quickperf.sql.config.library.SqlConfigLoader```

## QuickPerfConfigLoader implementation 

An implementation of QuickPerfConfigLoader has to define three methods:
* *loadAnnotationConfigs()*: returns the configurations of annotations
* *loadRecorderExecutionOrdersBeforeTestMethod()*: returns the recorder priorities *before* the execution of a test method
* *loadRecorderExecutionOrdersAfterTestMethod()*: returns the recorder priorities *after* the execution of a test method

```java
public class SqlConfigLoader implements QuickPerfConfigLoader {

 @Override
    public Collection<AnnotationConfig> loadAnnotationConfigs() {
        return Arrays.asList(
                // ..
                , SqlAnnotationsConfigs.DISABLE_EXACTLY_SAME_SQL_SELECTS
                // ...
        );
    }

    @Override
    public Collection<RecorderExecutionOrder> loadRecorderExecutionOrdersBeforeTestMethod() {
        return Arrays.asList(
                  new RecorderExecutionOrder(PersistenceSqlRecorder.class, 2000)
        );
    }

    @Override
    public Collection<RecorderExecutionOrder> loadRecorderExecutionOrdersAfterTestMethod() {
        return Arrays.asList(
                 new RecorderExecutionOrder(PersistenceSqlRecorder.class, 7000)
        );
    }

}
```

## Configuration of an annotation

The configuration of an annotation is defined with the help of an instance of AnnotationConfig.Builder().

```java
class SqlAnnotationsConfigs {

	static final AnnotationConfig DISABLE_EXACTLY_SAME_SQL_SELECTS = new AnnotationConfig.Builder()
			.perfRecorderClass(PersistenceSqlRecorder.class)
			.perfMeasureExtractor(HasExactlySameSelectExtractor.INSTANCE)
			.perfIssueVerifier(HasExactlySameSelectVerifier.INSTANCE)
			.build(DisableExactlySameSelects.class);
      // ...
}
```
Let's explain the methods used in the example above:
* *perfRecorderClass*: to specify a performance recorder, for example the set of SQL statements sent to database during the execution of the test method body 
* *perfMeasureExtractor*: to define the way a performance measure is extracted from the recorder, for example if there exists exactly same SELECT statements sent to the database
* *perfIssueVerifier*: to evaluate if a performance issue exists
* *build*: the annotation for which the annotation configuration is built

You can use *testHasToBeLaunchedInASpecificJvm()* method to specify that the test method has to be executed in a specific JVM. A *testHasToBeLaunchedInASpecificJvm(AnnotationToJvmOptionConverter annotationToJvmOptionConverter)* method is also available to add some options to the JVM. 

An example for @Xmx:
```java
   static final AnnotationConfig XMX = new AnnotationConfig.Builder()
            .testHasToBeLaunchedInASpecificJvm(XmxAnnotToJvmOptionConverter.INSTANCE)
            .build(Xmx.class);
```
The code of XmxAnnotToJvmOptionConverter can be found [here](https://github.com/quick-perf/quickperf/blob/master/jvm-annotations/src/main/java/org/quickperf/jvm/config/library/XmxAnnotToJvmOptionConverter.java).

# Define your custom annotations in a specific Maven module
You can develop custom QuickPerf annotations and gather them in a Maven module. To use them, you simply have to use this Maven dependency together with one QuickPerf dependency (see [here](https://github.com/quick-perf/doc/wiki/JUnit-4) for JUnit4 or [here](https://github.com/quick-perf/doc/wiki/Spring) for Spring). Don't hesitate to propose a [feature request](https://github.com/quick-perf/quickperf/issues/new?assignees=&labels=enhancement&template=feature_request.md&title=) and a PR to integrate your annotations in the QuickPerf project!


# Test your new annotation
## Test your new annotation with JUnit 4
[Example of JUnit4 test class](https://github.com/quick-perf/quickperf/blob/master/junit4-sql-test/src/test/java/org/quickperf/sql/DisableExactlySameSqlSelectTest.java).

# Debug an annotation
You can add [@DebugQuickPerf](https://github.com/quick-perf/doc/wiki/Core-annotations#DebugQuickPerf) on a test method to get debug data.