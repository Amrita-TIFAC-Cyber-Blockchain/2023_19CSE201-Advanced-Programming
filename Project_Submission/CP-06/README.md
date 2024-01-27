# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Student Management System
```
import os
import re
import hashlib
import getpass
from colorama import init, Fore, Style

init(autoreset=True)

YELLOW = Fore.YELLOW
RESET = Style.RESET_ALL

class User:
    def __init__(self, username):
        """
        Initialize a new instance of the User class with the provided username.
        """
        self.username = username
        self.filename = f"{username}.txt"

    def file_exists(self):
        """
        Check if a file with the given filename exists.

        Returns, True if the file exists, False otherwise.
        """
        return os.path.isfile(self.filename)

    def hash_password(self, password):
        """
        Hash a password using the SHA-256 algorithm.

        Returns, The hashed password.
        """
        hashed_password = hashlib.sha256(password.encode()).hexdigest()
        return hashed_password

    def is_valid_email(self, email):
        """
        Check if the given email is in a valid format.

        Returns, True if the email is valid, False otherwise.
        """
        email_pattern = re.compile(r"[^@]+@[^@]+\.[^@]+")
        return bool(re.match(email_pattern, email))

    def authenticate_user(self, password):
        """
        Authenticate a user by checking the provided username and password.

        Returns, True if authentication is successful, False otherwise.
        """
        if not self.file_exists():
            print("No such username.")
            return False

        with open(self.filename, "r") as file:
            stored_hashed_password = file.readline().strip()

        hashed_password_to_check = self.hash_password(password)

        if hashed_password_to_check == stored_hashed_password:
            return True
        else:
            print("Incorrect password.")
            return False

    def display_user_details(self):
        """
        Display details of a user based on the provided username.
        """
        if not self.file_exists():
            print("No such username.")
            return

        with open(self.filename, "r") as file:
            password = file.readline().strip()
            first_name, last_name = file.readline().split()
            email = file.readline().strip()
            phone_number = file.readline().strip()
            subject_marks = list(map(float, file.readline().split()))
            attendance_percentage = float(file.readline().strip())

        cgpa = sum(subject_marks) / 60.0

        print("\nUsername:", self.username)
        print("Name:", first_name, last_name)
        print("Email:", email)
        print("Phone Number:", phone_number)
        print("Subject Marks:", " ".join([f"{mark:.2f}" for mark in subject_marks]))
        print(f"Attendance Percentage: {attendance_percentage:.2f}%")
        print(f"CGPA: {cgpa:.2f}")

    def edit_profile(self):
        """
        Allow a user to edit their profile details.
        """
        if not self.file_exists():
            print("No such username.")
            return

        with open(self.filename, "r") as file:
            user_data = file.readlines()

        while True:
            print("\nChoose the field to edit:")
            print("1. First Name")
            print("2. Last Name")
            print("3. Email")
            print("4. Phone Number")
            print("5. Marks")
            print("6. Attendance Percentage")
            print("7. Finish Editing")

            try:
                option = int(input("Enter your option: "))

                if option == 1:
                    user_data[1] = f"{input('Enter new First Name: ')} {user_data[1].split()[1]}\n"
                elif option == 2:
                    user_data[1] = f"{user_data[1].split()[0]} {input('Enter new Last Name: ')}\n"
                elif option == 3:
                    user_data[2] = f"{input('Enter new Email: ')}\n"
                elif option == 4:
                    user_data[3] = f"{input('Enter new Phone Number: ')}\n"
                elif option == 5:
                    print("Enter new marks:")
                    for i in range(6):
                        try:
                            subject_mark = float(input(f"Subject {i + 1} - "))
                            user_data[4] = " ".join([f"{subject_mark:.2f}" for subject_mark in [subject_mark]])
                        except ValueError:
                            print("Invalid input. Please enter a numeric value for marks.")
                elif option == 6:
                    user_data[5] = f"{input('Enter new Attendance Percentage: ')}\n"
                elif option == 7:
                    break
                else:
                    print("Invalid option. Please choose again.")

            except ValueError:
                print("Invalid input. Please enter a number.")

        with open(self.filename, "w") as file:
            file.writelines(user_data)

        print("Profile updated successfully.")

class Student(User):
    def __init__(self, username):
        """
        Call the constructor of the base class (User) using super().
        """
        super().__init__(username)

    def signup(self):
        """
        Allow a user to sign up by providing necessary details and storing them in a file.
        """
        if self.file_exists():
            print("Username already exists.")
            return

        password = getpass.getpass("Enter password: ")
        hashed_password = self.hash_password(password)

        with open(self.filename, "w") as file:
            file.write(hashed_password + "\n")

            # Additional user details
            first_name = input("Enter first name: ")
            last_name = input("Enter last name: ")

            # Email validation loop
            while True:
                email = input("Enter email: ")
                if self.is_valid_email(email):
                    break
                else:
                    print("Invalid email format. Please enter a valid email.")

            # Phone number validation loop
            while True:
                phone_number = input("Enter phone number: ")
                if len(phone_number) == 10 and phone_number.isdigit():
                    break
                else:
                    print("Invalid phone number. Please enter a 10-digit numeric phone number.")

            print("Enter marks:")
            subject_marks = []
            subjects = ["Subject 1", "Subject 2", "Subject 3", "Subject 4", "Subject 5", "Subject 6"]
            for i in range(6):
                try:
                    mark = float(input(f"{subjects[i]} - "))
                    subject_marks.append(mark)
                except ValueError:
                    print("Invalid input. Please enter a numeric value for marks.")

            attendance_percentage = float(input("Enter attendance percentage: "))

            # Write user details to file
            file.write(f"{first_name} {last_name}\n")
            file.write(f"{email}\n")
            file.write(f"{phone_number}\n")
            file.write(" ".join([f"{mark:.2f}" for mark in subject_marks]) + "\n")
            file.write(f"{attendance_percentage:.2f}\n")

        print("\nSignup successful.")

class Menu:
    def display_main_menu():
        """
        Display the main menu options for the student portal.
        """
        print("\n1. Login")
        print("2. Signup")
        print("3. Exit")

    def display_welcome_message():
        """
        Display a welcome message for the student portal.
        """
        print(YELLOW + "                                      Welcome to the student portal" + RESET)

def main():
    os.system("clear")
    username = ""
    loggedIn = 0
    Menu.display_welcome_message()

    while True:
        Menu.display_main_menu()

        try:
            choice = int(input("\nEnter your choice: "))

            if choice == 1:  # Login
                if loggedIn:
                    print("You are already logged in.")
                    continue

                username = input("Enter username: ")
                password = input("Enter password: ")

                student = Student(username)
                if student.authenticate_user(password):
                    print("Login successful.")
                    loggedIn = 1

            elif choice == 2:  # Signup
                if loggedIn:
                    print("You are already logged in.")
                    continue

                student = Student(input("Enter username: "))
                student.signup()

            elif choice == 3:  # Exit
                print("Exiting the program.")
                break

            else:
                print("Invalid choice.")

            if loggedIn:
                while True:
                    student.display_user_details()

                    print("\n1. Logout")
                    print("2. Edit Profile")

                    option = input("Enter your option: ")

                    if option == '1':  # Logout
                        loggedIn = 0
                        break

                    elif option == '2':  # Edit Profile
                        student.edit_profile()

                    else:
                        print("Invalid option.")

        except ValueError:
            print("Invalid input. Please enter a number.")

if __name__ == "__main__":
    main()
```
