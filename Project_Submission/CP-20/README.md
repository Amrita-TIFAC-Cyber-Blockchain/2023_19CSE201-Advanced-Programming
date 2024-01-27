# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

### ATM System
```
class Bank:
    def __init__(self):
        '''
        Initializes the Bank class with a list of users and their account details.
        '''
        self.name = ""
        self.pin = 0
        self.accnumber = 0
        self.type = ""
        self.amount = 0
        self.tob = 0

    def login(self):
        '''
        Prompts the user to enter their account number and PIN to log in.
        '''
        while True:
            acc_number = int(input("Enter your Account number: "))
            pin = int(input("Enter your PIN: "))

            user_found = False
            for user in self.users:
                if user["accnumber"] == acc_number and user["pin"] == pin:
                    self.name = user["name"]
                    self.accnumber = user["accnumber"]
                    self.pin = user["pin"]
                    self.type = user["type"]
                    self.tob = user["tob"]
                    user_found = True
                    break

            if user_found:
                print("Login successful. Welcome,", self.name)
                break
            else:
                print("Invalid Account number or PIN. Please try again.")

    def get_valid_pin(self):
        '''
        Prompts the user to enter a valid 4-digit PIN.
        '''
        while True:
            pin = int(input("SetUp PIN (4 digits): "))
            if 1000 <= pin <= 9999:
                return pin
            else:
                print("Invalid PIN. Please enter a 4-digit PIN.")

    def deposit(self):
        '''
        Prompts the user to enter the amount to be deposited and updates the account balance.
        '''
        self.amount = int(input("\nEnter amount to be Deposited: "))
        print("\nRs.", self.amount, "credited to your Account")
        self.tob += self.amount
        print("\nTotal Balance:", self.tob)

    def showbal(self):
        '''
        Displays the available balance in the user's account.
        '''
        print("\nAvailable Balance:", self.tob)

    def withdrawl(self):
        '''
        Prompts the user to enter the amount to be withdrawn and checks if the balance is sufficient.
        '''
        a = int(input("\nEnter amount to withdraw: "))
        available_bal = self.tob - a
        if available_bal >= 0:
            print("Available Balance:", available_bal)
        else:
            print("Insufficient Balance")

    def change_pin(self):
        '''
        Prompts the user to enter their current PIN and a new PIN to change the PIN.
        '''
        while True:
            current_pin = int(input("\nEnter your Current PIN: "))
            if current_pin == self.pin:
                break
            else:
                print("Incorrect PIN. Try again.")
        new_pin = self.get_valid_pin()
        if new_pin != self.pin:
            self.pin = new_pin
            print("PIN Changed\n")
        else:
            print("New PIN matches the Current PIN. Please choose a different one.\n")

class User(Bank):
    def __init__(self):
        super().__init__()
        self.users = [
            {"name": "Ramaguru Radhakrishnan", "pin": 2100, "accnumber": 123456, "type": "Savings", "amount": 5000, "tob": 5000},
            {"name": "Harshitha Ranjith", "pin": 2122, "accnumber": 789012, "type": "Current", "amount": 8000, "tob": 8000},
            {"name": "Sri Sai Tanvi Sonti", "pin": 2172, "accnumber": 345678, "type": "Savings", "amount": 10000, "tob": 10000},
        ]

    def setvalue(self):
        '''
        Prompts the user to enter their name, PIN, account number, account type, and balance to set the values.
        '''
        while True:
            new_name = input("Enter your name: ")

            # Check if the entered name already exists
            if any(user["name"].lower() == new_name.lower() for user in self.users):
                print("User with the same name already exists. Please choose a different name.")
            else:
                self.name = new_name
                break

        self.pin = self.get_valid_pin()
        while True:
            try:
                self.accnumber = int(input("Enter your 6-digit Account number: "))
                if 100000 <= self.accnumber <= 999999:
                    break
                else:
                    print("Invalid account number. Please enter a 6-digit number.")
            except ValueError:
                print("Invalid input. Please enter a valid number.")
        while True:
            account_type_choice = input("Choose Account Type:\n1. Savings\n2. Current\nEnter your choice (1 or 2): ")
            if account_type_choice in ['1', '2']:
                self.type = "Savings" if account_type_choice == '1' else "Current"
                break
            else:
                print("Invalid choice. Please enter 1 for Savings or 2 for Current.")
        self.tob = int(input("Enter Balance to confirm: "))

    def showdata(self):
        '''
        Displays the user's account details.
        '''
        print("Name:", self.name)
        print("Account No:", self.accnumber)
        print("Account type:", self.type)
        print("Balance:", self.tob)

def main():
    '''
    The main function that interacts with the user and provides banking services.
    '''
    while True:
        user = User()
        print("--------------------- WELCOME ------------------------")
        print("------------------- TO CYS BANK ----------------------\n\n")
        option = int(input("SELECT:\n        1. Existing User\n        2. New User\n        3. Exit\n"))

        if option == 1:
            user.login()
        elif option == 2:
            user.setvalue()
        elif option == 3:
            exit()
        else:
            print("Invalid choice. Exiting.")
            exit()

        while True:
            print("\n\n HELLO", user.name.upper(), "\n")
            print("\nSelect Our Service:")
            print("\t1. Account Details")
            print("\t2. Balance Enquiry")
            print("\t3. Deposit")
            print("\t4. Withdraw")
            print("\t5. Change PIN")
            print("\t6. Logout")

            choice = int(input("Enter your choice: "))

            if choice == 1:
                user.showdata()
            elif choice == 2:
                user.showbal()
            elif choice == 3:
                user.deposit()
            elif choice == 4:
                user.withdrawl()
            elif choice == 5:
                user.change_pin()
            elif choice == 6:
                print("You have successfully logged out.")
                break
            else:
                print("Wrong Choice!!")

if __name__ == "__main__":
    main()

```