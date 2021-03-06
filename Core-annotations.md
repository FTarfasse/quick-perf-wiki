# 🚩 Table of contents
[@MeasureExecutionTime](#MeasureExecutionTime) <br><br>
[@ExpectMaxExecutionTime](#ExpectMaxExecutionTime) <br><br>
[@DisplayAppliedAnnotations](#DisplayAppliedAnnotations) <br><br>
[@DisableGlobalAnnotations](#DisableGlobalAnnotations) <br><br>
[@DisableQuickPerf](#DisableQuickPerf) <br><br>
[@FunctionalIteration](#FunctionalIteration) <br><br>
[@DebugQuickPerf](#DebugQuickPerf) <br><br>
[Configure core annotations with a global scope](#Configure-core-annotations-with-a-global-scope)
## @MeasureExecutionTime
Measures the execution time of the test method.

### :mag_right: Example
```
[QUICK PERF] Execution time of the test method: 5 s 289 ms (5 289 245 600 ns)
```

⚠️ *Be cautious  with time measurement results. It is a rough and first level result. Data has no meaning below the ~second/millisecond. JIT warm-up, GC, or [safe points](https://loonytek.com/2020/01/20/long-jvm-pauses-without-gc/) can impact the measure and its reproducibility. We recommend [JMH](https://openjdk.java.net/projects/code-tools/jmh/) to do more in-depth experiments.* 


## @ExpectMaxExecutionTime

The test will fail if the execution time exceeds the maximum expected.

It can be useful to [configure _@ExpectMaxExecutionTime_ with a global scope](https://github.com/quick-perf/doc/wiki/Core-annotations#mag_right-example-3).

⚠️ *Be cautious  with time measurement results. It is a rough and first level result. Data has no meaning below the ~second/millisecond. JIT warm-up, GC, or [safe points](https://loonytek.com/2020/01/20/long-jvm-pauses-without-gc/) can impact the measure and its reproducibility. We recommend [JMH](https://openjdk.java.net/projects/code-tools/jmh/) to do more in-depth experiments.* 

### :wrench: Elements
|Name          |Type    | Meaning                 | Default value |
| ------------ |:------:|-------------------------|:-------------:|
| hours        | int    | Number of hours         |  0            |
| minutes      | int    | Number of minutes       |  0            |
| seconds      | int    | Number of seconds       |  0            |
| milliSeconds | int    | Number of milli seconds |  0            |

You can use several @ExpectMaxExecutionTime elements together, as shown in the following example.

### :mag_right: Example
```java
@ExpectMaxExecutionTime(seconds = 2)
```     

```
[PERF] Execution time of the test method expected to be less than <2 s> but is <5 s 286 ms (5 285 734 000 ns)>
```  

## @DisplayAppliedAnnotations
Displays applied QuickPerf annotations in the console.<br><br>
An annotation can have three [scopes](https://github.com/quick-perf/doc/wiki/QuickPerf#Use-QuickPerf-annotations) (*global*, *test class*, *test method*). _@DisplayAppliedAnnotations_ is useful to see which annotations are applied on a test method.

### :mag_right: Example
```
[QUICK PERF] Applied annotations: @JdbcBatches(batchSize=30), @DisableSameSelectTypesWithDifferentParamValues
             Class specifying global annotations: org.quickperf.QuickPerfConfiguration
```

## @DisableGlobalAnnotations
Disables global annotations on test methods or test classes.

### :wrench:  Elements
|Name     |Type    | Meaning                                   | Default value  |
| --------|:------:|:----------------------------------------  |:--------------:|
| comment | String |Comment why global annotations are disabled|      -         |


## @DisableQuickPerf
Disables QuickPerf features.

### :wrench: Elements
|Name     |Type    | Meaning                         | Default value  |
| --------|:------:|:--------------------------------|:--------------:|
| comment | String |Comment why QuickPerf is disabled|      -         |

## @FunctionalIteration
Disables QuickPerf features.

## @DebugQuickPerf
QuickPerf developers can use _@DebugQuickPerf_ to display debugging information in the console.

## Configure core annotations with a global scope
Annotations having a [global scope](https://github.com/quick-perf/doc/wiki/QuickPerf#annotation-scopes) apply on each test.

`org.quickperf.annotation.CoreAnnotationBuilder` helps to configure core annotations with a global scope.

### :mag_right: Example

```java
package org.quickperf;

import org.quickperf.annotation.CoreAnnotationBuilder;
import org.quickperf.config.SpecifiableGlobalAnnotations;

import java.lang.annotation.Annotation;
import java.util.Arrays;
import java.util.Collection;

public class QuickPerfConfiguration implements SpecifiableGlobalAnnotations {

    public Collection<Annotation> specifyAnnotationsAppliedOnEachTest() {

        return Arrays.asList(
          
                CoreAnnotationBuilder.expectMaxExecutionTimeOfSeconds(1)

        );

    }

}
```

⚠️ *Reminder: the class implementing `SpecifiableGlobalAnnotations` has to be in org.quickperf package.*