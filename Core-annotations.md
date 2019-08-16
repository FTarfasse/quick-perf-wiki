# 🚩 Table of contents
[@DisplayAppliedAnnotations](#DisplayAppliedAnnotations) <br><br>
[@DisableGlobalAnnotations](#DisableGlobalAnnotations) <br><br>
[@DisableQuickPerf](#DisableQuickPerf) <br><br>
[@FunctionalIteration](#FunctionalIteration) <br><br>
[@DebugQuickPerf](#DebugQuickPerf) <br><br>

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
Disable QuickPerf features.<br><br>
[This worflow example](SQL-annotations#Worflow-with-SQL-annotations) illustrates how to use  @DisableQuickPerf with SQL annotations.

### :wrench: Parameters 
|Parameter|Type    | Meaning                         | Default value  |
| --------|:------:|:--------------------------------|:--------------:|
| comment | String |Comment why QuickPerf is disabled|      -         |

## @FunctionalIteration
Disable QuickPerf features.<br><br>
[This worflow example](SQL-annotations#Worflow-with-SQL-annotations) illustrates how to use @FunctionalIteration with SQL annotations.

## @DebugQuickPerf
This annotation is addressed to developers working on QuickPerf annotations.<br>
It displays information in console for debugging purpose.