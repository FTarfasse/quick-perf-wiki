# 🚩 Table of contents

[What is an N+1 select?](#what-is-an-n1-select)<br>

[Why N+1 selects can lead to a performance problem?](#why-n1-selects-can-lead-to-a-performance-problem)

[Easily detect N+1 selects with QuickPerf](#easily-detect-n1-selects-with-quickperf)<br>

[Easily fix N+1 selects with QuickPerf](#easily-fix-n1-selects-with-quickperf)

# What is an N+1 select?

For the following examples, this SQL script was previously executed: 
```sql
    INSERT INTO TEAM VALUES (1, 'Manchester United');
    INSERT INTO TEAM VALUES (2, 'Atlético de Madrid');

    INSERT INTO PLAYER VALUES (1, 'Paul', 'Pogba', 1);
    INSERT INTO PLAYER VALUES (2, 'Antoine', 'Griezmann', 2);
```

## N+1 select coming from an eager fetch type

Let's suppose that our project contains a Player JPA entity having a many to one association with a Team entity:
```java
    @ManyToOne(targetEntity = Team.class)
    @JoinColumn(name = "team_id")
    private Team team;
```
The fetch type is not specified. In JPA, the default fetching policy of @ManyToOne is EAGER.

And let's suppose that our application is executing the following "FROM Player" Java Persistence query:
```java
     TypedQuery<Player> fromPlayer = entityManager.createQuery("FROM Player", Player.class);
     List<Player> players = fromPlayer.getResultList();
```


The following SQL statements are then sent to the database:
```sql
    select
        player0_.id as id1_0_,
        player0_.firstName as firstNam2_0_,
        player0_.lastName as lastName3_0_,
        player0_.team_id as team_id4_0_ 
    from
        Player player0_
```

```sql
    select
        team0_.id as id1_1_0_,
        team0_.name as name2_1_0_ 
    from
        Team team0_ 
    where team0_.id=?
    
    Params:[(1)]
```

```sql
    select
        team0_.id as id1_1_0_,
        team0_.name as name2_1_0_ 
    from
        Team team0_ 
    where
        team0_.id=?"
    
    Params:[(2)]
```

Because of the default EAGER fetch type, for each player a SELECT statement is executed to retrieve his team.

## N+1 select with a lazy fetch type...

Now, let's suppose that our project contains a Player JPA entity having a many to one association with a Team entity:
```java
    @ManyToOne(targetEntity = Team.class, fetch = FetchType.LAZY)
    @JoinColumn(name = "team_id")
    private Team team;
```

The fetch type is set to LAZY.

And let's suppose that our application is executing the following Java code:

```java
    List<Player> players = fromPlayer.getResultList();

    List<PlayerWithTeamName> playersWithTeamName = players
                                                  .stream()
                                                  .map(player -> new PlayerWithTeamName( player.getFirstName()
                                                                                       , player.getLastName()
                                                                                       , player.getTeam().getName()
                                                                                       )
                                                       )
                                                  .collect(Collectors.toList());
```

Each time the `getName()` method is called, a SELECT... FROM Team statement is sent to the database:

```sql
    select
        player0_.id as id1_0_,
        player0_.firstName as firstNam2_0_,
        player0_.lastName as lastName3_0_,
        player0_.team_id as team_id4_0_ 
    from
        Player player0_
```

```sql
    select
        team0_.id as id1_1_0_,
        team0_.name as name2_1_0_ 
    from
        Team team0_ 
    where
        team0_.id=?

    Params:[(1)]
```

```sql
    select
        team0_.id as id1_1_0_,
        team0_.name as name2_1_0_ 
    from
        Team team0_ 
    where
        team0_.id=?

   Params:[(2)]
```

# Why N+1 selects can lead to a performance problem?
*N+1* select antipattern can lead to many JDBC roundtrips in production and JDBC roundtrips are harmful for performance as explained in [this paper](https://blog.jooq.org/2017/12/18/the-cost-of-jdbc-server-roundtrips/).

Hereafter, we show that, with the help of QuickPerf, we can quickly detect and fix the the N+1 select antipattern from tests.

# Easily detect N+1 selects with QuickPerf

## Add @ExpectSelect on test method

We can detect N+1 select by adding ***[@ExpectSelect](./@ExpectSelect) annotation on a test method*** to check the number of executed SELECT statements.

## Configure @DisableSameSelectTypesWithDifferentParams annotation with a global scope

The outcome of an N+1 select is to have the same SELECT statements with different values. We can systematically detect this by configuring ***@DisableSameSelectTypesWithDifferentParams annotation with a [global scope](https://github.com/quick-perf/doc/wiki/QuickPerf#annotation-scopes)***. In the previous examples, the outcome of the N+1 select was to have additional SELECT statements on Team table. These additional SELECT statements are the same apart from the id value of the Team table.

## Code examples

Hibernate code examples are available to play with these two ways of detecting N+1 selects: <br>
:point_right: &nbsp; [Hibernate JUnit 4 code example](https://github.com/quick-perf/quickperf-examples/blob/master/hibernate-junit4/src/test/java/org/quickperf/sql/HibernateJUnit4Test.java) <br>
:point_right: &nbsp; [Hibernate JUnit 5 code example](https://github.com/quick-perf/quickperf-examples/blob/master/hibernate-junit5/src/test/java/org/quickperf/sql/HibernateJUnit5Test.java) <br>
:point_right: &nbsp; [Hibernate TestNG code example](https://github.com/quick-perf/quickperf-examples/blob/master/hibernate-testng/src/test/java/org/quickperf/sql/HibernateTestNGTest.java) <br>

The use of QuickPerf to detect N+1 selects in a Spring Boot, a Quarkus or a Micronaut application is demonstrated in [this repository](https://github.com/quick-perf/quickperf-examples).

# Easily fix N+1 selects with QuickPerf

QuickPerf automatically detects that the tested code uses _Hibernate_ or _Spring Data JPA_. When a test is failing possibly due to an N+1 select, QuickPerf proposes ways to fix an N+1 select with _Hibernate_ or _Spring Data JPA_:

```
Perhaps you are facing a N+1 select issue
	* With Hibernate, you may fix it by using JOIN FETCH
	                                       or LEFT JOIN FETCH
	                                       or FetchType.LAZY
	                                       or ...
```
```
	* With Spring Data JPA, you may fix it by adding
		@EntityGraph(attributePaths = { "..." }) on repository method.
```

As suggested by QuickPerf, the [first N+1 select example](#n1-select-coming-from-an-eager-fetch-type
) can be fixed can be fixed by specifying a LAZY fetch type:
```java
    @ManyToOne(targetEntity = Team.class, fetch = FetchType.LAZY)
    @JoinColumn(name = "team_id")
    private Team team;
```

With the [second N+1 select example], players and their team can be loaded with one SELECT statement by adding a JOIN FETCH or a LEFT JOIN FETCH in the JPA query:
```java
    List<Player> players = fromPlayer.getResultList();
    TypedQuery<Player> fromPlayer = entityManager.createQuery("FROM Player p LEFT JOIN FETCH p.team"
                                                            , Player.class);

```

With a LEFT JOIN FETCH, the following SQL query is going to be executed:

```sql
    select
        player0_.id as id1_0_0_,
        team1_.id as id1_1_1_,
        player0_.firstName as firstNam2_0_0_,
        player0_.lastName as lastName3_0_0_,
        player0_.team_id as team_id4_0_0_,
        team1_.name as name2_1_1_ 
    from
        Player player0_ 
    left outer join
        Team team1_ 
            on player0_.team_id=team1_.id
```

