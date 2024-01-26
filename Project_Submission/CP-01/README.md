# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Bank Management System 
```
"""
Team 1
Simple Bank Management System
Done By : Namitha
          Anagh
          Adarsh
This Python program presents a comprehensive banking system featuring three account plans: Premium, Basic, and Advanced.
Users can perform various actions like creating accounts, checking balances, exploring plan benefits, calculating interest, and deleting accounts.
Data storage is managed using MySQL, ensuring secure and efficient data handling.
The program follows object-oriented programming (OOP) principles, employing classes and methods for streamlined account management.
With a user-friendly interface and robust functionality, it offers a complete banking solution for both customers and administrators.
The system ensures data integrity and security through proper error handling and validation mechanisms.
Each account plan comes with its set of benefits and interest rates, providing flexibility and choice to users based on their financial needs and preferences.
Overall, this program showcases a practical implementation of banking operations in Python, demonstrating proficiency in OOP concepts, database integration,
and user interaction design.
"""

import random
import datetime
import mysql.connector as mys
mydb = mys.connect(host='localhost', user= 'root', password = 'user', database = 'veritas')
if mydb.is_connected() == False:
    print("Error connecting to mysql")
mycursor = mydb.cursor()

class Account:
    def __init__(self, name, mobile_no, email, account_balance, membership_year):
        self.name = name
        self.mobile_no = mobile_no
        self.email = email
        self.account_balance = account_balance
        self.membership_year = membership_year

    def get_name(self):
        return self.name

    def set_name(self, name):
        self.name = name

    def get_mobile_no(self):
        return self.mobile_no

    def set_mobile_no(self, mobile_no):
        self.mobile_no = mobile_no

    def get_email(self):
        return self.email

    def set_email(self, email):
        self.email = email

    def get_account_balance(self):
        return self.account_balance

    def set_account_balance(self, account_balance):
        self.account_balance = account_balance

    def get_membership_year(self):
        return self.membership_year

    def set_membership_year(self, membership_year):
        self.membership_year = membership_year

class PremiumPlan(Account):
    def __init__(self, name, mobile_no, email, account_balance, membership_year):
        super().__init__(name, mobile_no, email, account_balance, membership_year)

    def calculate_interest(self, years):
        interest_rate = 0.15
        return round(self.account_balance * (1 + interest_rate) ** years, 2)

class BasicPlan(Account):
    def __init__(self, name, mobile_no, email, account_balance, membership_year):
        super().__init__(name, mobile_no, email, account_balance, membership_year)

    def calculate_interest(self, years):
        interest_rate = 0.07
        return round(self.account_balance * (1 + interest_rate) ** years, 2)

class AdvancedPlan(Account):
    def __init__(self, name, mobile_no, email, account_balance, membership_year):
        super().__init__(name, mobile_no, email, account_balance, membership_year)

    def calculate_interest(self, years):
        interest_rate = 0.12
        return round(self.account_balance * (1 + interest_rate) ** years, 2)
# different plans for the customers are taken as dict names with account number as key
# order of data entry [name, mobile no, email, account balance, membership year]
premium_plan = {1023156: ['Namitha Sudhish Nair', '5612325654', 'namitha@gmail.com', 12332, 2015],
                1235687: ['Adarsh R K', '562348916', 'adarshrk123@gmail.com', 26500, 2016],
                2315658: ['Anagh Shaji', '2356984112', 'Anagh600@jafco.ae', 12356, 2017]}

basic_plan = {1235684: ['Nandana Mahesh', '5612356981', 'Nandu@gmail.com', 12569, 2020],
              1239865: ['R Thanuj', '5615684165', 'r_thanuj@gmail.com', 22365, 2019]}

advanced_plan = {1235149: ['Navarang C D', '5565165865', 'Nava_CD@gmz.ae', 56513, 2013],
                 5665416: ['Rahul Shanker', '9581325865', 'rahul_shanker@gmail.com', 263365, 2015]}

list_id = [1023156, 1235687, 2315658, 1235684, 1239865, 1235149, 5665416]
list_mobile_no = ['5612325654', '562348916', '2356984112', '5612356981', '5615684165', '5565165865', '9581325865']

print('-----  WELCOME TO ANA NATIONAL BANK  -----')
flag = False # used to break out of the loop
flag1 = False # used to break out of the loop
while True:
    if flag:
        break
    print()
    print('Choose from the options:')
    print('''(1) Start a new account
(2) Check account balance
(3) Check plan features and benefits
(4) Interest data
(5) Delete account''')
    print()

    user_input = int(input("Enter the option number: "))
    if user_input == 1:

        while True:
            if flag1:
                break
            new_name = input('Enter full name: ')
            new_name = new_name.title()
            print('Plans available: (1) Premiun (2) Basic (3) Advanced')
            while True:
                plan_no = int(input('Enter the name of the plan: '))
                if plan_no == 1:
                    plan_name = 'premium_plan'
                    break
                elif plan_no == 2:
                    plan_name = 'basic_plan'
                    break
                elif plan_no == 3:
                    plan_name = 'advanced_plan'
                    break
                else:
                    print('Wrong input, Try again')
            new_account_balance = float(input('Enter the amount deposited: '))
            while True:
                if flag == 1:
                    break
                new_mobile_no = input('Enter mobile number: ')
                mycursor.execute('select mobile_no from userdata')
                rec = mycursor.fetchall()
                for mob in rec:
                    if new_mobile_no in mob:
                        print('Number already taken, try another one')
                    else:
                        list_mobile_no.append(new_mobile_no)
                        flag = 1
                        break
            new_email = input('Enter email address: ')
            while True:
                if flag == 1:
                    break
                id = random.randint(1000000, 9999999) # generating a new account using random module
                mycursor.execute('Select id from userdata')
                rec = mycursor.fetchall()
                for x in rec:
                    if id not in x:
                        list_id.append(id)
                        flag = 1  
                    else:
                        break
            now = str(datetime.date.today()) # datetime for extracting current year

            if plan_name == 'premium_plan':
                info = [new_name, new_mobile_no, new_email, new_account_balance, now]
                premium_plan[id] = info
                print('Your acoount number -', id)
                print('Your information', info[:4])
                mycursor.execute('insert into userdata values({},'{}',{},{}id, new_name, new_mobile_no, new_email, new_account_balance, now
            elif plan_name == 'basic_plan':
                info = [new_name, new_mobile_no, new_email, new_account_balance, now]
                basic_plan[id] = info
                print('Your acoount number -', id)
                print('Your information', info[:4])
            elif plan_name == 'advanced_plan':
                info = [new_name, new_mobile_no, new_email, new_account_balance, now]
                advanced_plan[id] = info
                print('Your acoount number -', id)
                print('Your information', info[:4])
                
            comformation = int(input('Please confirm to continue registration (1) continue (2) return to details: '))
            if comformation == 1:
                print('Registration complete, Thankyou')
            elif comformation == 2:
                continue # goes to the second loop in the code 
            
            exit_data = int(input('(1) Exit (2) Return to main menu: '))
            if exit_data == 1:
                flag = True
                break

           elif exit_data == 2:
                flag1 = True
                continue

    elif user_input == 2:
        while True:
            id_old = int(input('Enter account number: '))
            if id_old not in list_id: # checking if id exists
                print('Wrong account number')
                continue
            else:
                break
        if id_old in premium_plan:
            print('Account balance -', premium_plan[id_old][3])
        elif id_old in basic_plan:
            print('Account balance -', basic_plan[id_old][3])
        elif id_old in advanced_plan:
            print('Account balance -', advanced_plan[id_old][3])
            
        exit_data = int(input('(1) Exit (2) Return to main menu: '))
        if exit_data == 1:
            break
        elif exit_data == 2:
            continue

    elif user_input == 3:
        while True:
            id_old = int(input('Enter account number: '))
            if id_old not in list_id:
                print('Wrong account number')
                continue
            else:
                break
        print()
        if id_old in premium_plan:
            print(f'''Hi {premium_plan[id_old][0]},
Discover exiting offers and discounts

(1) You receive an annually compounded interest of 15%
(2) Get access to 24/7 helpline
(3) Enjoy quick transfer of money with top priority

Check out other offers!
50% discount at One day trip to Lakshwadeep Islands
30% Cashback on INR 300 or above at Lulu Mall
Buy one get one free at Hourtime watches

Terms and conditions apply''')

        elif id_old in basic_plan:
            print(f'''Hi {basic_plan[id_old][0]},
Discover exiting offers and discounts

(1) You receive an annually compounded interest of 7%
(2) Access our helpline facility
(3) Enjoy quick transfer of money

Check out other offers!
50% discount at One day trip to Connor
25% Cashback on INR 300 or above at Lulu Mall
10% discount at All India across Duty Free

Terms and conditions apply''')

        elif id_old in advanced_plan:
            print(f'''Hi {advanced_plan[id_old][0]},
Discover exiting offers and discounts

(1) You receive an annually compounded interest of 12%
(2) Access our helpline facility
(3) Enjoy quick transfer of money with top priority

Check out other offers!
25% discount at One day trip to Munnar
Get free entry to Wonderla Kochi
15% discount at All India across Duty Free

Terms and conditions apply''')

        exit_data = int(input('(1) Exit (2) Return to main menu: '))
        if exit_data == 1:
            break
        elif exit_data == 2:
            continue

    elif user_input == 4:
        while True:
            id_old = int(input('Enter account number: '))
            if id_old not in list_id:
                print('Wrong account number')
                continue
            else:
                break
        if id_old in premium_plan:
            P = premium_plan[id_old][3]
            n = 1
            r = 15
            y = int(input("Enter the amount of years. "))
            FV = round(P * (((1 + ((r/100.0)/n)) ** (n*y))), 2)
            print("The final amount after", y, "years is", FV)

        elif id_old in basic_plan:
            P = basic_plan[id_old][3]
            n = 1
            r = 7
            y = int(input("Enter the amount of years. "))
            FV = round(P * (((1 + ((r/100.0)/n)) ** (n*y))), 2)
            print ("The final amount after", y, "years is", FV)

        elif id_old in advanced_plan:
            P = advanced_plan[id_old][3]
            n = 1
            r = 12
            y = int(input("Enter the amount of years. "))
            FV = round(P * (((1 + ((r/100.0)/n)) ** (n*y))), 2)
            print("The final amount after", y, "years is", FV)

        exit_data = int(input('(1) Exit (2) Return to main menu: '))
        if exit_data == 1:
            break
        elif exit_data == 2:
            continue

    elif user_input == 5:
        while True:
            id_old = int(input('Enter account number: '))
            if id_old not in list_id:
                print('Wrong account number')
                continue
            else:
                break
        delete_data = int(input('Are u sure to delete your account (1) Yes (2) No: '))
        if delete_data == 1:
            if id_old in premium_plan:
                print('Your Information -', premium_plan[id_old])
                list_mobile_no.remove(premium_plan[id_old][1])
                del premium_plan[id_old]
                list_id.remove(id_old)
                print('Account deleted')
                
            elif id_old in basic_plan:
                print('Your Information -', basic_plan[id_old])
                list_mobile_no.remove(basic_plan[id_old][1])
                del basic_plan[id_old]
                list_id.remove(id_old)
                print('Account deleted')
                
            elif id_old in advanced_plan:
                print('Your Information -', advanced_plan[id_old])
                list_mobile_no.remove(advanced_plan[id_old][1])
                del advanced_plan[id_old]
                list_id.remove(id_old)
                print('Account deleted')

        exit_data = int(input('(1) Exit (2) Return to main menu: '))
        if exit_data == 1:
            break
        elif exit_data == 2:
            continue

    else:
        print('Wrong input')

```
