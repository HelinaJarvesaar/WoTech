```py
import datetime

class Client:
  pass  #nothing happens here

  number_of_clients = 0

  def __init__ (self, id, name):    # i dont't require account, cause I dont want to add list
    self.id = id                    # self.id ehk each clients id = id
    self.name = name
    self.accounts =[]
    Client.number_of_clients += 1
  
  def add_account(self, account):
    self.accounts.append(account)    # adding new account

class Account:
  def __init__ (self, number, currency, balance = 0.0): # it will ask for blance but if we do not provide, then it is 0 
    self.number = number
    self.currency = currency
    self.balance = balance
    self.transactions = []

  def make_deposite(self, amount, note):    # every account can make deposit
    self.transactions.append(Transaction(self.currency, amount, note))
    self.balance += amount

  def make_withdrawal (self,amount, note):
    self.transactions.append(Transaction(self.currency, -amount, note))
    self.balance -= amount  # every account can make withdarwal

class Transaction:
  def __init__(self, currency, amount, note):
    self.currency = currency
    self.amount = amount
    self.note = note
    self.time_stamp = datetime.datetime.now()  # adds automatically (google...import datetime) -
                                               # when transaction is made we made transaction object and it automatically adds datetime
                                               # in this (without parameter require) way no one can owerwrite it, its safer!
# adding clients to list
clients = []
clients.append(Client("12345", "Anna"))
clients.append(Client("5678", "Oskar"))
clients.append(Client("13567", "Elsa"))

# adding accounts to clients
clients[0].add_account(Account("EE2348598", "EUR", 1000.00)) # we add account to client [0]
clients[0].add_account(Account("EE3457725", "YPY", 25000.00))    # we add another account to client [0]
clients[0].add_account(Account("EE4330976", "USD"))
clients[1].add_account(Account("EE2340975", "PLN", 47800.00))
clients[2].add_account(Account("EE2309534", "SEK", 200.18))


#lets make some transactions

clients[0].accounts[0].make_deposite(1200, "Salary")  # "Salary" is a note
clients[0].accounts[0].make_withdrawal(50, "Crocery")
clients[0].accounts[0].make_withdrawal(140, "Clothes")
clients[0].accounts[0].make_withdrawal(20, "Dinner")

print(Client.number_of_clients)

print(clients[1].id)                  # NB! use dot . ! 

print(f"We have clients in our bank:")
for client in clients:
  print(f"Clients {client.name} has the following accounts: ")
  for account in client.accounts:
    print(f'    {account.number} ({account.currency}) {account.balance}')
    for transaction in account.transactions:
      print(f"        {transaction.time_stamp} {transaction.amount}")
```


