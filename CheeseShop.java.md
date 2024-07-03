```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
  private static Scanner scanner = new Scanner(System.in);
  private static CheeseShop cheeseShop = new CheeseShop();
  private static CheeseService cheeseService = new CheeseService();

  public static void main(String[] args) {
    while (true) {
      System.out.println("Press 1, to add an item: ");
      System.out.println("Press 2, to print all items: ");
      System.out.println("Press 3, to buy items: ");
      System.out.println("Press any key to quit: ");
      int action = scanner.nextInt();

      if (action == 1) {
        addCheese();
      } else if (action == 2) {
        printCheeses();
      } else if (action == 3) {
        buyCheeses();
      } else {
        break;
      }
    }
  }

  public static void buyCheeses() {
    System.out.println("Enter the cheese you want to buy: ");
    String cheeseToBuy = scanner.next();
    Cheese cheese = cheeseShop.findCheeseByName(cheeseToBuy, cheeseService.getCheeses());
    cheeseShop.addToCart(cheese);
    System.out.println(cheeseToBuy + " cheese has been added to the cart. The cart total cost is: "
        + cheeseShop.cartTotalCost() + " eur.");

  }

  public static void addCheese() {
    System.out.println("Enter the cheese name: ");
    String name = scanner.next();
    System.out.println("Enter the cheese cost: ");
    int cost = scanner.nextInt();
    Cheese cheese = new Cheese(name, cost);
    cheeseService.addCheese(cheese);
  }

  public static void printCheeses() {
    System.out.println("These are the cheeses in the shop: ");
    ArrayList<Cheese> cheeses = cheeseService.getCheeses();
    for (Cheese cheese : cheeses) {
      System.out.println(cheese.getName() + " " + cheese.getCost() + " eur/kg.");
    }
  }
}


public class Cheese{
  private String name; 
  private int cost;


  public Cheese (String name, int cost){
    this.name = name;
    this.cost = cost;
  }

  public void setName(String name){
    this.name = name;
  }

  public String getName(){
    return name;
  }
  
  public void setCost(int cost){
    this.cost = cost;
  }

  public int getCost(){
    return cost;
  }
}


import java.util.ArrayList;

public class CheeseService {
    private ArrayList<Cheese> cheeses = new ArrayList<>();

    public void addCheese(Cheese cheese) {
        cheeses.add(cheese);
    }

    public ArrayList<Cheese> getCheeses() {
        return new ArrayList<>(cheeses);
    }
}

import java.util.ArrayList;

public class CheeseShop {
    private ArrayList<Cheese> cart = new ArrayList<>();

    public void addToCart(Cheese cheese) {
        cart.add(cheese);
    }

    public int cartTotalCost() {
        int total = 0;
        for (Cheese cheese : cart) {
            total += cheese.getCost();
        }
        return total;
    }

    public Cheese findCheeseByName(String name, ArrayList<Cheese> cheeses) {
        for (Cheese cheese : cheeses) {
            if (cheese.getName().equalsIgnoreCase(name)) {
                return cheese;
            }
        }
        return null;
    }
}
```
