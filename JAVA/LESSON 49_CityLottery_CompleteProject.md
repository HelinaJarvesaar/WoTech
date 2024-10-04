# Complete CityLottery project:


<img width="321" alt="Screenshot 2024-10-05 at 01 49 04" src="https://github.com/user-attachments/assets/cfb953ef-2d52-48dc-b41f-70b8f9ac6855">

FinalProject1Application(Main).java
```java
import com.example.demo.CityLottery.CityService;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication

public class FinalProject1Application {

	public static void main(String[] args) throws Exception {
		var cityService = new CityService(new CityRepository());
		var randomCity = cityService.getRandomCity();
		System.out.println(randomCity.getName());

	}
}
```

City.java
```java
package com.example.demo.CityLottery;

import jakarta.persistence.Entity; // persistence could be called as repository
import jakarta.persistence.Id;

@Entity
public class City {

    @Id
    private String name;
    private int population;

    public City(String name, int population){
        this.name = name;
        this.population = population;
    }

    public City(){
    }

    public String getName(){
        return name;
    }

    public int getPopulation(){
        return population;
    }

}
```

CityService.java
```java
package com.example.demo.CityLottery;

import java.util.List;
import java.util.Random;

public class CityService {

    private final ICityRepository cityRepository;
    private int seed;

    public CityService(ICityRepository cityRepository, int seed) {
        this.cityRepository = cityRepository;
        this.seed = seed;
    }

    public CityService(ICityRepository cityRepository) {
        this.cityRepository = cityRepository;
    }
    /*
     * 0. Prepare a list of cities
     * 1. Count total amount of citizens -> 100
     * 2. Choose random number -> 56
     * 3. Loop going through all the cities
     * 4. Choose the city with correct lottery ticket
     * */

    public City getRandomCity() throws Exception {
        var cities = cityRepository.getCities();

        // Check if city list is empty
        if (cities.isEmpty()) {
            throw new Exception("No cities available.");
        }
        int totalCitizenCount = getCitizenCount(cities);

        // Ensure totalCitizenCount is greater than 0
        if (totalCitizenCount <= 0) {
            throw new Exception("Total population of all cities must be greater than 0.");
        }
        int randomValue = getRandomNumber(totalCitizenCount);
        return getCity(cities, randomValue);
    }

    // 1. Count total amount of citizens -> 100
    public int getCitizenCount(List<City> cities) {
        int totalCitizenCount = 0;
        for (City city : cities) {
            totalCitizenCount += city.getPopulation();
        }
        return totalCitizenCount;
    }

    // 2. Choose random number -> 56
    // If we have seed, then random = new Random(seed) else random = new Random()
    public int getRandomNumber(int totalCitizenCount) {
        Random random = (seed != 0)
                ? new Random(seed)
                : new Random();

        return random.nextInt(totalCitizenCount);// totalCitizenCount is guaranteed to be > 0
    }

    // 3. Loop going through all the cities
    // 4. Choose the city with correct lottery ticket
    // population -> 25
    // randomValue -> 56
    // We subtract 56 - 25 = 31
    // BECAUSE ITS NOT BELOW OR EQUAL TO 0, GO TO NEXT
    // 31 - 75 -> because it's below 0, we choose this city
    // GET CITY

    public City getCity(List<City> cities, int randomValue) throws Exception {
        if(cities.isEmpty()){
            throw new Exception("City list is empty.");  //YOU WOULD THROW AN EXCEPTION HERE
        }
        for (City city : cities) {
            randomValue -= city.getPopulation();
            if (randomValue <= 0) {
                return city;
            }
        }
        throw new Exception("Something went wrong.");
    }
}
```

CityRepository.java
```java
package com.example.demo.CityLottery;

import org.hibernate.cfg.Configuration;

import java.util.ArrayList;
import java.util.List;

public class CityRepository implements ICityRepository {
    //mock db
    @Override
    public List<City> getCities(){ // this is declaration
        // You can return mock data for testing
        List<City> cities = new ArrayList<>();
        var goog = new City("Goog", 75);
        var wocity = new City("Wocity", 25);
        var oskarscity = new City("Oskars city", 25);
        cities.add(goog);
        cities.add(wocity);
        cities.add(oskarscity);
        return cities;

       /* try (var factory = new Configuration()
                .configure("hibernate.cfg.xml")
                .addAnnotatedClass(City.class)
                .buildSessionFactory()) {
            // Everything above should be in a different class
            // And factory itself should be using dependency injection
            try (var session = factory.openSession()) {
                session.beginTransaction();
                var query = session.createQuery("from City", City.class);
                return query.getResultList();
            }
        }*/
    }
}
```

ICityRepository.java
```java
package com.example.demo.CityLottery;

import java.util.List;

public interface ICityRepository {
    List<City> getCities(); // this is declaration

}
```

CityServiceTests.java
```java
package com.example.demo;

import com.example.demo.CityLottery.City;
import com.example.demo.CityLottery.CityRepository;
import com.example.demo.CityLottery.CityService;
import com.example.demo.CityLottery.ICityRepository;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import org.springframework.util.Assert;

import java.util.ArrayList;

import static org.mockito.Mockito.when;

public class CityServiceTests {

    // when there is Goog and Wocity
    // and Goog has 10% (10citizens)
    // and Wocity has 90% (90 citizen)
    // randomizer choose 9
    // choose Goog

    @Test
    public void GIVEN_Goog83_Wocity17_WHEN_Randomizer82_THEN_ChooseGoog() throws Exception{
        // Arrange
        // 1. CityService need cityRepository in the constructor
        // 2. Make a face cityRepository
        // Instead of using db, we are using fake data
        var cityRepository = Mockito.mock(ICityRepository.class);

        // 3. create a city service, by providing fake repository
        // 4. add a seed for the city service, that gives us repeatable result
        var cityService = new CityService(cityRepository, 123);

        // 5. create list of fake cities
        var cities = new ArrayList<City>();
        // 6. add fake cities to our fake city list
        cities.add(new City("Goog", 83));
        cities.add(new City("Wocity", 17));

        // 7. set it up, that when we want to get cities,
        // we actually get these fake cities prepared in point 6

        // WHEN someone asks DB for cities
        // THEN return predetermined cities
        when(cityRepository.getCities()).thenReturn(cities);

        // Act
        var randomCity = cityService.getRandomCity();

        // Asset
        Assert.isTrue(randomCity.getName().equals("Goog"), "Goog is supposed to be chosen!");
    }
    //1. Write a MOCK test
    //  1.1. When there is only one city with 100 citizens, then choose this city
    //  1.2. When there is no seed provided, then choose whatever city
    //2. Look how to check for exceptions
    //  2.1. When there is negative citizen count in a city, then show exception
    //  2.2. When there is no cities provided, then show exception

    @Test
    public void WHEN_OneCity100_THEN_CooseCity() throws Exception{
        var cityRepository = Mockito.mock(ICityRepository.class);
        var cityService = new CityService(cityRepository);

        ArrayList<City> cities = new ArrayList<>();
        cities.add(new City("Onecity", 100));

        when(cityRepository.getCities()).thenReturn(cities);

        var randomCity = cityService.getRandomCity();

        Assert.isTrue(randomCity.getName().equals("Onecity"), "Onecity is supposed to be chosen.");

    }

    @Test
    void WHEN_NoSeedProvided_THEN_ChooseWhateverCity() throws Exception {
        ICityRepository cityRepository = Mockito.mock(ICityRepository.class);
        CityService cityService = new CityService(cityRepository);

        ArrayList<City> cities = new ArrayList<>();
        cities.add(new City("Tallinn", 150));
        cities.add(new City("Viljandi", 50));

        when(cityRepository.getCities()).thenReturn(cities);

        var randomCity = cityService.getRandomCity();

        boolean isValidCity = randomCity.getName().equals("Tallinn") || randomCity.getName().equals("Viljandi");
        Assert.isTrue(isValidCity, "A valid city should be chosen.");
    }

}
```


hibernate.cfg.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC "-//Hibernate/Hibernate Configuration DTD 3.0//EN" "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <!-- SQLite connection settings -->
        <property name="hibernate.connection.driver_class">org.sqlite.JDBC</property>
        <property name="hibernate.connection.url">jdbc:sqlite:database.db</property>
        <property name="hibernate.dialect">org.hibernate.community.dialect.SQLiteDialect</property>
        <property name="hibernate.connection.pool_size">1</property>

        <!-- Show SQL in console -->
        <property name="hibernate.show_sql">true</property>
        <property name="hibernate.format_sql">true</property>

        <!-- Automatically create or update the database schema -->
        <property name="hibernate.hbm2ddl.auto">update</property>

        <!-- Mapping class -->
        <mapping class="com.example.demo.CityLottery.City"/>
    </session-factory>
</hibernate-configuration>
```


FinalProject1ApplicationTests(Main).java
```java
package com.example.demo;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class FinalProject1ApplicationTests {

	@Test
	void contextLoads() {
	}

}
```

CityServiceTests.java
```java
package com.example.demo;

import com.example.demo.CityLottery.City;
import com.example.demo.CityLottery.CityRepository;
import com.example.demo.CityLottery.CityService;
import com.example.demo.CityLottery.ICityRepository;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import org.springframework.util.Assert;

import java.util.ArrayList;

import static org.mockito.Mockito.when;

public class CityServiceTests {

    // when there is Goog and Wocity
    // and Goog has 10% (10citizens)
    // and Wocity has 90% (90 citizen)
    // randomizer choose 9
    // choose Goog

    @Test
    public void GIVEN_Goog83_Wocity17_WHEN_Randomizer82_THEN_ChooseGoog() throws Exception{
        // Arrange
        // 1. CityService need cityRepository in the constructor
        // 2. Make a face cityRepository
        // Instead of using db, we are using fake data
        var cityRepository = Mockito.mock(ICityRepository.class);

        // 3. create a city service, by providing fake repository
        // 4. add a seed for the city service, that gives us repeatable result
        var cityService = new CityService(cityRepository, 123);

        // 5. create list of fake cities
        var cities = new ArrayList<City>();
        // 6. add fake cities to our fake city list
        cities.add(new City("Goog", 83));
        cities.add(new City("Wocity", 17));

        // 7. set it up, that when we want to get cities,
        // we actually get these fake cities prepared in point 6

        // WHEN someone asks DB for cities
        // THEN return predetermined cities
        when(cityRepository.getCities()).thenReturn(cities);

        // Act
        var randomCity = cityService.getRandomCity();

        // Asset
        Assert.isTrue(randomCity.getName().equals("Goog"), "Goog is supposed to be chosen!");
    }
    //1. Write a MOCK test
    //  1.1. When there is only one city with 100 citizens, then choose this city
    //  1.2. When there is no seed provided, then choose whatever city
    //2. Look how to check for exceptions
    //  2.1. When there is negative citizen count in a city, then show exception
    //  2.2. When there is no cities provided, then show exception

    @Test
    public void WHEN_OneCity100_THEN_CooseCity() throws Exception{
        var cityRepository = Mockito.mock(ICityRepository.class);
        var cityService = new CityService(cityRepository);

        ArrayList<City> cities = new ArrayList<>();
        cities.add(new City("Onecity", 100));

        when(cityRepository.getCities()).thenReturn(cities);

        var randomCity = cityService.getRandomCity();

        Assert.isTrue(randomCity.getName().equals("Onecity"), "Onecity is supposed to be chosen.");

    }

    @Test
    void WHEN_NoSeedProvided_THEN_ChooseWhateverCity() throws Exception {
        ICityRepository cityRepository = Mockito.mock(ICityRepository.class);
        CityService cityService = new CityService(cityRepository);

        ArrayList<City> cities = new ArrayList<>();
        cities.add(new City("Tallinn", 150));
        cities.add(new City("Viljandi", 50));

        when(cityRepository.getCities()).thenReturn(cities);

        var randomCity = cityService.getRandomCity();

        boolean isValidCity = randomCity.getName().equals("Tallinn") || randomCity.getName().equals("Viljandi");
        Assert.isTrue(isValidCity, "A valid city should be chosen.");
    }

}
```

## OUTCOME:

<img width="526" alt="Screenshot 2024-10-05 at 01 53 02" src="https://github.com/user-attachments/assets/e1a468ef-8bd6-4ba6-9d12-2635c17f7c92">

<img width="526" alt="Screenshot 2024-10-05 at 01 51 40" src="https://github.com/user-attachments/assets/0add65dd-6dbb-443e-91b6-da4c8e0e783c">

<img width="526" alt="Screenshot 2024-10-05 at 01 52 21" src="https://github.com/user-attachments/assets/2318baff-4c04-4f17-aa69-f046f1f94551">


