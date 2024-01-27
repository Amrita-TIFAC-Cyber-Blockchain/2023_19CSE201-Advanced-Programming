# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Password Manager with Strenght Analyser
```
import hashlib, getpass, os, time

class Print():
	'''
	Print class provides formatted console output, including colored text, bold text,
	centered text, and specific formatting options. It is designed to enhance the visual representation
	of information, particularly in the context of a larger program such as a password management system.
	'''
	
	def __init__(self):
		self.colors={"red":31,"yellow":33,"default":0,"underline":4,"white":37,"green":32}
		self.print_inline = lambda text, color: print(f"\33[{self.colors[color]}m{text}\33[0m")

	def red(self, text):
		self.print_inline(text, "red")

	def yellow(self, text):
		self.print_inline(text, "yellow")

	def green(self, text):
		self.print_inline(text, "green")

	def reset(self):
		print("\33[0m")

	def bold(self):
		print("\33[1m")

	def centered(self, text, color):
		if color == "underline":
			t = f"\33[4m{text}\33[0m"
		else:
			t = f"\33[{self.colors[color]}m{text}"
		print('{:^115}'.format(t))

class RenderScreen(Print):
	'''
	RenderScreen class extends the functionality of the Print class,
	offering methods for displaying formatted screens and headers in a password management system.
	It includes functions for presenting information, authentication options, home screen options,
	and headers with specified styles and colors.
	'''

	def clear_screen(self):
		os.system("cls" if os.name == "nt" else "clear")
	
	def InfoScreen(self):
		'''Use the InfoScreen method to display information about the program'''

		self.clear_screen()

		self.bold()
		self.centered("19CSE201 - Advanced Programming","red");
		print()
		self.centered("Welcome to SecurePass ","yellow")
		self.centered("A Place to Store you passwords securely","default")
		self.reset()

		print("\n\n\n");

		self.bold()
		self.centered("Team Members\n","underline");

		print("\t\t\t\t      CB.EN.U4CYS22013 BM Sai Sathvik")
		print("\t\t\t\t      CB.EN.U4CYS22028 JP Hemanth Kumar")    
		print("\t\t\t\t      CB.EN.U4CYS22033 Krishna Moorthi")    
		print("\t\t\t\t      CB.EN.U4CYS22068 Mukesh R")    

		print("\033[?25l")
		time.sleep(2)
		print("\33[?25h")

		self.clear_screen()

	def AuthScreen(self):
		'''
		Use the AuthScreen method to display the authentication options
		'''
	
		self.bold()
		self.centered("SecurePass","yellow")
		self.centered("Password Management System","white")
		self.reset()

		self.centered(" (1) Sign-Up","default")
		self.centered("(2) Log-In","default")

		print('\n\n')

	def HomeScreen(self):
		'''
		Use the HomeScreen method to display the home screen options
		'''
	
		self.clear_screen()

		self.bold()
		self.centered("SecurePass","green")
		self.centered("Password Management System","white")
		self.reset()

		print("\t\t\t\t\t     (1) Store Password")
		print("\t\t\t\t\t     (2) Display Password")
		print("\t\t\t\t\t     (3) Display All Passwords")
		print("\t\t\t\t\t     (4) Exit Application")

	def Header(self,header,color):
		'''
		Use the Header method to display a header with a specified color
		'''
		self.clear_screen()
		self.bold()
		self.centered("SecurePass",f"{color}")
		self.centered("Password Management System","white")
		print()
		self.centered(f"{header}","yellow")
		self.reset()

class PasswordChecker(Print):
	'''
	PasswordChecker class, inheriting from the Print class, is designed for checking the validity of passwords
	based on various criteria such as uppercase, lowercase, digits, special characters, and length.
	It also includes a method to calculate the MD5 hash of a given message.
	'''
	
	def __init__(self):
		self.Issue_Count=1

	def check_upper(self,password):
		if not any(char.isupper() for char in password):
			print(f"\n({self.Issue_Count}) Minimum One \033[0;31mUppercase\033[0m Character")
			self.Issue_Count+=1
		return any(char.isupper() for char in password)

	def check_lower(self,password):
		if not any(char.islower() for char in password):
			print(f"\n({self.Issue_Count}) Minimum One \033[0;31mLowercase\033[0m Character")
			self.Issue_Count+=1            
		return any(char.islower() for char in password)

	def check_digit(self,password):
		if not any(char.isdigit() for char in password):
			print(f"\n({self.Issue_Count}) Minimum One \033[0;31mDigit\033[0m");
			self.Issue_Count+=1

		return any(char.isdigit() for char in password)

	def check_special(self,password):
		if not any(not char.isalnum() or char.isspace() for char in password):
			print(f"\n({self.Issue_Count}) Minimum One \033[0;31mSpecial Character\033[0m")
			self.Issue_Count+=1
		return any(not char.isalnum() or char.isspace() for char in password)

	def check_length(self,password):
		if not len(password) >= 8:
			print(f"\n({self.Issue_Count}) Minimum \033[0;31mLength 8\033[0m")
			self.Issue_Count+=1
		return len(password) >= 8

	def is_password_valid(self,password):
		self.Issue_Count = 1
		bool_upper=self.check_upper(password)
		bool_lower=self.check_lower(password)
		bool_digit=self.check_digit(password)
		bool_special=self.check_special(password)
		bool_length=self.check_length(password)
		return (bool_upper and bool_lower and bool_digit and bool_special and bool_length)
		self.reset()

	def calculate_md5(self,message):
		return hashlib.md5(message.encode()).hexdigest()

class EncryptionUtility:
	'''
	EncryptionUtility class provides basic encryption and decryption functionality.
	It includes methods for encrypting and decrypting text using Vigenere cipher based on ASCII values and a key.
	'''
	
	def decryption(cipher, key):
		decrypted = ""
		for i in range(len(cipher)):
			decrypted += chr((ord(cipher[i]) - ord(key[i % len(key)]) + 128) % 128)
		return decrypted

	def encryption(plain, key):
		cipher = ""
		for i in range(len(plain)):
			cipher += chr((ord(plain[i]) + ord(key[i % len(key)])) % 128)
		return cipher

class PasswordManager(PasswordChecker):
	'''
	PasswordManager class extends PasswordChecker and adds functionality for managing user passwords,
	including adding new passwords and displaying stored passwords based on user choices.
	'''
	
	def __init__(self):
		self.account_type = ""
		self.AccountType = {1 : "Website" , 2 : "App" , 3 : "Mail"} 

	def add_password(self, m_user, m_key):	
		'''
		Method for adding new password
		'''
	
		global account_type_choice
		print("\t\t\t\t\t\t(1) Website")
		print("\t\t\t\t\t\t(2) Application")
		print("\t\t\t\t\t\t(3) E-Mail")
		print("\t\t\t\t\t\t(4) Return back")

		try:
			account_type_choice = int(input("\n>>> Enter Your \33[1m\33[33mChoice\33[0m: "))
		except:
			account_type_choice=-1        
		
		while not 1<=account_type_choice<=4:
			Printer.bold()
			Printer.red("Invalid choice. Try Again")
			Printer.reset()
			try:
				account_type_choice = int(input(">>> Enter Your \33[1m\33[33mChoice\33[0m: "))
			except:
				pass 

		if account_type_choice==4:
			return

		else:
			self.account_type = input(f"\n>>> Enter \33[1m\33[33m{self.AccountType[account_type_choice]}\33[0m : ")

			username = input("\n>>> Enter \33[1m\33[33mUsername\33[0m: ")

			password = getpass.getpass(prompt="\n>>> Enter \33[1m\33[33mPassword\33[0m: ")

			while not self.is_password_valid(password):
				password = getpass.getpass(prompt="\n>>> Enter \33[1m\33[33mPassword\33[0m Again: ")
				
			with open("userinfo.txt", "a") as user_file:
				user_file.write(
					f"{account_type_choice}<$?$>{self.account_type}<$?$>{username}<$?$>{m_key}<$?$>{EncryptionUtility.encryption(password, m_key)}<$?$>{m_user}\n"
				)

	def display(self, username, m_user):
		'''
		Method for displaying stored passwords based on username
		'''
	
		global account_type_choice
		number_of_records=0
		record=[]
	
		try:
			with open("userinfo.txt", "r") as file:
				lines = file.readlines()
		except:
			return False

		for line in lines:
			data = line.split("<$?$>")
			if data[5][:-1] == m_user:
				if data[2] == username:
					number_of_records+=1
					decrypted_password = EncryptionUtility.decryption(data[4], data[3])
					record.append([number_of_records,self.AccountType[int(data[0])],data[1],decrypted_password])

		if len(record)!=0:
			print()
			print("                       ----------------------------------------------------------------------")
			print("                       | S.No |    Account Type    |      Service       |      Password     |")
			print("                       ----------------------------------------------------------------------")        
			for i in range(len(record)):
				print(f"                       | {record[i][0]:<3}  | {record[i][1]:<18} | {record[i][2]:<18} | {record[i][3]:<17} |")
				print("                       ----------------------------------------------------------------------")
			print()
			return True
		else:
			return False

	def display_all(self, m_user):
		'''
		Method for displaying all stored passwords for a user
		'''
		global account_type_choice
		number_of_records=0
		record=[]

		try:
			with open("userinfo.txt", "r") as file:
				lines = file.readlines()
		except:
			return False

		at_pad=18
		s_pad=18

		for line in lines:
			data = line.split("<$?$>")
			if data[5][:-1] == m_user:
				number_of_records+=1
				decrypted_password = EncryptionUtility.decryption(data[4], data[3])
				record.append([number_of_records,data[2],self.AccountType[int(data[0])],data[1],decrypted_password])

		if len(record)!=0:
			print()
			print("          ------------------------------------------------------------------------------------------")
			print("          | S.No |     Username       |    Account Type    |      Service       |      Password     |")
			print("          ------------------------------------------------------------------------------------------")        
			for i in range(len(record)):
				if len(record[i][1])>at_pad and len(record[i][1])<36:
					at_pad=len(record[i][1])
					s_pad=36-at_pad
				print(f"          | {record[i][0]:<3}  | {record[i][1]:<{at_pad}} | {record[i][2]:<{s_pad}} | {record[i][3]:<18} | {record[i][4]:<17} |")
				print("          ------------------------------------------------------------------------------------------")        
			print()
			return True
		else:
			return False

class MasterCredentialsManager:
	'''
	Manages master credentials including checking username existence,
	validating credentials, and loading data from the "master.txt" file.
	'''
	
	def is_master_username_exists(self, username, master_info):
		return any(user_info[0] == username for user_info in master_info)

	def is_master_credentials_exists(self, username, password, master_info):
		for user_info in master_info:
			if user_info[0] == username and user_info[2] == password:
				return user_info[1]
		return None

	def data_from_master(self, master_info):
		try:
			with open("master.txt", "r") as file:
				lines = file.readlines()

			for line in lines:
				data = line.split()
				master_info.append(data)
		except:
			with open("master.txt", "w"):
				pass

if __name__ == "__main__":

	Printer=Print()
	Render_Screen=RenderScreen()
	Password_Check=PasswordChecker()

	Render_Screen.InfoScreen()

	master_info = []
	MCM_instance = MasterCredentialsManager()
	MCM_instance.data_from_master(master_info)

	Render_Screen.AuthScreen()
	
	'''
	Asking user to choose Signup or Login
	'''
	
	try:
		m_choice = int(input(">>> Enter Your \33[1m\33[33mChoice\33[0m: "))
	
	except:
		Printer.bold()
		Printer.red("Invalid choice. Exiting...\n")
		exit()

	if m_choice == 1:
	
		Render_Screen.Header("Sign-Up","yellow")

		m_user = input("\n>>> Enter \33[1m\33[33mMaster Username\33[0m: ")
		while MCM_instance.is_master_username_exists(m_user, master_info):
			Printer.bold()
			Printer.red("Username already exists")
			Printer.reset()
			m_user = input(">>> Enter \33[1m\33[33mMaster Username\33[0m again: ")

		m_key = input("\n>>> Enter \33[1m\33[33mMaster Key\33[0m: ")
		m_password = getpass.getpass(prompt="\nEnter \33[1m\33[33mMaster Password\33[0m: ")

		m_length = len(m_password)

		while not Password_Check.is_password_valid(m_password):
			m_password = getpass.getpass(prompt="\n>>> Enter \33[1m\33[33mMaster Password\33[0m Again: ")
			m_length = len(m_password)

		encrypted_signup_pass = Password_Check.calculate_md5(m_password)

		with open("master.txt", "a") as master_file:
			master_file.write(f"{m_user} {m_key} {encrypted_signup_pass}\n")

	elif m_choice == 2:

		Render_Screen.Header("Log-In","yellow")

		m_user = input("\n>>> Enter \33[1m\33[33mMaster Username\33[0m: ")
		m_password = getpass.getpass(prompt="\n>>> Enter \33[1m\33[33mMaster Password\33[0m: ")

		encrypted_login_pass = Password_Check.calculate_md5(m_password)

		m_key = MCM_instance.is_master_credentials_exists(m_user, encrypted_login_pass, master_info)

		if m_key is not None:
			Printer.bold()
			input(f"\33[1m\33[32mWelcome {m_user}, Press Enter to Continue\33[0m ")
			Printer.reset()
		else:
			Printer.bold()
			Printer.red("Wrong Credentials, Exiting... \n")
			exit()
	else:
		Printer.bold()
		Printer.red("Invalid choice. Exiting...\n")
		exit()

	password_manager = PasswordManager()

	while True:
		Render_Screen.HomeScreen()
		'''
		User enters choice to choose either Store password, Display password, Display all passwords or Exit
		'''
		try:
			choice = int(input("\n>>> Enter Your \33[1m\33[33mChoice\33[0m: "))
		except:
			choice = -1
		
		if choice == 1:
			'''
			New password will be stored in users details
			'''
			Render_Screen.Header("Store Password","green")
			
			password_manager.add_password(m_user, m_key)

		elif choice == 2:
			'''
			This choice is chosen to display passwords with a specific username
			'''
			Render_Screen.Header("Display Passwords","green")

			username_to_display = input(">>> Enter \33[1m\33[33mUsername\33[0m to be displayed: ")
			if not password_manager.display(username_to_display, m_user):
				Printer.bold()
				Printer.red("No such Records found")
				Printer.reset()
			input(">>> Press \33[1m\33[33mEnter\33[0m to continue ")

		
		elif choice == 3:
			'''
			This choice is chosen to diplay all the password under the user
			'''
			Render_Screen.Header("Display All Passwords","green")

			if not password_manager.display_all(m_user):
				Printer.bold()
				Printer.red("No such Records found")
				Printer.reset()
			input(">>> Press \33[1m\33[33mEnter\33[0m to continue ")


		elif choice == 4:
			'''
			This choice is chosen to Exit the Password Management System
			'''
			Printer.bold()
			Printer.red("Exiting...\n")
			exit()

		else:
			Printer.bold()
			Printer.red("Invalid choice. Try Again")
			Printer.reset()
			input(">>> Press \33[1m\33[33mEnter\33[0m to continue ")
```
