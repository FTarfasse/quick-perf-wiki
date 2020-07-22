_We could add many things to this document! It's a **draft** version. However, we hope that it is interesting! Don't hesitate to give feedback to jean.bisutti@gmail.com_.

At first, we will implement functional behavior with the help of *Test-Driven Development* (TDD). In a second step, we will use TDD to implement some persistence performance properties.

<br>

**Write a failing test on business behavior**

Let's imagine that you want to retrieve some data stored in a database.

Let's write a failing test!

```java
    @Test
    public void should_find_all_players() {

        PlayerRepository playerRepository = new PlayerRepository(entityManager);

        List<Player> players = playerRepository.findAll();

        assertThat(players).hasSize(2);

    }
```

```
java.lang.AssertionError: 
Expected size:<2> but was:<0>
```

The test fails because the implementation returns an empty list:
```java
public class PlayerRepository {

    private final EntityManager entityManager;

    public PlayerRepository(EntityManager entityManager) {
        this.entityManager = entityManager;
    }

    public List<Player> findAll() {
        return Collections.emptyList();
    }

}
```

<br>

**Implement the business behavior**

Now let's write code to have a green test and to have a working business behavior:

```java
public class PlayerRepository {

    private final EntityManager entityManager;

    public PlayerRepository(EntityManager entityManager) {
        this.entityManager = entityManager;
    }

    public List<Player> findAll() {
        return entityManager.createQuery("FROM Player").getResultList();
    }

}
```

Now we have a green test! :)

<br>

**Refactor the code**

Let's clean up a little the code to have something a little more readable:

```java
public class PlayerRepository {

    private final EntityManager entityManager;

    public PlayerRepository(EntityManager entityManager) {
        this.entityManager = entityManager;
    }

    public List<Player> findAll() {
        Query loadPlayerQuery = entityManager.createQuery("FROM Player");
        return loadPlayerQuery.getResultList();
    }

}
```

The test is still green!

**Evaluate a resource usage property: number of JDBC roundtrips**

Let's now evaluate a performance property!
We expect to retrieve the data with one select statement, and so one JDBC roundtrip.

Let's check this with the help of an `ExpectSelect(1)` QuickPerf annotation:

```java
@QuickPerfTest
public class PlayerRepositoryTest {
    
    @ExpectSelect(1)
    @Test
    public void should_find_all_players() {

        PlayerRepository playerRepository = new PlayerRepository(entityManager);

        List<Player> players = playerRepository.findAll();

        assertThat(players).hasSize(2);

    }
```

Let's restart the test. Now the test is failing!
```
[PERF] You may think that <1> select statement was sent to the database
       But in fact <3>...

💣  You may have even more select statements with production data.
Be careful with the cost of JDBC server roundtrips: https://blog.jooq.org/2017/12/18/the-cost-of-jdbc-server-roundtrips/
```

The code should be modified to have only one select sent to the database and get a green test.

<br>

**Evaluate other performance related properties**

After, we could check other performance properties, as [the number of selected columns](https://github.com/quick-perf/doc/wiki/Why-limit-the-number-of-selected-columns): 

```java
@QuickPerfTest
public class PlayerRepositoryTest {

    @ExpectSelectedColumn(3)
    @ExpectSelect(1)
    @Test
    public void should_find_all_players() {

        PlayerRepository playerRepository = new PlayerRepository(entityManager);

        List<Player> players = playerRepository.findAll();

        assertThat(players).hasSize(2);

    }
```


After that, we could for example check the query execution time with a production-like database:

``` java
    @ExpectMaxQueryExecutionTime(value = 20, unit = TimeUnit.MILLISECONDS)
    @ExpectSelectedColumn(3)
    @ExpectSelect(1)
    @Test
    public void should_find_all_players() {

        PlayerRepository playerRepository = new PlayerRepository(entityManager);

        List<Player> players = playerRepository.findAll();

        assertThat(players).hasSize(2);

    }
 ```

_It is worth noting that the performance properties do not systematically fail after adding the annotation, unlike the functional properties._



_Performance properties are evaluated (and perhaps fixed) one after the other._

By evaluating and fixing some performance properties, we can promote performance and scalability at the beginning of application development. These performance properties are like quality attributes added to the feature. In the previous example, we could think that @ExpectSelect(1) and @ExpectMaxQueryExecutionTime are more priority performance quality attributes than @ExpectSelectedColumn. Indeed, @ExpectSelect(1) allows detecting N+1 selects that could lead to many JDBC roundtrips with production data. @ExpectMaxQueryExecutionTime is essential to avoid long queries. Later, we could decrease the maximum permitted query execution time. The number of selected columns can [make the query execution time greater](https://use-the-index-luke.com/sql/clustering/index-only-scan-covering-index) and increase the memory consumed and the IO. In some situations, these two last things could be considered at first with a low priority even though it is a waste of resources.  
    
If a functional property is broken during an iteration, you can temporarily disable the verification of the performance properties by adding _@FunctionalIteration_ on your test method. We try to do one thing at a time; that is to say, we fix the functional property, and after that, we check the performance properties.

We could temporarily disable the [global performance checks](https://github.com/quick-perf/doc/wiki/SQL-annotations#configure-global-annotations) with the addition of @DisableGlobalAnnotations on the test method. _We have to explain why in the frame of a TDD workflow... Stay tuned!_