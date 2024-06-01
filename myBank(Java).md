```java
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

class Client {
    private static int numberOfClients = 0;
    private String id;
    private String name;
    private List<Account> accounts;

    public Client(String id, String name) {
        this.id = id;
        this.name = name;
        this.accounts = new ArrayList<>();
        numberOfClients++;
    }

    public void addAccount(Account account) {
        this.accounts.add(account);
    }

    public String getId() {
        return this.id;
    }

    public String getName() {
        return this.name;
    }

    public List<Account> getAccounts() {
        return this.accounts;
    }

    public static int getNumberOfClients() {
        return numberOfClients;
    }
}

class Account {
    private String number;
    private String currency;
    private double balance;
    private List<Transaction> transactions;

    public Account(String number, String currency, double balance) {
        this.number = number;
        this.currency = currency;
        this.balance = balance;
        this.transactions = new ArrayList<>();
    }

    public Account(String number, String currency) {
        this(number, currency, 0.0);
    }

    public void makeDeposit(double amount, String note) {
        this.transactions.add(new Transaction(this.currency, amount, note));
        this.balance += amount;
    }

    public void makeWithdrawal(double amount, String note) {
        this.transactions.add(new Transaction(this.currency, -amount, note));
        this.balance -= amount;
    }

    public String getNumber() {
        return this.number;
    }

    public String getCurrency() {
        return this.currency;
    }

    public double getBalance() {
        return this.balance;
    }

    public List<Transaction> getTransactions() {
        return this.transactions;
    }
}

class Transaction {
    private String currency;
    private double amount;
    private String note;
    private Date timeStamp;

    public Transaction(String currency, double amount, String note) {
        this.currency = currency;
        this.amount = amount;
        this.note = note;
        this.timeStamp = new Date();
    }

    public String getCurrency() {
        return this.currency;
    }

    public double getAmount() {
        return this.amount;
    }

    public String getNote() {
        return this.note;
    }

    public Date getTimeStamp() {
        return this.timeStamp;
    }
}

public class Main {
    public static void main(String[] args) {
        List<Client> clients = new ArrayList<>();
        clients.add(new Client("12345", "Anna"));
        clients.add(new Client("5678", "Oskar"));
        clients.add(new Client("13567", "Elsa"));

        clients.get(0).addAccount(new Account("EE2348598", "EUR", 1000.00));
        clients.get(0).addAccount(new Account("EE3457725", "JPY", 25000.00)); // Corrected currency from "YPY" to "JPY"
        clients.get(0).addAccount(new Account("EE4330976", "USD"));
        clients.get(1).addAccount(new Account("EE2340975", "PLN", 47800.00));
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

