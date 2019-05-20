# The problem
Suppose that you have an Hibernate Book entity having a many to one Shef attribute:
```java
@ManyToOne(fetch = FetchType.LAZY)
private Shelf shelf;
```
Imagine we request a set of Book, for example with this query:

```java
List<Book> books = entityManager.createQuery("FROM Book", Book.class)
                                .getResultList();
```


# How to detect N+1 select with QuickPerf annotations
A N+1 select will lead to have at most N select with different parameters.
So, you can use the *@DisableSameSelectTypesWithDifferentParams* annotation with a global scope (the annotation is applied to each test) to try to detect them.

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

*@SelectNumber*

*@MaxOfSelects*