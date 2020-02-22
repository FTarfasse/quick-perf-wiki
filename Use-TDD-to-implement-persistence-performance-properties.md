_We could add many things to this document! It's a first version. However, we hope that it is interesting!_

At first, we are going to implement the functional behavior with the help of *Test-Driven Development* (TDD). In second step, we are going to use TDD to implement some persistence performance properties.

Let's imagine that you want to retrieve some data stored in a relational database.

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

The test fails because the implemenation returns an empty list:
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

Now let's write code to have a green test and to have a working functional behavior:

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

Let's now evaluate a performance property!
We expect to retrieve the date with one select.

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

The code would be modified to have only one select sent to the database and to have a green test.

After that we could check other performance properties, as [the number of selected columns](https://github.com/quick-perf/doc/wiki/Why-limit-the-number-of-selected-columns): 

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

It is worth noting that unlike the functional properties, the performance properties do not systematically fail after adding the annotation.

One performance property is evaluated (and perhaps fix) one after the other.

If, during an iteration, a functional property is broken, you can temporarily disable the verification of the performance properties by adding _@FunctionalIteration_ on your test method. We try do one thing at a time, that is to say fix the performance property and after that check the performance properties.

The [global performance checks](https://github.com/quick-perf/doc/wiki/SQL-annotations#configure-global-annotations) can be temporarily disabled with the addition of @DisableGlobalAnnotations on the test method. _We have to explain why in the frame of a TDD workflow... Stay tuned!_

Don't hesitate to give feedback: [@jean_bisutti](https://twitter.com/jean_bisutti)