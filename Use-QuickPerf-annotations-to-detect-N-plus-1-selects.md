# The problem

# How to detect N+1 select with QuickPerf annotations
A N+1 select will lead to have at most N select with different parameters.
So, you can use the [@DisableSameSelectTypesWithDifferentParams](https://github.com/quick-perf/doc/wiki/SQL-annotations#DisableSameSelectTypesWithDifferentParams) annotation with a global scope (the annotation is applied to each test) to try to detect them.

[@SelectNumber](https://github.com/quick-perf/doc/wiki/SQL-annotations#SelectNumber)

```java
package org.quickperf;

import org.quickperf.config.user.SpecifiableAnnotations;
import org.quickperf.sql.annotation.SqlAnnotationBuilder;

import java.lang.annotation.Annotation;
import java.util.Arrays;
import java.util.Collection;

import static org.quickperf.sql.annotation.SqlAnnotationBuilder.*;

public class QuickPerfConfiguration implements SpecifiableAnnotations {

    public Collection<Annotation> specifyAnnotationsAppliedOnEachTest() {
        return Arrays.asList(disableSameSelectTypesWithDifferentParams() // can some reveal N+1 selects
                            );
    }

}
```
***The class implementing SpecifiableAnnotations has to be in org.quickperf package.***