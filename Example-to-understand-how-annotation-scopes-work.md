```java
package org.quickperf;

import org.quickperf.config.user.SpecifiableAnnotations;
import org.quickperf.sql.annotation.SqlAnnotationBuilder;

import java.lang.annotation.Annotation;
import java.util.Collection;
import java.util.Collections;

/*The configuration class has to be in org.quickperf package*/
public class QuickPerfConfiguration implements SpecifiableAnnotations {

    public Collection<Annotation> specifyDefaultAnnotations() {

        /* To build an instance of MaxSqlSelect annotation without SqlAnnotationBuilder
        MaxSqlSelect maxSqlSelect = new MaxSqlSelect() {
            @Override
            public Class<? extends Annotation> annotationType() {
                return MaxSqlSelect.class;
            }
            @Override
            public int value() {
                return 3;
            }
        };
        */

        Annotation maxSqlSelect = SqlAnnotationBuilder.maxSqlSelect(3);

        return Collections.singletonList(maxSqlSelect);

    }

}
```

```java
import org.junit.Test;

public class AClassWithGlobalScopeAnnotationAppliedTest {

     //@MaxSqlSelect(3) default annotation is applied
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

@MaxSqlSelect(2) // CLASS SCOPE
                 // This annotation overrides the annotation
                 // defined in QuickPerfConfiguration class (GLOBAL SCOPE)
public class AClassWithAnnotationsTest {

    // @MaxSqlSelect(2) annotation placed on class is applied
    @Test
    public void a_test_method() {
        //...
    }

    @MaxSqlSelect(1) // METHOD SCOPE
                     // This annotation overrides the annotation placed
                     // on class
    @Test
    public void a_test_method_with_quick_perf_annotation() {
        //...
    }

}
```
