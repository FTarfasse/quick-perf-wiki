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
Measure execution time of test method.

### :mag_right: Example
```
[QUICK PERF] Execution time of test method: 1 s 1 ms (1 001 016 700 ns)
```

⚠️ *Be cautious  with time measurement results. It is a rough and first level result. Data has no meaning below the seconde / millisecond. JIT warm up, GC or safe points can impact the measure and its reproducibility. We recommend [JMH](https://openjdk.java.net/projects/code-tools/jmh/) to do more in depth experiments.* 


## @ExpectMaxExecutionTime

The test will fail if the execution time is greater than expected.

It can be useful to [configure this annotation with a global scope](https://github.com/quick-perf/doc/wiki/Core-annotations#mag_right-example-3).

⚠️ *Be cautious  with time measurement results. It is a rough and first level result. Data has no meaning below the seconde / millisecond. JIT warm up, GC or safe points can impact the measure and its reproducibility. We recommend [JMH](https://openjdk.java.net/projects/code-tools/jmh/) to do more in depth experiments.* 

### :wrench: Elements
|Name          |Type    | Meaning                 | Default value |
| ------------ |:------:|-----------------------|:-------------:|
| nanoSeconds  | long   | Number of nano seconds  |  0            |
| microSeconds | long   | Number of micro seconds |  0            |
| milliSeconds | int    | Number of milli seconds |  0            |
| seconds      | int    | Number of seconds       |  0            |
| minutes      | int    | Number of minutes       |  0            |
| hours        | int    | Number of hours        |  0            |


You can use several elements together, as shown in the following example.

### :mag_right: Example
```java
@ExpectMaxExecutionTime(seconds = 1, milliSeconds = 10)
```     

```
[PERF] Execution time of test method expected to be less than <1 s 10 ms> but is <9 s 502 ms (9 501 875 600 ns)>
```  

## @DisplayAppliedAnnotations
Display applied QuickPerf annotations in console.<br><br>
An annotation can have three [scopes](https://github.com/quick-perf/doc/wiki/QuickPerf#Use-QuickPerf-annotations) (*gobal*, *test class*, *test method*). This annotation is useful to see which annotations are applied on a test method.

### :mag_right: Example
```
[QUICK PERF] Applied annotations: @JdbcBatches(batchSize=30), @DisableSameSelectTypesWithDifferentParamValues
             Class specifying global annotations: org.quickperf.QuickPerfConfiguration
```

## @DisableGlobalAnnotations
Disable global annotations on test method or test class.

### :wrench:  Elements
|Name     |Type    | Meaning                                   | Default value  |
| --------|:------:|:----------------------------------------  |:--------------:|
| comment | String |Comment why global annotations are disabled|      -         |


## @DisableQuickPerf
Disable QuickPerf features.

### :wrench: Elements
|Name     |Type    | Meaning                         | Default value  |
| --------|:------:|:--------------------------------|:--------------:|
| comment | String |Comment why QuickPerf is disabled|      -         |

## @FunctionalIteration
Disable QuickPerf features.

## @DebugQuickPerf
This annotation is addressed to developers working on QuickPerf annotations.<br>
It displays information in console for debugging purpose.

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
          
                CoreAnnotationBuilder.expectMaxExecutionTimeOfMilliSeconds(500)

        );

    }

}
```

⚠️ **The class implementing `SpecifiableGlobalAnnotations` has to be in org.quickperf package.**