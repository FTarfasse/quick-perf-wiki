# 🚩 Table of contents

[Quick start](#Quick-start)<br>

[Interesting checks](#Interesting-checks)<br>

[Available SQL annotations](#Available-SQL-annotations)<br>

[Recommended global annotations](#Recommended-global-annotations)<br>

[Cancel the behavior of global annotations](#Cancel-the-behavior-of-global-annotations)<br>

[Recommended method annotations](#Recommended-method-annotations)<br>

# Quick start
## Add configuration 
### [Configuration for Spring](https://github.com/quick-perf/doc/wiki/Spring)
### [Configuration for JUnit 4](https://github.com/quick-perf/doc/wiki/JUnit-4)

## Check the configuration
To check that the configuration is properly done, you can try to add an annotation on a test method in order to make it fail. For example, add @ExpectSelect(0) on a test method that is supposed to send one or several selects to the database.

## Use SQL annotations
You can use SQL annotations with a [global scope](#Recommended-global-annotations), a class scope or a [method scope](#Recommended-method-annotations).

## Automatic framework detection
The SQL annotations automatically detect if *Hibernate* or *Spring* frameworks are used. You don't have any configuration to do. If a SQL property is not respected, the SQL annotations can suggest you solutions to fix it with these frameworks.

For example, the following message is diplayed when a N+1 select is presumed and Spring Data JPA is detected:
```
	* With Spring Data JPA, you may fix it by adding
	@EntityGraph(attributePaths = { "..." }) on repository method.
	https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.entity-graph
```
# What to take care of?
You can take care of several things about SQL statements to promote performance and scalability at the beginning of application development.
* JDBC roundtrips
  * Detect N+1 selects by using [@ExpectSelect](#ExpectSelect), [@ExpectMaxSelect](#ExpectMaxSelect) or [@DisableSameSelectTypesWithDifferentParams](#DisableSameSelectTypesWithDifferentParams)<br> 
  * Detect JDBC batching disabled by using [@ExpectJdbcBatching](#ExpectJdbcBatching)
  * Detect exactly same selects by using [@DisableExactlySameSelects](#DisableExactlySameSelects)

  *[Why limit JDBC roundtrips?](https://blog.jooq.org/2017/12/18/the-cost-of-jdbc-server-roundtrips/)*

* Fetched data
  * Detect too many selected columns by using [@ExpectSelectedColumn](#ExpectSelectedColumn) or [@ExpectMaxSelectedColumn](#ExpectMaxSelectedColumn)<br><br>
*[Why limit the number of selected columns?](https://github.com/quick-perf/doc/wiki/Why-limit-the-number-of-selected-columns)*
* SQL statements having a LIKE pattern starting with a wildcard by using [@DisableLikeWithLeadingWildcard](#DisableLikeWithLeadingWildcard)

* ...

⚠️ *Do little configuration described in [**Quick start**](#Quick-start) before using SQL annotations.*


# Available SQL annotations

## SELECT statements

|Annotation                                                                                |Short description                                              |
| -----------------------------------------------------------------------------------------|---------------------------------------------------------------|
|[@ExpectSelect](./@ExpectSelect)                                                          | SELECT number                                                 |
|[@ExpectMaxSelect](./@ExpectMaxSelect)                                                    | Max SELECT number                                             |
|[@ExpectSelectedColumn](./@ExpectSelectedColumn)                                          | Selected columns number                                       |
|[@ExpectMaxSelectedColumn](./@ExpectMaxSelectedColumn)                                    | Max selected columns number                                   |
|[@DisableExactlySameSelect](./@DisableExactlySameSelect)                                  | Disable exactly same SELECT statements                        |
|[@EnableExactlySameSelect](./@EnableExactlySameSelect)                                    | Enable exactly same SELECT statements                         |
|[@DisableSameSelectTypesWithDifferentParams](./@DisableSameSelectTypesWithDifferentParams)| Disable same SELECT statements with different parameter values|
|[@EnableExactlySameSelects](./@EnableExactlySameSelects)                                  | Enable same SELECT statements with different parameter values |

## INSERT statements

|Annotation                      |Short description|
| -------------------------------|-----------------|
|[@ExpectInsert](./@ExpectInsert)| INSERT number   |

## DELETE statements

|Annotation                      |Short description|
| -------------------------------|-----------------|
|[@ExpectDelete](./@ExpectDelete)| DELETE number   |

## UPDATE statements

|Annotation                                          |Short description   |
| ---------------------------------------------------|--------------------|
|[@ExpectUpdate](./@ExpectUpdate)                    | UPDATE number      |
|[@ExpectMaxUpdatedColumn](./@ExpectMaxUpdatedColumn)| Max updated columns|

## Debug annotations

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

# Recommended global annotations

## Configure recommended global annotations
A SqlAnnotationBuilder class is available to easily implement SpecifiableGlobalAnnotations.

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
***The class implementing SpecifiableGlobalAnnotations has to be in org.quickperf package.***

|Annotation                                                                                |Short description                                              |
| -----------------------------------------------------------------------------------------|---------------------------------------------------------------|
|[@DisableExactlySameSelect](./@DisableExactlySameSelect)                                  | Disable exactly same SELECT statements                        |
|[@DisableSameSelectTypesWithDifferentParams](./@DisableSameSelectTypesWithDifferentParams)| Disable same SELECT statements with different parameter values|
|[@DisableLikeWithLeadingWildcard](./@DisableLikeWithLeadingWildcard)                      | Disable like with leading wildcard                            |
|[@ExpectJdbcBatching](./@ExpectJdbcBatching)                                              | JDBC batching is enabled                                      |
|[@ExpectMaxQueryExecutionTime](./@ExpectMaxQueryExecutionTime)                            | Max query execution time                                      |

# Cancel the behavior of global annotations at method level

|Annotation                                                                              |Short description             |
| ---------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
|[@EnableExactlySameSelects](./@EnableExactlySameSelects)                                |Cancel behavior of [@DisableExactlySameSelects](./@DisableExactlySameSelects)                                |
|[@EnableSameSelectTypesWithDifferentParams](./@EnableSameSelectTypesWithDifferentParams)|Cancel behavior of [@DisableSameSelectTypesWithDifferentParams](./@DisableSameSelectTypesWithDifferentParams)|
|[@EnableLikeWithLeadingWildcard](./@EnableLikeWithLeadingWildcard)                      |Cancel behavior of [@DisableLikeWithLeadingWildcard](./@DisableLikeWithLeadingWildcard)                      |
|[@ExpectJdbcBatching(batchSize=0)](./@ExpectJdbcBatching)                               |Cancel behavior of [@ExpectJdbcBatching](./@ExpectJdbcBatching)                                              |

# Recommended method annotations

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