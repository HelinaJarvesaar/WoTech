## TASK:

1. We need to choose a city randomly.
2. The chance for the city should be proportional for the citizen count.

   99 citizens -> Goog 99%
  
    1 citizen -> Wocity 1%
  
4. Cities are inside database
5. We should have an API where we can call getRandomCity()
6. Winners shouldn't be excluded from next years lottery

City.java

```java
package com.example.demo.CityLottery;

public class City {
    private final String name;
    private final int population;

    public City(String name, int population){
        this.name = name;
        this.population = population;
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
ackage com.example.demo.CityLottery;

import java.util.ArrayList;
import java.util.Random;

public class CityService {

    private ICityRepository cityRepository;
    public CityService(ICityRepository cityRepository) {
        this.cityRepository = cityRepository;
    }
    public City getRandomCity() throws Exception {
        // 0. Prepare a list of cities
        ArrayList<City> cities = new ArrayList<>();

        var goog = new City("Goog", 75);
        var wocity = new City("Wocity", 25);
        var oskarscity = new City("Oskars city", 25);
        cities.add(goog);
        cities.add(wocity);
        cities.add(oskarscity);

        //1. Count total amount of citizens -> 100
        var totalCitizenCount = 0;

        for (City city: cities){
            totalCitizenCount += city.getPopulation();
        }

        //2. Choose random number -> 56
        Random random = new Random();
        int randomValue = random.nextInt(totalCitizenCount);

        //3. Loop going through all of the cities
        //4. Choose the city with correct lottery ticket
        //population -> 25
        //randomValue -> 56
        //We subtract 56 - 25 = 31
        // BECAUSE ITS NOT BELOW OR EQUAL TO 0, GO TO NEXT
        // 31 - 75 -> because it's below 0, we choose this city

       //GETCITY
        for(City city: cities){
            randomValue -= city.getPopulation();

            if(randomValue <= 0){
                return city;
            }
        }
        throw new Exception("Something wrong");
    }
}
```
CityRepository.java
```java
package com.example.demo.CityLottery;

import java.util.ArrayList;

public class CityRepository implements ICityRepository {
    //mock db
    public ArrayList<City> getCities(){ // this is declaration
        ArrayList<City> cities = new ArrayList<>();
        var goog = new City("Goog", 75);
        var wocity = new City("Wocity", 25);
        var oskarscity = new City("Oskars city", 25);
        cities.add(goog);
        cities.add(wocity);
        cities.add(oskarscity);
        return cities;
    }
}
```

ICityRepository.java
```java
package com.example.demo.CityLottery;

import java.util.ArrayList;

public interface ICityRepository {
    ArrayList<City> getCities(); // this is declaration

}
```

FinalProject1Application.java
```java
package com.example.demo;
import com.example.demo.CityLottery.CityRepository;
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

Unit Tests:

CityServiceTests.java
```java
package com.example.demo;

import com.example.demo.CityLottery.CityRepository;
import com.example.demo.CityLottery.CityService;
import com.example.demo.CityLottery.ICityRepository;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;

public class CityServiceTests {

    // when there is Goog and Wocity
    // and Goog has 10% (10citizens)
    // and Wocity has 90% (90 citizen)
    // randomizer choose 9
    // choose Goog

    @Test
    public void GIVEN_Goog10_Wocity90_WHEN_Randomizer9_THEN_ChooseGoog(){
        // Arrange
        var cityRepository = Mockito.mock(ICityRepository.class);
        var cityService = new CityService(cityRepository);
        // Act

        // Asset
    }
}
```

FinalProject1ApplicationTests.java
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

<img width="498" alt="Screenshot 2024-09-28 at 18 36 38" src="https://github.com/user-attachments/assets/33d4da0d-c678-42ef-b36a-a211e84f99a3">



## INDIVIDUAL HOMEWORK

1. Log in leetcode - OR ANY OTHER developer site where you can get problems to solve
   
	https://leetcode.com/problemset/

2. Choose a random problem from leetcode algorithms problem, which has difficulty Medium, but if you can't go Easy
