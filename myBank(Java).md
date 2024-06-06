```java
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

// This is the Client class
class Client {

// This is a class variable
    private static int numberOfClients = 0;

// These are instance variables
    private String id;
    private String name;
    private List<Account> accounts;

// Constructor method
    public Client(String id, String name) {
        this.id = id;
        this.name = name;
        this.accounts = new ArrayList<>();
        numberOfClients++;
    }

// Method to add an account to the client
    public void addAccount(Account account) {
        this.accounts.add(account);
    }

// Getter method for the client's ID
    public String getId() {
        return this.id;
    }

// Getter method for the client's name
    public String getName() {
        return this.name;
    }

// Getter method for the client's accounts
    public List<Account> getAccounts() {
        return this.accounts;
    }

// Static method to get the number of clients
    public static int getNumberOfClients() {
        return numberOfClients;
    }
}

// This is the Account class
class Account {

// Instance variables
    private String number;
    private String currency;
    private double balance;
    private List<Transaction> transactions;

// Constructor method with initial balance
    public Account(String number, String currency, double balance) {
        this.number = number;
        this.currency = currency;
        this.balance = balance;
        this.transactions = new ArrayList<>();
    }

// Constructor method without initial balance
    public Account(String number, String currency) {
        this(number, currency, 0.0);
    }

// Method to make a deposit
    public void makeDeposit(double amount, String note) {
        this.transactions.add(new Transaction(this.currency, amount, note));
        this.balance += amount;
    }

// Method to make a withdrawal
    public void makeWithdrawal(double amount, String note) {
        this.transactions.add(new Transaction(this.currency, -amount, note));
        this.balance -= amount;
    }

// Getter method for the account number
    public String getNumber() {
        return this.number;
    }

// Getter method for the account currency
    public String getCurrency() {
        return this.currency;
    }

// Getter method for the account balance
    public double getBalance() {
        return this.balance;
    }

// Getter method for the transactions
    public List<Transaction> getTransactions() {
        return this.transactions;
    }
}

// This is the Transaction class
class Transaction {

// Instance variables
    private String currency;
    private double amount;
    private String note;
    private Date timeStamp;

// Constructor method
    public Transaction(String currency, double amount, String note) {
        this.currency = currency;
        this.amount = amount;
        this.note = note;
        this.timeStamp = new Date();
    }

// Getter method for the transaction currency
    public String getCurrency() {
        return this.currency;
    }

 // Getter method for the transaction amount
    public double getAmount() {
        return this.amount;
    }

 // Getter method for the transaction note
    public String getNote() {
        return this.note;
    }

// Getter method for the transaction timestamp
    public Date getTimeStamp() {
        return this.timeStamp;
    }
}


// This is the Main class
public class Main {

// This is the main method
    public static void main(String[] args) {

// This is a List of Client instances
        List<Client> clients = new ArrayList<>();

// Adding Client instances to the list
        clients.add(new Client("12345", "Anna"));
        clients.add(new Client("5678", "Oskar"));
        clients.add(new Client("13567", "Elsa"));

// Adding Account instances to the first Client
        clients.get(0).addAccount(new Account("EE2348598", "EUR", 1000.00));
        clients.get(0).addAccount(new Account("EE3457725", "JPY", 25000.00)); // Corrected currency from "YPY" to "JPY"
        clients.get(0).addAccount(new Account("EE4330976", "USD"));
// Adding an Account instance to the second Client
        clients.get(1).addAccount(new Account("EE2340975", "PLN", 47800.00));
// Adding an Account instance to the third Client
        clients.get(2).addAccount(new Account("EE2309534", "SEK", 200.18));

        // Make some transactions
        clients.get(0).getAccounts().get(0).makeDeposit(1200, "Salary");
        clients.get(0).getAccounts().get(0).makeWithdrawal(50, "Grocery");
        clients.get(0).getAccounts().get(0).makeWithdrawal(140, "Clothes");
        clients.get(0).getAccounts().get(0).makeWithdrawal(20, "Dinner");

        System.out.println(Client.getNumberOfClients());

        System.out.println(clients.get(1).getId());

        System.out.println("We have clients in our bank:");
        for (Client client : clients) {
            System.out.println("Client " + client.getName() + " has the following accounts: ");
            for (Account account : client.getAccounts()) {
                System.out.println("    " + account.getNumber() + " (" + account.getCurrency() + ") " + account.getBalance());
                for (Transaction transaction : account.getTransactions()) {
                    System.out.println("        " + transaction.getTimeStamp() + " " + transaction.getAmount());
                }
            }
        }
    }
}
```

