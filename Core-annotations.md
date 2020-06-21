# 🚩 Table of contents
[@MeasureExecutionTime](#MeasureExecutionTime) <br><br>
[@ExpectMaxExecutionTime](#ExpectMaxExecutionTime) <br><br>
[@DisplayAppliedAnnotations](#DisplayAppliedAnnotations) <br><br>
[@DisableGlobalAnnotations](#DisableGlobalAnnotations) <br><br>
[@DisableQuickPerf](#DisableQuickPerf) <br><br>
[@FunctionalIteration](#FunctionalIteration) <br><br>
[@DebugQuickPerf](#DebugQuickPerf) <br><br>

## @MeasureExecutionTime
Measure execution time of test method.

### :mag_right: Example
```
[QUICK PERF] Execution time of test method: 1 s 1 ms (1 001 016 700 ns)
```

## @ExpectMaxExecutionTime

The test will fail if the execution time is greater than expected.

### :wrench: Parameters 
|Parameter |Type                       | Meaning    | Default value |
| --------   |:-------------------------:|:----------:|:-------------:|
| nanoSeconds| long                    |   |  0   |
| microSeconds| long                    |   |  0   |
| milliSeconds| int|   |  0   |
| seconds| int|   |  0   |
| minutes| int|   |  0   |
| hours| int|   |  0   |

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
[QUICK PERF] Applied annotations: @JdbcBatches(batchSize=30), @DisableSameSelectTypesWithDifferentParams
             Class specifying global annotations: org.quickperf.QuickPerfConfiguration
```

## @DisableGlobalAnnotations
Disable global annotations on test method or test class.

### :wrench:  Parameters 
|Parameter|Type    | Meaning                                   | Default value  |
| --------|:------:|:----------------------------------------  |:--------------:|
| comment | String |Comment why global annotations are disabled|      -         |


## @DisableQuickPerf
Disable QuickPerf features.

### :wrench: Parameters 
|Parameter|Type    | Meaning                         | Default value  |
| --------|:------:|:--------------------------------|:--------------:|
| comment | String |Comment why QuickPerf is disabled|      -         |

## @FunctionalIteration
Disable QuickPerf features.

## @DebugQuickPerf
This annotation is addressed to developers working on QuickPerf annotations.<br>
It displays information in console for debugging purpose.