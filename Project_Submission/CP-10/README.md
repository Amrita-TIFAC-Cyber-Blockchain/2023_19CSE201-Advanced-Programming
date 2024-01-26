# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Course Objective Attainment
```
MAX_STUDENTS = 5
MAX_SUBJECTS = 50
MAX_LENGTH = 50
MAX_USERS = 10
MAX_USERNAME = 50
MAX_PASSWORD = 50

class Exam:
    '''
    Exam class acts as enum function which assigns numbers for different types of exams.
    '''
    Mid = 0
    End = 1

def check_users_file_exists():
    '''
    It checks if users.txt is already present during the start of the program.
    '''
    try:
        with open("users.txt", "r"):
            pass
        return True
    except FileNotFoundError:
        return False

class User:
    '''
    User class acts as a base class to admin class and contains functions that perform authentication and displaying menu.
    '''
    def __init__(self, username, password):
        self.username = username
        self.password = password

    def authenticate(self, input_username, input_password):
        '''
        It takes username and password that are given as input from the user and checks them with the credentials stored in users.txt.
        It permits only when the given credentials match with users.txt and disallows everyone else.
        '''
        try:
            with open("users.txt", "r") as user_file:
                user_credentials = [line.strip() for line in user_file.readlines()]

            for user_credential in user_credentials:
                stored_username, stored_password = user_credential.split(":")
                if stored_username == input_username and stored_password == input_password:
                    return stored_username,stored_password

            print("Error: Invalid username or password.")
            return False
        except FileNotFoundError:
            print("Error: Failed to open users file.")
            return False

    def display_menu(self):
        '''
        It displays menu which acts as a guide for users to access various features of the program.
        '''
        print("Menu:")
        print("1. Add User")
        print("2. Add Subject")
        print("3. Display Marks for a Specific Student")
        print("4. Display Marks for the Whole Class")
        print("5. Exit")

class Admin(User):
    '''
    Admin class inherits the properties of User class which contains all the functions that can be performed by the Admin.
    It includes methods to create a default admin user, adding another user, displaying marks of a specific student and of the whole class.
    '''
    def create_default_admin(self):
    '''
    It creates a file users.txt and writes the credentials for admin.
    '''
        try:
            with open('users.txt', 'w') as file:
                file.write('admin:123\n')
            print("Default user created successfully")
        except Exception as e:
            print(f"Error: {e}")

    def add_user(self):
        '''
        This method can be accessed only by admin which adds a new user data.
        It also checks if the given user credentials are already present or not.
        It writes the credentials of the new user in users.txt file.
        '''
        username = input("Enter the new username: ")
        password = input("Enter the new password: ")

        try:
            with open("users.txt", "r") as user_file:
                existing_users = [line.split(":")[0] for line in user_file.readlines()]

            if username in existing_users:
                print("Error: Username already exists. Please choose a different username.")
                return 0

            with open("users.txt", "a") as user_file:
                user_file.write(f"{username}:{password}\n")

            print("User added successfully!")
            return 1
        except FileNotFoundError:
            print("Error: Failed to open users file.")
            return 0

    def add_student_marks(self, subject, exam):
        '''
        This method can be accessed only by admin which adds marks of students for a subject.
        It adds the subject in a separate file and proceeds only if it is a new subject.
        It creates a reference for n exam file which stores the blueprint of the question paper 
        Reference file contains no.of questions, COs, and marks for each question.
        It also creates a marks file that stores the marks of all the students in a particular subject.
        '''
        filename = subject

        if exam == Exam.Mid:
            filename += "_MID"
        elif exam == Exam.End:
            filename += "_END"

        try:
            with open("SUBJECTS.txt", "a+") as file:
                existing_subjects = [line.strip() for line in file.readlines()]
                if filename in existing_subjects:
                    print("This subject already exists in the record!!!!")
                    return 4
                else:
                    file.write(f"{filename}\n")

            m = []
            file1name = f"{filename}_REFERENCE.txt"
            with open(file1name, "w") as file:
                num_questions = int(input("Enter the number of questions in this paper: "))
                num_codes = int(input("Enter the number of Course objectives in this paper: "))
                file.write(f"{num_questions}\n{num_codes}\n")

                for i in range(num_questions):
                    code = int(input(f"Enter code (only number) for question {i + 1}: "))
                    while not (0 < code and code <= num_codes):
                        print("Enter a valid CO code!!")
                        code = int(input(f"Enter code (only number) for question {i + 1}: "))        
                    marks = float(input(f"Enter marks for question {i + 1}: "))
                    m.append(marks)
                    file.write(f"{i + 1}\n{marks}\n{code}\n")

            filename += "_MARKS.txt"
            with open(filename, "w") as file:
                for i in range(1, MAX_STUDENTS + 1):
                    file.write(f"{i}\n")
                    for j in range(num_questions):
                        while True:
                            input_marks = float(input(f"Enter the marks of question {j + 1} for roll number {i}: "))
                            if input_marks > m[j]:
                                print("Enter valid marks within the maximum marks for the question!")
                                continue
                            else:
                                break        
                        file.write(f"{input_marks}\n")

            return 0
        except IOError:
            print("Failed to open subjects or reference file.")
            return 1

    def display_student_marks(self):
        '''
        This method can be accessed by any user that displays marks of a specific student in a subject.
        It uses reference file and marks file to display the marks attained by each student and also the marks
        attained by each student in various course objectives too.
        '''
        roll_num = int(input("Enter the roll number of the student: "))
        file2name = "SUBJECTS.txt"

        try:
            with open(file2name, "r") as file:
                subjects = [line.strip() for line in file.readlines()]

            for i, subject in enumerate(subjects, start=1):
                print(f"{i}. {subject}")

            option = int(input(f"Select one option from 1 to {len(subjects)}: "))
            filen = subjects[option - 1]
            filename = f"{filen}_REFERENCE.txt"

            with open(filename, "r") as ref_file:
                total_questions = int(ref_file.readline().strip())
                total_course_obj = int(ref_file.readline().strip())

                marks_by_course_obj = [0] * total_course_obj
                course_obj_by_question = []

                for i in range(total_questions):
                    question_no = int(ref_file.readline().strip())
                    marks = float(ref_file.readline().strip())
                    course_obj = int(ref_file.readline().strip())
                    course_obj_by_question.append(course_obj)

                    if 1 <= course_obj <= total_course_obj:
                        marks_by_course_obj[course_obj - 1] += marks

            file1name = f"{filen}_MARKS.txt"
            with open(file1name, "r") as marks_file:
                marks_by_question = [0] * total_questions

                for _ in range(MAX_STUDENTS):
                    temp2 = int(marks_file.readline().strip())

                    for j in range(total_questions):
                        temp = float(marks_file.readline().strip())
                        if temp2 == roll_num:
                            marks_by_question[j] += temp

            print("\nMarks by question:")
            for i in range(total_questions):
                print(f"Marks obtained for question {i + 1} is: {marks_by_question[i]}")

            print("\nMarks by course objectives:")
            marks_by_course_objective = [0] * total_course_obj
            for i in range(total_questions):
                a = course_obj_by_question[i]
                marks_by_course_objective[a - 1] += marks_by_question[i]

            for i in range(total_course_obj):
                print(f"Marks obtained in CO{i + 1} is {marks_by_course_objective[i]:.3f}/{marks_by_course_obj[i]:.3f}")

            print("\n")
            return 0
        except FileNotFoundError:
            print(f"Error: Could not open file {filename}")
            return 2

    def display_class_marks(self):
        '''
        This method can be accessed by any user which is used to display the marks of whole class in a subject.
        It displays average marks by questions and also course objectives of the whole class.
        '''
        file2name = "SUBJECTS.txt"

        try:
            with open(file2name, "r") as file:
                subjects = [line.strip() for line in file.readlines()]

            for i, subject in enumerate(subjects, start=1):
                print(f"{i}. {subject}")

            option = int(input(f"Select one option from 1 to {len(subjects)}: "))
            filen = subjects[option - 1]
            filename = f"{filen}_REFERENCE.txt"

            with open(filename, "r") as ref_file:
                total_questions = int(ref_file.readline().strip())
                total_course_obj = int(ref_file.readline().strip())

                marks_by_course_obj = [0] * total_course_obj
                course_obj_by_question = []

                for i in range(total_questions):
                    question_no = int(ref_file.readline().strip())
                    marks = float(ref_file.readline().strip())
                    course_obj = int(ref_file.readline().strip())
                    course_obj_by_question.append(course_obj)

                    if 1 <= course_obj <= total_course_obj:
                        marks_by_course_obj[course_obj - 1] += marks

            file1name = f"{filen}_MARKS.txt"
            with open(file1name, "r") as marks_file:
                average_marks_by_question = [0] * total_questions

                for _ in range(MAX_STUDENTS):
                    temp2 = int(marks_file.readline().strip())

                    for j in range(total_questions):
                        temp = float(marks_file.readline().strip())
                        average_marks_by_question[j] += temp

                for i in range(total_questions):
                    average_marks_by_question[i] /= MAX_STUDENTS

            print("\nAverage marks by question:")
            for i in range(total_questions):
                print(f"Average marks for question {i + 1} is: {average_marks_by_question[i]}")

            print("\nAverage marks by course objectives:")
            average_marks_by_course_objective = [0] * total_course_obj
            for i in range(total_questions):
                a = course_obj_by_question[i]
                average_marks_by_course_objective[a - 1] += average_marks_by_question[i]

            for i in range(total_course_obj):
                print(f"Average marks for CO{i + 1} is {average_marks_by_course_objective[i]:.3f}/{marks_by_course_obj[i]:.3f}")

            print("\n")
            return 0
        except FileNotFoundError:
            print(f"Error: Could not open file {filename}")
            return 2

def main():
    if not check_users_file_exists():
        ''' 
        Checking if users.txt is present or not. If not present, then it creates one.
        '''
        print("The 'users.txt' file does not exist. Creating one...")
        admin = Admin("admin", "123")
        admin.create_default_admin()
    else:
        admin = Admin("admin", "123")

    '''
    Authentication feature.
    '''
    username_input = input("Enter your username: ")
    password_input = input("Enter your password: ")
    
    authenticated = []
    authenticated = admin.authenticate(username_input, password_input)

    if authenticated[0] == False:
        print("Authentication failed. Exiting...")
        return

    while True:
        '''
        Display menu options.
        '''
        admin.display_menu()
        choice = int(input("Enter your choice: "))

        if choice == 1:
            '''
            Add user option only accessed by admin.
            '''
            if authenticated[0] == "admin":
                admin.add_user()
            else:
                print("Only Admins can add uers!")
                break
        elif choice == 2:
            '''
            Add subject option only accessed by admin.
            '''
            if authenticated[0] == "admin":
                subject = input("Enter the subject: ")
                exam_type = input("Enter the exam type (Mid (or) End): ").upper()
                if exam_type == "MID":
                    exam = Exam.Mid
                elif exam_type == "END":
                    exam = Exam.End
                else:
                    print("Invalid exam type. Please try again.")
                    continue
            else:
                print("Only Admins can add subject!!")
                break

            admin.add_student_marks(subject, exam)
        elif choice == 3:
            '''
            Displaying the marks of a specific student in a subject.
            '''
            admin.display_student_marks()
        elif choice == 4:
            '''
            Displaying the marks of whole class in a subject.
            '''
            admin.display_class_marks()
        elif choice == 5:
            '''
            Exiting the program.
            '''
            print("Exiting...")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
```
