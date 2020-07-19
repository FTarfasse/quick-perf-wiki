# 🚩 Table of contents

[How to promote performance and scalability?](#how-to-promote-performance-and-scalability)<br>

[Quickstart](#Quickstart)<br>

[Available SQL annotations](#Available-SQL-annotations)<br>

[Configure global annotations](#Configure-global-annotations)<br>

[Cancel the behavior of global annotations at method level](#Cancel-the-behavior-of-global-annotations-at-method-level)<br>

[Apply SQL annotations on methods](#apply-sql-annotations-on-test-methods)<br>

[Use TDD to implement persistence performance properties](https://github.com/quick-perf/doc/wiki/Use-TDD-to-implement-persistence-performance-properties)

# How to promote performance and scalability?
To promote performance and scalability at the beginning of application development, we can
* **Limit JDBC roundtrips**
  * [***Detect N+1 selects***](https://github.com/quick-perf/doc/wiki/Easily-detect-and-fix-N-plus-One-SELECT-with-QuickPerf) by using [@ExpectSelect](./@ExpectSelect), [@ExpectMaxSelect](./@ExpectMaxSelect) or [@DisableSameSelectTypesWithDifferentParamValues](./@DisableSameSelectTypesWithDifferentParamValues)<br> 
  * ***Detect JDBC batching disabled*** by using [@ExpectJdbcBatching](./@ExpectJdbcBatching)
  * ***Detect exactly same selects*** by using [@DisableExactlySameSelects](./@DisableExactlySameSelects)

     ***[Why limit JDBC roundtrips?](https://blog.jooq.org/2017/12/18/the-cost-of-jdbc-server-roundtrips/)***

* **Limit fetched data**
  * ***Detect too many selected*** columns by using [@ExpectSelectedColumn](./@ExpectSelectedColumn) or [@ExpectMaxSelectedColumn](./@ExpectMaxSelectedColumn)<br><br>
***[Why limit the number of selected columns?](https://github.com/quick-perf/doc/wiki/Why-limit-the-number-of-selected-columns)***
* **Avoid SQL statements without bind parameters**, detect them by using [@DisableQueriesWithoutBindParameters](./@DisableQueriesWithoutBindParameters)
* **Avoid SQL statements having a LIKE pattern starting with a wildcard**, identify them by using [@DisableLikeWithLeadingWildcard](./@DisableLikeWithLeadingWildcard)

* ...

⚠️ *Do little configuration described in [**Quickstart**](#Quickstart) before using SQL annotations.*

# Quickstart
## Add configuration 
### [Configuration for Spring (JUnit 4, JUnit 5, TestNG)](https://github.com/quick-perf/doc/wiki/Spring)
### [Configuration for JUnit 4](https://github.com/quick-perf/doc/wiki/JUnit-4)

## Check the configuration
To check that the configuration is properly done, you can try to add an annotation on a test method in order to make it fail. For example, add @ExpectSelect(0) on a test method that is supposed to send one or several selects to the database.

## Use SQL annotations
You can use SQL annotations with a [global scope](#Recommended-global-annotations), a class scope or a [method scope](#Recommended-method-annotations).

## Automatic framework detection
The SQL annotations automatically detect the presence of *Hibernate* and *Spring* frameworks. These annotations can propose solutions to get the expected behavior with these frameworks.

For example, QuickPerf displays the following message when an N+1 select is presumed, and Spring Data JPA is detected:
```
	* With Spring Data JPA, you may fix it by adding
	@EntityGraph(attributePaths = { "..." }) on repository method.
	https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.entity-graph
```

# Available SQL annotations

## All the SQL annotations
<table>
    <tbody>
       <tr>
            <td> <a href="./@ExpectSelect">@ExpectSelect</a> </td>
            <td> <a href="./@ExpectMaxSelect"> @ExpectMaxSelect</a> </td>
       </tr>      
        <tr>
            <td> <a href="./@ExpectSelectedColumn">@ExpectSelectedColumn</a> </td>
            <td> <a href="./@ExpectMaxSelectedColumn"> @ExpectMaxSelectedColumn</a> </td>
       </tr>     
       <tr>
            <td> <a href="./@ExpectUpdate">@ExpectUpdate</a> </td>
            <td> <a href="./@ExpectMaxUpdate">@ExpectMaxUpdate</a> </td>
       </tr>    
       <tr>
            <td> <a href="./@ExpectUpdatedColumn">@ExpectUpdatedColumn</a> </td>
            <td> <a href="./@ExpectMaxUpdatedColumn"> @ExpectMaxUpdatedColumn</a> </td>
       </tr>    
       <tr>
            <td> <a href="./@ExpectInsert">@ExpectInsert</a> </td>
            <td> <a href="./@ExpectMaxInsert"> @ExpectMaxInsert</a> </td>
       </tr>   
       <tr>
           <td> <a href="./@ExpectDelete">@ExpectDelete</a> </td>
           <td></td>
       </tr>   
       <tr>
            <td> <a href="./@DisplaySql">@DisplaySql</a> </td>
            <td> <a href="./@DisplaySqlOfTestMethodBody">@DisplaySqlOfTestMethodBody</a> </td>
       </tr>       
       <tr>
            <td> <a href="./@ExpectJdbcBatching">@ExpectJdbcBatching</a> </td>
            <td> <a href="./@ExpectMaxQueryExecutionTime">@ExpectMaxQueryExecutionTime</a> </td>
       </tr>      
        <tr>
            <td> <a href="./@DisableExactlySameSelects">@DisableExactlySameSelects</a> </td>
            <td> <a href="./@EnableExactlySameSelects">@EnableExactlySameSelects</a> </td>
       </tr>        
       <tr>
            <td> <a href="./@DisableSameSelectTypesWithDifferentParamValues">@DisableSameSelectTypesWithDifferentParamValues</a> </td>
            <td> <a href="./@EnableSameSelectTypesWithDifferentParamValues">@EnableSameSelectTypesWithDifferentParamValues</a> </td>
       </tr>       
       <tr>
            <td> <a href="./@DisableLikeWithLeadingWildcard">@DisableLikeWithLeadingWildcard</a> </td>
            <td> <a href="./@EnableLikeWithLeadingWildcard">@EnableLikeWithLeadingWildcard</a> </td>
       </tr>       
       <tr>
            <td> <a href="./@DisableQueriesWithoutBindParameters">@DisableQueriesWithoutBindParameters</a> </td>
            <td> <a href="./@EnableQueriesWithoutBindParameters">@EnableQueriesWithoutBindParameters</a> </td>
       </tr>
    </tbody>
</table>

## SELECT statements

|Annotation                                                                                |Short description                                              |
| -----------------------------------------------------------------------------------------|---------------------------------------------------------------|
|[@ExpectSelect](./@ExpectSelect)                                                          | SELECT number                                                 |
|[@ExpectMaxSelect](./@ExpectMaxSelect)                                                    | Max SELECT number                                             |
|[@ExpectSelectedColumn](./@ExpectSelectedColumn)                                          | Selected columns number                                       |
|[@ExpectMaxSelectedColumn](./@ExpectMaxSelectedColumn)                                    | Max selected columns number                                   |
|[@DisableExactlySameSelects](./@DisableExactlySameSelects)                                  | Disable exactly same SELECT statements                        |
|[@EnableExactlySameSelects](./@EnableExactlySameSelects)                                    | Enable exactly same SELECT statements                         |
|[@DisableSameSelectTypesWithDifferentParamValues](./@DisableSameSelectTypesWithDifferentParamValues)| Disable same SELECT statements with different parameter values|
|[@EnableSameSelectTypesWithDifferentParamValues](./@EnableSameSelectTypesWithDifferentParamValues)                                  | Enable same SELECT statements with different parameter values |

## INSERT statements

|Annotation                      |Short description       |
| -------------------------------|------------------------|
|[@ExpectInsert](./@ExpectInsert)| INSERT number          |
|[@ExpectMaxInsert](./@ExpectMaxInsert)| Max INSERT number|

## DELETE statements

|Annotation                      |Short description|
| -------------------------------|-----------------|
|[@ExpectDelete](./@ExpectDelete)| DELETE number   |

## UPDATE statements

|Annotation                                          |Short description          | 
| ---------------------------------------------------|---------------------------|
|[@ExpectUpdate](./@ExpectUpdate)                    | UPDATE number             |
|[@ExpectMaxUpdate](./@ExpectMaxUpdate)              | Max UPDATE number         |
|[@ExpectMaxUpdatedColumn](./@ExpectMaxUpdatedColumn)| Updated columns number    |
|[@ExpectUpdatedColumn](./@ExpectUpdatedColumn)      | Max updated columns number|

## Debug

|Annotation                                                  |Short description                        |
| -----------------------------------------------------------|-----------------------------------------|
|[@DisplaySql](./@DisplaySql)                                | Display SQL                             |
|[@DisplaySqlOfTestMethodBody](./@DisplaySqlOfTestMethodBody)| Display SQL executed in test method body|

You can also use [@DisplayAppliedAnnotations](https://github.com/quick-perf/doc/wiki/Core-annotations#DisplayAppliedAnnotations) in debug activity.

## Other

|Annotation                                                          |Short description                  |
| -------------------------------------------------------------------|-----------------------------------|
|[@ExpectJdbcBatching](./@ExpectJdbcBatching)                        | JDBC batching is enabled          |
|[@ExpectMaxQueryExecutionTime](./@ExpectMaxQueryExecutionTime)      | Max query execution time          |
|[@DisableLikeWithLeadingWildcard](./@DisableLikeWithLeadingWildcard)| Disable like with leading wildcard|
|[@EnableLikeWithLeadingWildcard](./@EnableLikeWithLeadingWildcard)  | Enable like with leading wildcard |
|[@DisableQueriesWithoutBindParameters](./@DisableQueriesWithoutBindParameters)  | Disable queries without bind variables |
|[@EnableQueriesWithoutBindParameters](./@EnableQueriesWithoutBindParameters)  | Enable queries without bind variables |

# Configure global annotations

_Global annotations_ apply on each test.

Let's suppose that you just add QuickPerf to an application having automatic tests. With global annotations, you can quickly apply some performance checks on the existing tests in order to detect some classical performance bottlenecks.

To apply the global annotations, the test classes have to be annotated with @QuickPerfJUnitRunner or @QuickPerfSpringRunner with JUnit 4 and @QuickPerfTest with JUnit 5. With TestNG, you don't have to add a QuickPerf annotation on the test class.
Global annotations can be configured by creating a class implementing _SpecifiableGlobalAnnotations_. ***This class has to be in _org.quickperf_ package***.
A _SqlAnnotationBuilder_ class is available to easily configure SQL global annotations.

```java
package org.quickperf;
import org.quickperf.config.user.SpecifiableGlobalAnnotations;
import org.quickperf.sql.annotation.SqlAnnotationBuilder;
import java.lang.annotation.Annotation;
import java.util.Arrays;
import java.util.Collection;
import static org.quickperf.sql.annotation.SqlAnnotationBuilder.*;

public class QuickPerfConfiguration implements SpecifiableGlobalAnnotations {
    public Collection<Annotation> specifyAnnotationsAppliedOnEachTest() {

        return Arrays.asList(
                // Can reveal some N+1 selects
                // https://blog.jooq.org/2017/12/18/the-cost-of-jdbc-server-roundtrips/
                disableSameSelectTypesWithDifferentParams()

                , // Sometimes, JDBC batching can be disabled:
                // https://abramsm.wordpress.com/2008/04/23/hibernate-batch-processing-why-you-may-not-be-using-it-even-if-you-think-you-are/
                // https://stackoverflow.com/questions/27697810/hibernate-disabled-insert-batching-when-using-an-identity-identifier
                expectJdbcBatching()

                , // https://use-the-index-luke.com/sql/where-clause/searching-for-ranges/like-performance-tuning
                disableLikeWithLeadingWildcard()

                , disableExactlySameSelects()

                // Not relevant with an in-memory database used for testing purpose
                , expectMaxQueryExecutionTime( 30, TimeUnit.MILLISECONDS)

        );

    }
}
```

We recommend to configure the following SQL global annotations:

|Annotation                                                                                |Short description                                              |
| -----------------------------------------------------------------------------------------|---------------------------------------------------------------|
|[@DisableExactlySameSelects](./@DisableExactlySameSelects)                                  | Disable exactly same SELECT statements                        |
|[@DisableSameSelectTypesWithDifferentParamValues](./@DisableSameSelectTypesWithDifferentParamValues)| Disable same SELECT statements with different parameter values|
|[@DisableLikeWithLeadingWildcard](./@DisableLikeWithLeadingWildcard)                      | Disable like with leading wildcard                            |
|[@ExpectJdbcBatching](./@ExpectJdbcBatching)                                              | JDBC batching is enabled                                      |
|[@ExpectMaxQueryExecutionTime](./@ExpectMaxQueryExecutionTime)                            | Max query execution time                                      |

# Cancel the behavior of global annotations at method level

In some specific cases, you may want to disable dôme global annotations.

You can use the following annotations to disable the [recommended global annotations](#Configure-global-annotations) for some test methods: 

|Annotation                                                                              |Short description             |
| ---------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
|[@EnableExactlySameSelects](./@EnableExactlySameSelects)                                |Cancels behavior of [@DisableExactlySameSelects](./@DisableExactlySameSelects)                                |
|[@EnableSameSelectTypesWithDifferentParamValues](./@EnableSameSelectTypesWithDifferentParamValues)|Cancels behavior of [@DisableSameSelectTypesWithDifferentParamValues](./@DisableSameSelectTypesWithDifferentParamValues)|
|[@EnableLikeWithLeadingWildcard](./@EnableLikeWithLeadingWildcard)                      |Cancels behavior of [@DisableLikeWithLeadingWildcard](./@DisableLikeWithLeadingWildcard)                      |
|[@ExpectJdbcBatching(batchSize=0)](./@ExpectJdbcBatching)                               |Cancels behavior of [@ExpectJdbcBatching](./@ExpectJdbcBatching)                                              |

In the case where you are developing a new feature, perhaps with the help of *Test-Driven Development* (TDD), your test may fail because the business property is unrespected but also because some performance properties checked by global annotations are unrespected. In order to do one step at a time, you can _temporarily_ disable global annotations by applying [@FunctionalIteration](https://github.com/quick-perf/doc/wiki/core-annotations#disablequickperf) or [@DisableQuickPerf](https://github.com/quick-perf/doc/wiki/core-annotations#disablequickperf) or [@DisableGlobalAnnotations](https://github.com/quick-perf/doc/wiki/core-annotations#disableglobalannotations) at method level.

# Apply SQL annotations on test methods

In addition to the performance properties verified by the global annotations, others can be checked for some test methods.

The annotations added on the test methods can help to document the code. For example, by reading ```@ExpectSelect(1)``` annotation applied on a test method, we know that we expect exactly one select sent to the database.

Among all the SQL annotations, we recommend using the following on test methods: 

|Annotation                                             |Short description            |
| ------------------------------------------------------|-----------------------------|
|[@ExpectSelect](./@ExpectSelect)                       | SELECT number               |
|[@ExpectMaxSelect](./@ExpectMaxSelect)                 | Max SELECT number           |
|[@ExpectSelectedColumn](./@ExpectSelectedColumn)       | Selected columns number     |
|[@ExpectMaxSelectedColumn](./@ExpectMaxSelectedColumn) | Max selected columns number |
|[@ExpectInsert](./@ExpectInsert)                       | INSERT number               |
|[@ExpectUpdate](./@ExpectUpdate)                       | UPDATE number               |
|[@ExpectMaxUpdatedColumn](./@ExpectMaxUpdatedColumn)   | Max updated columns         |
|[@ExpectDelete](./@ExpectDelete)                       | DELETE number               |