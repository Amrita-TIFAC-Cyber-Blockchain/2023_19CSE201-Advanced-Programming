# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Bank Management System
```
import os
import pickle

''' Base class for different types of accounts'''
class Customer:
    def __init__(self, id, name, account_number, pin, age, phone, balance,type):
        self.id = id
        self.name = name
        self.account_number = account_number
        self.pin = pin
        self.age = age
        self.phone = phone
        self.balance = balance
        self.type=type

''' Derived class for Savings Account'''
class SavingsAccount(Customer):
    def __init__(self, id, name, account_number, pin, age, phone, balance):
        super().__init__(id, name, account_number, pin, age, phone, balance,"Savings")

''' Derived class for Salary Account'''
class SalaryAccount(Customer):
    def __init__(self, id, name, account_number, pin, age, phone, balance,company):
        super().__init__(id, name, account_number, pin, age, phone, balance,"Salary")
        self.company=company

''' Derived class for Fixed Deposit Account'''
class FixedDepositAccount(Customer):
    def __init__(self, id, name, account_number, pin, age, phone, balance,interest):
        super().__init__(id, name, account_number, pin, age, phone, balance,"Fixed")
        self.interest=interest


''' Bank class encapsulating all the operations'''
class Bank:
    def __init__(self):
        self.MINIMUM_BALANCE = 1000
        self.FILE_NAME = "Bank.pkl"

    '''Initializing the file to store all the records, If file does not exist then it creates a new file'''
    def initialize_file(self):
        with open(self.FILE_NAME, "wb") as file:
            inp1 = Customer(1, "Admin", "000000", 1234, 100, 1234568, 5000,"Admin")
            pickle.dump([inp1], file)
            print("File created and initialized successfully!")

    '''Resets the whole file into a new file '''
    def reset(self):
        self.initialize_file()
        
    ''' Displaying all accounts details'''
    def display(self):
        try:
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
                for inp in data:
                    print(f"id = {inp.id} name = {inp.name} Account_type={inp.type} Account={inp.account_number} pin={inp.pin} age={inp.age} "
                        f"Phone number: {inp.phone} Balance={inp.balance}")
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")

    '''Displays the account details based on the type of account'''
    def displayBytype(self):
        try:
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
                ch = int(input("\nEnter the required number:\n1.Savings\n2. Salary\n3. Fixed_deposit\n"))
                if ch == 1:
                    for inp in data:
                        if inp.type=="Savings":
                            print(f"id = {inp.id} name = {inp.name} Account_type={inp.type} Account={inp.account_number} pin={inp.pin} age={inp.age} "
                        f"Phone number: {inp.phone} Balance={inp.balance} ")
                elif ch == 2:
                    for inp in data:
                        if inp.type=="Salary":
                            print(f"id = {inp.id} name = {inp.name} Account_type={inp.type} Account={inp.account_number} pin={inp.pin} age={inp.age} "
                        f"Phone number: {inp.phone} Balance={inp.balance} Company={inp.company}")
                elif ch == 3:
                    for inp in data:
                        if inp.type=="Fixed_deposit":
                            print(f"id = {inp.id} name = {inp.name} Account_type={inp.type} Account={inp.account_number} pin={inp.pin} age={inp.age} "
                        f"Phone number: {inp.phone} Balance={inp.balance} Interest={inp.interest}")
                else:
                    print("Entered type does not exist")
                    return 0
                
                            
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")

    '''Creating a new account'''
    def add_account(self):
        if not os.path.exists(self.FILE_NAME):
            self.initialize_file()

        try:
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
        except (EOFError, FileNotFoundError):
            data = []

        print("\n ACCOUNT CREATION: \n1.Savings \n2.Salary \n3.Fixed_Deposit")
        type=int(input("Enter the type of account:"))
        cus = Customer(0, "", "", 0, 0, 0, 0,"NONE")
        cus.account_number = input("Enter the account number: ")

        # Check if the account number already exists
        for inp in data:
            if inp.account_number == cus.account_number:
                print("Account with the same number already exists. Please choose a different account number.")
                return

        cus.id = data[-1].id + 1 if data else 1  # Incrementing id for the new account
        cus.name = input("Enter the name: ")
        cus.age = int(input("Enter the age: "))
        cus.phone = int(input("Enter the phone number: "))
        cus.balance = max(int(input("Enter the balance: ")), self.MINIMUM_BALANCE)  # Ensuring minimum balance
        cus.pin = int(input("Enter the pin: "))

        if type==1:
            acc=SavingsAccount(cus.id,cus.name,cus.account_number,cus.pin,cus.age,cus.phone,cus.balance)
        elif type==2:
            company=input("Enter the company name: \n")
            acc=SalaryAccount(cus.id,cus.name,cus.account_number,cus.pin,cus.age,cus.phone,cus.balance,company)
        elif type==3:
            interest=input("Enter the interest rate: \n")
            acc=FixedDepositAccount(cus.id,cus.name,cus.account_number,cus.pin,cus.age,cus.phone,cus.balance,interest)
        else:
            print("Invalid Account type try again")
            return
        print("Account created successfully!")

        data.append(acc)

        with open(self.FILE_NAME, "wb") as file:
            pickle.dump(data, file)
            print("Contents to file written successfully!")


    '''Counting number of accounts present in the bank'''
    def count(self):
        try:
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
                n = len(data)
                print(f"\nNo: of records: {n}")
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")

    '''Searching a account based on account number'''
    def search(self):
        try:
            ac_no = input("Enter the Account number to be searched: ")
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
                found = False
                for inp in data:
                    if inp.account_number == ac_no:
                        found = True
                        print(f"id = {inp.id} name = {inp.name} Account={inp.account_number} pin={inp.pin} Balance={inp.balance}")
                if not found:
                    print("\nnot found")
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")


    '''Sort the account details based on balance only in the display'''
    def sortdis(self):
        try:
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
                data.sort(key=lambda x: x.balance)
                for inp in data:
                    print(f"id = {inp.id} name = {inp.name} Account={inp.account_number} pin={inp.pin} Balance={inp.balance}")
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")


    '''Sort the account details based on balance in the display and file also '''
    def sortfile(self):
        try:
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
                data.sort(key=lambda x: x.balance)
                for index, inp in enumerate(data):
                    inp.id = index + 1  # Reassign unique id based on the order in the sorted list
                    print(f"id = {inp.id} name = {inp.name} Account={inp.account_number} pin={inp.pin} Balance={inp.balance}")
            with open(self.FILE_NAME, "wb") as file:
                pickle.dump(data, file)
            print("File sorted by balance successfully!")
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")


    '''Sort the account details based on account number only in the display'''
    def sortdisn(self):
        try:
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
                data.sort(key=lambda x: x.account_number)
                for inp in data:
                    print(f"id = {inp.id} name = {inp.name} Account={inp.account_number} pin={inp.pin} Balance={inp.balance}")
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")


    '''Sort the account details based on account number in the display and file also '''
    def sortfilen(self):
        try:
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
                data.sort(key=lambda x: x.account_number)
                for index, inp in enumerate(data):
                    inp.id = index + 1  # Reassign unique id based on the order in the sorted list
                    print(f"id = {inp.id} name = {inp.name} Account={inp.account_number} pin={inp.pin} Balance={inp.balance}")
            with open(self.FILE_NAME, "wb") as file:
                pickle.dump(data, file)
            print("File sorted by account number successfully!")
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")

    '''Verifing the user based on account number and pin of the respective account'''
    def login(self):
        try:
            ac_no = input("Enter the Account number: ")
            pin = int(input("Enter the pin: "))
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
                found = False
                for inp in data:
                    if inp.account_number == ac_no:
                        found = True
                        if inp.pin == pin:
                            print(f"id = {inp.id} name = {inp.name} AccountType={inp.type} Account={inp.account_number} pin={inp.pin} Balance={inp.balance}")
                        else:
                            print("Incorrect credentials")
                        break
                if not found:
                    print("\nnot found")
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")

    '''Operating  the transactions of a account'''
    def user(self):
        ch = int(input("\nEnter the required number:\n1.See Balance\n2. Withdraw\n3. Deposit\n"))
        if ch == 1:
            self.see_balance()
        elif ch == 2:
            self.withdraw()
        elif ch == 3:
            self.deposit()
        else:
            return 0

    '''Removing record of any selected account based on the account number'''
    def remove_record(self):
        try:
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)

            ac_no = input("Enter the Account number to be deleted: ")
            found = False

            for inp in data:
                if inp.account_number == ac_no:
                    found = True
                    data.remove(inp)
                    print("Record removed successfully!")
                    break

            if not found:
                print("Account not found.")

            with open(self.FILE_NAME, "wb") as file:
                pickle.dump(data, file)
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")

    '''Displays the balance of a account'''
    def see_balance(self):
        try:
            ac_no = input("Enter the Account number: ")
            pin = int(input("Enter the pin: "))
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
                found = False
                for inp in data:
                    if inp.account_number == ac_no:
                        found = True
                        if inp.pin == pin:
                            print(f"Balance remaining: {inp.balance}")
                        else:
                            print("Incorrect credentials")
                        break
                if not found:
                    print("\nnot found")
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")


    '''Handles the Withdraw transactions of a account'''
    def withdraw(self):
        try:
            ac_no = input("Enter the Account number to be updated: ")
            pin = int(input("Enter the pin: "))
            amount = int(input("Enter the amount to be deducted: "))
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
            updated_data = []  # Initialize an empty list to store updated records
            for inp in data:
                if inp.account_number == ac_no:
                    if inp.pin == pin:
                        if inp.type=="Fixed Deposit":
                            print("Cannot withdraw. Visit/contact the bank center the clarify.")
                            return
                        if amount > inp.balance:
                            print("Error: Withdrawal amount exceeds the current balance.")
                            return
                        else:
                            inp.balance -= amount
                    else:
                        print("Incorrect credentials. Withdrawal failed.")
                        return

                updated_data.append(inp)  # Append each record to the updated_data list
            with open(self.FILE_NAME, "wb") as file:
                pickle.dump(updated_data, file)
            print("Transaction Successful!")
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")


    '''Handles the deposit transactions of a account'''
    def deposit(self):
        try:
            ac_no = input("Enter the Account number to be updated: ")
            pin = int(input("Enter the pin: "))
            amount = int(input("Enter the amount to be deposited: "))
            with open(self.FILE_NAME, "rb") as file:
                data = pickle.load(file)
            updated_data = []  # Initialize an empty list to store updated records
            for inp in data:
                if inp.account_number == ac_no:
                    if inp.pin == pin:
                        if inp.type=="Salary":
                            print("Cannot deposit money into account. Request organization to do so")
                            return
                        if amount<0:
                            print("Invalid amount. Please retry")
                            return
                        inp.balance += amount
                updated_data.append(inp)  # Append each record to the updated_data list
            with open(self.FILE_NAME, "wb") as file:
                pickle.dump(updated_data, file)
            print("Transaction Successful!")
        except FileNotFoundError:
            print("File not found. Please add an account to create the file.")

def main():
    '''Creating Object to the bank class'''
    b=Bank()
    while True:
        print("\n1. Add an account")
        print("2. Reset")
        print("3. Display")
        print("4. Display based on type")
        print("5. User")
        print("6. Remove record")
        print("7. Sort by account in display")
        print("8. Sort by account in file")
        print("9. Sort by balance in display")
        print("10. Sort by balance in file")
        print("11. Search")
        print("12. Number of entries")
        print("0. Exit")

        choice = int(input("Enter your choice: "))


        '''Making menu driven program '''
        if choice == 1:
            b.add_account()
        elif choice == 2:
            b.reset()
        elif choice == 3:
            b.display()
        elif choice == 4:
            b.displayBytype()
        elif choice == 5:
            b.user()
        elif choice == 6:
            b.remove_record()
        elif choice == 7:
            b.sortdisn()
        elif choice == 8:
            b.sortfilen()
        elif choice == 9:
            b.sortdis()
        elif choice == 10:
            b.sortfile()
        elif choice == 11:
            b.search()
        elif choice == 12:
            b.count()
        elif choice == 0:
            break
        else:
            print("Invalid choice. Please enter a valid option.")

main()

```
