# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Library Management System
```
from enum import Enum
import os

NUMBOOKS = 100

class Library(Enum):
    Newsletters = 0
    Literature = 1
    Physics = 2
    Chemistry = 3
    Biology = 4
    Mathematics = 5
    General_Knowledge = 6
    Politics = 7
    Economics = 8
    Magazines = 9
    Technology = 10

class Book:
    def __init__(self):
        self.title = ""
        self.author = ""
        self.publication = 0
        self.availability = 0
        self.lib = Library.Newsletters

class PersDetail:
    def __init__(self):
        self.name = ""
        self.roll_number = ""
        self.department = ""
        self.contactno = 0
        self.email = ""
        self.borrowed_books = []

def add_book(details, current_index):
    """Function Adds a book to the library.

    Function Arguements:
        details (list): List of Book objects.
        current_index (int): Index to track the current position in the details list.

    Function Returns:
        int: Updated current_index for the next book.
    """
    print("Enter details of the book:")
    details[current_index].title = input("Title: ")
    details[current_index].author = input("Author: ")
    details[current_index].publication = int(input("Publication year: "))
    details[current_index].lib = Library(int(input("Genre (0 for Newsletters, 1 for Literature, etc.): ")))
    details[current_index].availability = 1
    print("Book added successfully!")
    return current_index + 1  # Increment the index for the next book

def display_all_books(details, current_index):
    """Functions Displays the  details of all available books in the library.

    Function Arguements:
        details (list): List of Book objects.
        current_index (int): Index representing the number of books in the library.
    """
    print("List of books:")
    for i in range(current_index):
        if details[i].availability == 1:
            print("Title:", details[i].title)
            print("Author:", details[i].author)
            print("Publication:", details[i].publication)
            print("Library:", details[i].lib)
            print()  # Add a newline for better readability

def fine_calculation():
    """ Function Calculates and displays the fine amount for a borrowed book."""
    days_borrowed = int(input("Enter the number of days the book was borrowed: "))
    if days_borrowed <= 20:
        print("No fine.")
    else:
        fine = days_borrowed - 20
        print("Fine is", fine)

def add_personal_details(perdetails, index):
    """Function Adds a  personal details of a user.

    Function Arguements:
        perdetails (list): List of PersDetail objects.
        index (int): Index to track the current user.
    """
    perdetails[index].name = input("Enter your name: ")
    perdetails[index].roll_number = input("Enter your roll number: ")
    perdetails[index].department = input("Enter your department: ")
    perdetails[index].contactno = int(input("Enter your contact number: "))
    perdetails[index].email = input("Enter your email: ")

def time_info():
    """Get login and logout time from the user.

    Returns:
        tuple: Login and logout time in the format (Hrs:Minutes:Seconds).
    """
    login = input("Enter the login time of your entry (Hrs:Minutes:Seconds): ")
    logout = input("Enter the logout time of your exit (Hrs:Minutes:Seconds): ")
    return login, logout

# Initialization
details = [Book() for _ in range(NUMBOOKS)]
perdetails = [PersDetail() for _ in range(1)]  # Updated to handle multiple users
choice = 0
current_index = 0  # Initialize the index for tracking added books

def view_personal_details(perdetails, user_index):
    """
    Function Arguements:
        perdetails (list): List of PersDetail objects.
        user_index (int): Index representing the current user.
    """
    print("Personal details:")
    print("Name:", perdetails[user_index].name)
    print("Roll Number:", perdetails[user_index].roll_number)
    print("Department:", perdetails[user_index].department)
    print("Contact Number:", perdetails[user_index].contactno)
    print("Email:", perdetails[user_index].email)

def borrow_book(details, user_index):
    """
    Function Arguements:
        details (list): List of Book objects.
        user_index (int): Index representing the current user.
    """
    print("Enter details to borrow a book:")
    title = input("Enter the title of the book: ")

    for book in details:
        if book.title == title and book.availability == 1:
            book.availability = 0  # Mark the status of book as unavailable
            print("Book borrowed successfully!")
            return

    print("Book not available.")

def return_book(details, user_index):
    """
    Function Arguements:
        details (list): List of Book objects.
        user_index (int): Index representing the current user.
    """
    print("Enter details to return a book:")
    title = input("Enter the title of the book: ")

    for book in details:
        if book.title == title and book.availability == 0:
            book.availability = 1  # Mark the book as available
            print("Book returned successfully!")
            return

    print("You haven't borrowed this book.")

# Main program
print("LIBRARY MANAGEMENT SYSTEM")
while choice != 5:
    print("Enter the appropriate choice from the following options:")
    print("1 : Add a book")
    print("2 : Display all books")
    print("3 : Display fine")
    print("4 : Entry details")
    print("5 : Quit")
    print("6 : Borrow a book")
    print("7 : Return a book")
    print("8 : View Personal Details")
    choice = int(input())
    if choice == 1:
        print("Add a book")
        current_index = add_book(details, current_index)
    elif choice == 2:
        print("Display all books")
        display_all_books(details, current_index)
    elif choice == 3:
        print("Fine Calculation")
        fine_calculation()
    elif choice == 4:
        print("Login and Logout time")
        login_time, logout_time = time_info()
        add_personal_details(perdetails, 0)
    elif choice == 5:
        print("Quitting the program")
    elif choice == 6:
        print("Borrow a book")
        borrow_book(details, 0)
    elif choice == 7:
        print("Return a book")
        return_book(details, 0)  # Pass the user index here
    elif choice == 8:
        print("View Personal Details")
        view_personal_details(perdetails, 0)
    else:
        print("Invalid option selected. Choose appropriate options from above.")

    os.system("clear")
```
