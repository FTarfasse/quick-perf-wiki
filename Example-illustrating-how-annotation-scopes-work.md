A class implementing SpecifiableAnnotations provides global annotations, that is to say annotations applying on each test.
```java
package org.quickperf;

import org.quickperf.config.user.SpecifiableAnnotations;
import org.quickperf.sql.annotation.SqlAnnotationBuilder;

import java.lang.annotation.Annotation;
import java.util.Collection;
import java.util.Collections;

/*The configuration class has to be in org.quickperf package*/
public class QuickPerfConfiguration implements SpecifiableAnnotations {

    public Collection<Annotation> specifyAnnotationsAppliedOnEachTest() {

        /* To build an instance of MaxOfSelects annotation without SqlAnnotationBuilder
        MaxSqlSelect maxOfSelects = new MaxOfSelects() {
            @Override
            public Class<? extends Annotation> annotationType() {
                return MaxOfSelects.class;
            }
            @Override
            public int value() {
                return 3;
            }
        };
        */

        Annotation maxOfSelects = SqlAnnotationBuilder.maxOfSelects(3);

        return Collections.singletonList(maxOfSelects);

    }

}
```

**The class implementing SpecifiableAnnotations has to be in org.quickperf package.**

<p><img src="https://github.com/quick-perf/doc/blob/master/doc/images/Scopes.PNG"</p>


```java
import org.junit.Test;

public class AClassWithGlobalScopeAnnotationAppliedTest {

     //@MaxOfSelects(3) annotation is applied
     @Test
     public void a_test_method() {
         //...
     }

}
```

```java
package org.mycompany;

import org.junit.Test;
import org.quickperf.sql.annotation.MaxSqlSelect;

@MaxOfSelects(2) // CLASS SCOPE
                 // This annotation overrides the annotation
                 // defined in QuickPerfConfiguration class (GLOBAL SCOPE)
public class AClassWithAnnotationsTest {

    // @MaxOfSelects(2) annotation placed on class is applied
    @Test
    public void a_test_method() {
        //...
    }

    @MaxOfSelects(1) // METHOD SCOPE
                     // This annotation overrides the annotation placed
                     // on class
    @Test
    public void a_test_method_with_quick_perf_annotation() {
        //...
    }

}
```
