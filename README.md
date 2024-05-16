# Звіт з лабораторної роботи №9. Ланцюжок викликів.
> Виконала студентка групи ІКМ-М223в **Павленко Дарина**
> 
### Мета: Рефакторінг коду з проблемою «Ланцюжок викликів».

### Завдання 1: Наданий код

    class Customer:
        def __init__(self, account):
            self.account = account

        def get_account(self):
            return self.account

    class Account:
        def __init__(self, balance):
            self.balance = balance

        def get_balance(self):
            return self.balance

    class Bank:
        def __init__(self, customer):
            self.customer = customer

        def get_customer(self):
            return self.customer

    # Користування ланцюжком викликів
    bank = Bank(Customer(Account(1000)))
    balance = bank.get_customer().get_account().get_balance()
    print(f"The balance is: {balance}")

### Рішення

Для реорганізації коду та уникнення прямого ланцюжка викликів можна використати делегування обов'язків або введення методу, який інкапсулює цей ланцюжок.
Можна ввести метод get_balance() у клас Bank, який делегує виклики до потрібних методів:

    class Customer:
        def __init__(self, account):
            self._account = account

        def get_account(self):
            return self._account

    class Account:
        def __init__(self, balance):
            self._balance = balance

        def get_balance(self):
            return self._balance
  
    class Bank:
        def __init__(self, customer):
            self._customer = customer

        def get_customer(self):
            return self._customer

        def get_balance(self):
            return self._customer.get_account().get_balance()

    # Користування методу для отримання балансу
    bank = Bank(Customer(Account(1000)))
    balance = bank.get_balance()
    print(f"The balance is: {balance}")

Метод get_balance() у класі Bank інкапсулює логіку отримання балансу клієнта через делегування викликів до відповідних методів у класах Customer та Account. Це дозволяє уникнути прямого ланцюжка викликів та робить код більш модульним та зрозумілим.
