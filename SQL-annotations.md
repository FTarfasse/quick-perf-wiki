***Take control of SQL requests sent to the database.***

[**Configuration**](#Configuration)<br>

[**Worflow**](#Worflow)<br>

[**Recommended global annotations**](#Recommended-global-annotations)<br>

[**Disable some global annotations**](#Disable-some-global-annotations)<br>

[**Recommended method annotations**](#Recommended-method-annotations)<br>

[**Debug annotations**](#Debug-annotations)<br>

# Configuration
[Configuration with JUnit 4 and Spring](https://github.com/quick-perf/doc/wiki/QuickPerfSpringRunner-&-SQL-annotations)

# Worflow
Below, we try to describe a way to efficiently use SQL annotation during development.

# Recommended global annotations
We recommend to apply the following SQL annotations by default, that is to say for each test.

## @DisableExactlySameSqlSelects

## @DisableSameSelectTypesWithDifferentParams

## @DisableSqlCrossJoin
 The [cartesian product induced by a cross join can be very inefficient](https://vladmihalcea.com/hibernate-facts-always-check-criteria-api-sql-queries/). Although most database engines will try to remove a cross join, we can decide to remove cross join to not have to check if a database engine version will really remove it.
 
x## @DisableLikeStartingWithWildcard

## @DisableSelectDistinct

## @JdbcBatches

### Parameters 
|Parameter  |Type| Meaning           | Default value  |
| -------- |:---:|:-----------------:|:--------------:|
| batchSize| int |JDBC batch size    |      -         |

A 0 batch size means that JDBC batching is disabled.

### Example
```java
    @JdbcBatches(batchSize = 30)
```

## Configure global annotations
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
                             , jdbcBatches(batchSize)
                             , disableSqlCrossJoin()
                             , disableLikeStartingWithWildcard()
                             , disableSelectDistinct()
                             );
    }

}
```
**The class implementing SpecifiableAnnotations has to be in org.quickperf package.**

# Disable some global annotations
## @EnableSqlCrossJoin
To decide to enable a cross join in a specific case if you add @DisableSqlCrossJoin check for every test or at test class level.

## @EnableExactlySameSqlSelects


## @EnableSameSelectTypesWithDifferentParams


## @EnableSelectDistinct

## @EnableLikeStartingWithWildcard

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


