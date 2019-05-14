## @DisplayAppliedAnnotations
Display applied QuickPerf annotations in console.<br><br>
An annotation can have three [scopes](https://github.com/quick-perf/doc/wiki/QuickPerf#Use-QuickPerf-annotations) (*gobal*, *test class*, *test method*). This annotation is useful to see which are the annotations applied on a test method.


### Example
In console:
```
[QUICK PERF] Applied annotations: @JdbcBatches(batchSize=30), @DisableSameSelectTypesWithDifferentParams
             Class specifying global annotations: org.quickperf.QuickPerfConfiguration
```

## @DisableGlobalAnnotations
To disable global annotations on test method or test class.

## @DisableQuickPerf
To disable QuickPerf functionalities.<br><br>
[This worflow example](SQL-annotations#Worflow) illustrates how to use  @DisableQuickPerf with SQL annotations.

### Parameters 
|Parameter|Type    | Meaning                        | Default value  |
| --------|:------:|:-------------------------------|:--------------:|
| reason  | String |Reason why QuickPerf is disabled|      -         |

## @FunctionalIteration
To disable QuickPerf functionalities.<br><br>
[This worflow example](SQL-annotations#Worflow) illustrates how to use @FunctionalIteration with SQL annotations.

## @DebugQuickPerf
This annotation is addressed to developers working on the conception of QuickPerf annotations.<br>
It displays information in console for debugging purpose.