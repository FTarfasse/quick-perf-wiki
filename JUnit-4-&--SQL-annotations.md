To use [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations) in JUnit 4 tests, you need first of all add *quick-perf-junit4* and *quick-perf-sql* dependencies:<br>
...

You also need to:
* annotate your test class with *@RunWith(QuickPerfJUnitRunner.class)*
* build a datasource proxy with *QuickPerfSqlDataSourceBuilder*
* use this proxy in your test. 

Look at the example below.

### Example
```java
	import org.junit.runner.RunWith;
	import quickperf.junit4.QuickPerfJUnitRunner;

        import net.ttddyy.dsproxy.support.ProxyDataSource;
        import org.hibernate.jpa.HibernatePersistenceProvider;
        import org.junit.Before;
        import org.quickperf.sql.entities.Book;

        import javax.persistence.EntityManagerFactory;
        import javax.persistence.spi.PersistenceProvider;
        import javax.persistence.spi.PersistenceUnitInfo;
        import javax.sql.DataSource;
        import java.util.HashMap;
        import java.util.Properties;

        import org.quickperf.sql.QuickPerfSqlDataSourceBuilder;

        import org.quickperf.sql.annotation.SelectNumber;

	@RunWith(QuickPerfJUnitRunner.class)
	public class bookTest {

          private EntityManagerFactory emf;

          @Before
          public void before() {
             PersistenceProvider persistenceProvider = new HibernatePersistenceProvider();
             PersistenceUnitInfo info = buildPersistenceUnitInfo();
             emf = persistenceProvider.createContainerEntityManagerFactory(info, new HashMap<>());
          }

         private PersistenceUnitInfo buildPersistenceUnitInfo() {
             DataSource baseDataSource = TestDataSourceBuilder.aDataSource().build();
             ProxyDataSource proxyDataSource = QuickPerfSqlDataSourceBuilder.aDataSourceBuilder().buildProxy(baseDataSource);
             Properties hibernateProperties = HibernateConfigBuilder.anHibernateConfig().build();
             return PersistenceUnitInfoBuilder.aPersistenceUnitInfo()
                                              .build(proxyDataSource
                                                   , hibernateProperties
                                                   , Book.class);
         }

         @ExpectMaxSelectNumber(1)
         @Test
         public void should_retrieve_books_from_database() {
         // ... 
        }

}
```

Add quick-perf-junit4 and 