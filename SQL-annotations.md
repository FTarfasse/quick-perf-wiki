***Take control of SQL requests sent to the database.***

[**Configuration**](#Configuration)<br>

[**Worflow**](#Worflow)<br>

[**Recommended global annotations**](#Recommended-global-annotations)<br>

[**Disable some global annotations**](#Disable-some-global-annotations)<br>

[**Recommended method annotations**](#Recommended-method-annotations)<br>

[**Debug annotations**](#Debug-annotations)<br>

# Configuration
[Configuration with JUnit 4 (without Spring)](https://github.com/quick-perf/doc/wiki/QuickPerfRunner-&-SQL-annotations)<br><br>
[Configuration with JUnit 4 and Spring](https://github.com/quick-perf/doc/wiki/QuickPerfSpringRunner-&-SQL-annotations)

The SQL annotations automatically detect if you use *Hibernate* or *Spring Boot* framewoks. If a SQL property is not respected, the SQL annotations can suggest you solutions to fix it.

# Worflow
Below, we try to propose a way to efficiently use SQL annotations during development.<br>

Firstly, we focus our work and attention on the functional behavior. The goal is to have something working without worrying about performances. *We try to do one thing at a time.* After, we check some performance properties.

## Configure global annotations
Configure once some [recommended global annotations](#Recommended-global-annotations). These annotations are applied to every test method.<br> The idea is to systematically apply some performance checks to avoid some classical performance bottlenecks.

## Implementation of a new business use case
### **Work on functional behavior** <br>
* **Write a test describing and verifying the *functional behavior*** <br> 
* **Annotate this test with *@DisableQuickPerf* or *@FunctionalIteration* to disable the QuickPerf annotations** <br>So, we disable annotations having global or class scopes. <br>
* **Make the functional behavior working** <br>
  You can do this applying a TDD approach (Red/Green/Refactor).
### **Work on performance behavior**
* **Remove @DisableQuickPerf or @FunctionalIteration to enable QuickPerf annotations** 
* **Fix or ignore issues reported by global annotations**
<br>In some specific cases, you can [disable some global annotations](#Disable-some-global-annotations).
* **Possibly add QuickPerf annotation on method to document the code** 

## Add some performance checks to existing tests

# Recommended global annotations

## Configure recommended global annotations
A SqlAnnotationBuilder class is available to easily implement SpecifiableAnnotations.

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
        int batchSize = 30; // set the expected batch size
        return Arrays.asList(  disableExactlySameSqlSelects()
                             , disableSameSelectTypesWithDifferentParams()
                             , jdbcBatching(batchSize)
                             , disableSqlCrossJoin()
                             , disableLikeStartingWithWildcard()
                             );
    }

}
```
**The class implementing SpecifiableAnnotations has to be in org.quickperf package.**

## @DisableExactlySameSqlSelects

## @DisableSameSelectTypesWithDifferentParams

## @JdbcBatching

Verify that inserts and updates are processed in JDBC batches.

You may sometimes think that you are using JDBC batching but in fact not ([Paper 1](https://abramsm.wordpress.com/2008/04/23/hibernate-batch-processing-why-you-may-not-be-using-it-even-if-you-think-you-are/), [Paper 2](https://stackoverflow.com/questions/27697810/hibernate-disabled-insert-batching-when-using-an-identity-identifier)).

### Parameters 
|Parameter  |Type| Meaning           | Default value  |
| -------- |:---:|:-----------------:|:--------------:|
| batchSize| int |JDBC batch size    |      -         |

A 0 batch size means that JDBC batching is disabled.

### Example
```java
    @JdbcBatching(batchSize = 30)
```

## @DisableSqlCrossJoin
 The [cartesian product induced by a cross join can be very inefficient](https://vladmihalcea.com/hibernate-facts-always-check-criteria-api-sql-queries/). Although most database engines will try to remove a cross join, we can decide to remove cross join to not have to check if a database engine version will really remove it.
 
## @DisableLikeStartingWithWildcard

# Disable some global annotations

## @EnableExactlySameSqlSelects
Cancel behavior of [@DisableExactlySameSqlSelects](#DisableExactlySameSqlSelects).

## @EnableSameSelectTypesWithDifferentParams
Cancel behavior of [@DisableSameSelectTypesWithDifferentParams](#DisableSameSelectTypesWithDifferentParams).

## @EnableSqlCrossJoin
Cancel behavior of [@DisableSqlCrossJoin](#DisableSqlCrossJoin).
To decide to enable a cross join in a specific case if you add @DisableSqlCrossJoin check for every test or at test class level.

## @EnableLikeStartingWithWildcard
Cancel behavior of [@DisableLikeStartingWithWildcard](#DisableLikeStartingWithWildcard).

# Recommended method annotations
## @SqlSelectNumber

### Parameters 
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Number of select requests  |        0       |

### Example
```java
    @SqlSelectNumber(1)
    @Test
    public void should_retrieve_all_cars() {	
     //...
    }
```

## @SqlInsertNumber

### Parameters 
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Number of insert requests  |        0       |


## @SqlUpdateNumber

### Parameters 
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Number of update requests  |        0       |

## @SqlDeleteNumber

|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Number of delete requests  |        0       |


## @MaxSqlSelect
With this annotation, the test will fail if the number of SELECT requests is greater than expected. 
### Parameters 
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Maximum number of selects  |        0       |

### Example
```java
    @MaxSqlSelect(1)
    @Test
    public void should_retrieve_all_cars() {	
     //...
    }
```
## @MaxSqlInsert
With this annotation, the test will fail if the number of INSERT requests is greater than expected. 
### Parameters
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Maximum number of inserts  |        0       |
## @MaxSqlUpdate
With this annotation, the test will fail if the number of UPDATE requests is greater than expected. 
### Parameters
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Maximum number of updates  |        0       |
## @MaxSqlDelete
With this annotation, the test will fail if the number of DELETE requests is greater than expected. 
### Parameters
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Maximum number of deletes  |        0       |

## @MaxReturnedSqlColumns

With this annotation, the test will fail if the number of returned columns is greater than expected.
### Parameters 
|Parameter  |Type| Meaning                             | Default value  |
| --------  |:---:|:----------------------------------:|:--------------:|
| value     | int |Maximum number of returned columns  |        0       |
### Example
```java
    @MaxReturnedSqlColumns(5)
```




# Debug annotations
## [@DisplayAppliedAnnotations](https://github.com/quick-perf/doc/wiki/Base-annotations)

## @DisplaySql
With this annotation the SQL order are diplayed in the console during the test execution.<br>
<br>
This annotation is useful during *debugging*. <br>
<br>
*It is not recommended to commit your test with this annotation. Indeed, the displaying of SQL orders would pollute the logs of your continuous integration build and it may slow down your continuous integration build.*

## @DisplaySqlOfTestMethodBody
With this annotation the SQL order are diplayed in the console during the execution of the test method body, not just after if a SQL property is not respected. <br>
Compared to @DisplaySql, this annotation does not diplay SQL order before (JUnit 4: @Before, @BeforeClass) and after (JUnit 4: @After, @AfterClass) the execution of the test method body. <br>
<br>
This annotation is useful during *debugging*. <br>
<br>
*It is not recommended to commit your test with this annotation. Indeed, the displaying of SQL orders would pollute the logs of your continuous integration build and it may slow down your continuous integration build.*


