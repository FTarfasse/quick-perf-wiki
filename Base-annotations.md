## @DisplayAppliedAnnotations
Display applied QuickPerf annotations in console.<br><br>
An annotation can have three [scopes](https://github.com/quick-perf/doc/wiki/QuickPerf#Use-QuickPerf-annotations) (*gobal*, *test class*, *test method*). This annotation is usefull to see which are the annotations applied on a test method.


### Example
In console:
```
[QUICK PERF] Applied annotations: @JdbcBatches(batchSize=30), @DisableSameSelectTypesWithDifferentParams
             Class specifying global annotations: org.quickperf.QuickPerfConfiguration
```

## @DisableGlobalAnnotations
To disable global annotations on test method or test class level.

## @DisableQuickPerf or @FunctionalIteration
To disable QuickPerf functionalities.<br>
[This worflow example](SQL-annotations#Worflow) illustrates how to use  @DisableQuickPerf or @FunctionalIteration with SQL annotations.

## @DebugQuickPerf