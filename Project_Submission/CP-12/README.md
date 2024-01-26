# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Encryption and Decryption Calculator
```
# Amrita Vishwa Vidyapeetham
# TIFAC-CORE in Cyber Security
# 19CSE201 - Advanced Programing Project
# Encryption Decryption calculator
# Authors: Chitla Vyshali, C N Hansica, M Sanju,T Sree Chandan
# Created Date: 10-January-2024
# Updated Date: 24-January-2024 

# Welcome to the Encryption and Decryption Calculator!
# This interactive program allows you to explore various classical ciphers for encrypting and decrypting messages.
# Choose from the following ciphers:
#   1 - Caesar Cipher
#   2 - Substitution Cipher
#   3 - Permutation Cipher
#   4 - Affine Cipher
# You can perform encryption or decryption based on your selection.


# Define a base class Cipher with common methods for encryption and decryption
class Cipher:
    def _init_(self, name):
        self.name = name

    def encrypt(self, message):
        pass

    def decrypt(self, message):
        pass


# Implement a CaesarCipher class inheriting from Cipher
class CaesarCipher(Cipher):
    def _init_(self):
        super()._init_("Caesar Cipher")

    def encrypt(self, message):
        # Get key from user input, encrypt message, and print result
        try:
            key = int(input("Enter the key for Caesar Cipher: "))
        except ValueError:
            raise ValueError("Key must be an integer for Caesar Cipher.")
        
        ct = ""
        for i in range(len(message)):
            c = message[i]
            if c.isalpha():
                if c.isupper():
                    k = chr((ord(c) - ord('A') + key) % 26 + ord('A'))
                else:
                    k = chr((ord(c) - ord('a') + key) % 26 + ord('a'))
                ct += k
            else:
                ct += c
        print(ct)
    
    def decrypt(self, message):
        # Get key from user input, decrypt message, and print result
        try:
            key = int(input("Enter the key for Caesar Cipher: "))
        except ValueError:
            raise ValueError("Key must be an integer for Caesar Cipher.")
        
        print("Decrypted message:", end=" ")
        for i in range(len(message)):
            c = message[i]
            if c.isalpha():
                if c.isupper():
                    k = chr((ord(c) - ord('A') - key + 26) % 26 + ord('A'))
                else:
                    k = chr((ord(c) - ord('a') - key + 26) % 26 + ord('a'))
                print(k, end="")
            else:
                print(c, end="")
        print()


# Implement a SubstitutionCipher class inheriting from Cipher
class SubstitutionCipher(Cipher):
    def _init_(self):
        super()._init_("Substitution Cipher")
        self.key = "QWERTYUIOPASDFGHJKLZXCVBNM"

    def encrypt(self, message):
        # Encrypt message using a substitution key and print result
        ct = ""
        for char in message:
            if char.isalpha():
                if char.isupper():
                    index = ord(char) - ord('A')
                    ct += self.key[index]
                else:
                    index = ord(char) - ord('a')
                    ct += self.key[index].lower()
            else:
                ct += char
        print(ct)

    def decrypt(self, message):
        # Decrypt message using the inverse of the substitution key and print result
        dt = ""
        for char in message:
            if char.isalpha():
                if char.isupper():
                    index = self.key.find(char)
                    dt += chr(ord('A') + index)
                else:
                    index = self.key.find(char.upper())
                    dt += chr(ord('a') + index)
            else:
                dt += char
        print(dt)


# Implement a PermutationCipher class inheriting from Cipher
class PermutationCipher(Cipher):
    def _init_(self):
        super()._init_("Permutation Cipher")

    def encrypt(self, message):
        # Encrypt message using a permutation key and print result
        n = int(input("Permutation cipher encrypts and decrypts text based on the length of plain text.\nPlease enter a key of length which is a factor of the text length.\nEnter the length of the key: "))
        key_input = input("Enter the key for Encryption:\n")
        key = [int(k) for k in key_input.split()]

        for i in range(n):
            key[i] = key[i] - 1

        x = len(message)
        if x % n != 0:
            print("Error: The length of the plain text is not divisible by the key length.")
        else:
            y = 0
            ciphertext = ""
            for i in range(x):
                j = i % n
                j = key[j]
                j = j + (y * n)
                ciphertext += message[j]
                if i % n == n - 1:
                    y += 1
            print("Encrypted text:", ciphertext)

    def decrypt(self, message):
        # Decrypt message using the inverse of the permutation key and print result
        n = int(input("Permutation cipher encrypts and decrypts text based on the length of plain text.\nPlease enter a key of length which is a factor of the text length.\nEnter the length of the key: "))
        key_input = input("Enter the key for Decryption:\n")
        key = [int(k) for k in key_input.split()]

        for i in range(n):
            key[i] = key[i] - 1

        x = len(message)
        count = 0  # Initialize count

        if x % n != 0:
            print("Error: The length of the cipher text is not divisible by the key length.")
        else:
            y = 0
            plaintext = [""] * x  # Use a list to store characters and join later
            for i in range(x):
                j = i % n
                j = key[j]
                j = j + (n * y)
                plaintext[j] = message[i]
                if i % n == n - 1:
                    y += 1
            decrypted_text = "".join(plaintext)
            print("Decrypted text:", decrypted_text)


# Implement an AffineCipher class inheriting from Cipher
class AffineCipher(Cipher):
    def _init_(self):
        super()._init_("Affine Cipher")
        self.key_a = 0
        self.key_b = 0

    def modInverse(self, a, m):
        # Calculate the modular multiplicative inverse
        a = a % m
        for x in range(1, m):
            if (a * x) % m == 1:
                return x
        return -1

    def set_keys(self, a, b):
        # Set keys 'a' and 'b' for the Affine Cipher
        m = 26
        a_inverse = self.modInverse(a, m)
        if a_inverse == -1:
            print("Cipher text cannot be decrypted since 'a' does not have an inverse modulo 26.")
        else:
            self.key_a = a
            self.key_b = b

    def are_coprime(self, a, b):
        # Check if two numbers are coprime
        while b != 0:
            a, b = b, a % b
        return a == 1

    def encrypt(self, message):
        # Encrypt message using the Affine Cipher and print result
        if self.key_a == 0 or self.key_b == 0:
            raise ValueError("Keys 'a' and 'b' must be set before encryption.")
        encrypted_text = ""
        for char in message:
            if char.isalpha():
                if char.isupper():
                    encrypted_text += chr(((self.key_a * (ord(char) - ord('A')) + self.key_b) % 26) + ord('A'))
                else:
                    encrypted_text += chr(((self.key_a * (ord(char) - ord('a')) + self.key_b) % 26) + ord('a'))
            else:
                encrypted_text += char
        print(encrypted_text)

    def decrypt(self, message):
        # Decrypt message using the Affine Cipher and print result
        if self.key_a == 0 or self.key_b == 0:
            raise ValueError("Keys 'a' and 'b' must be set before decryption.")
        
        decrypted_text = ""
        mod_inverse = self.modInverse(self.key_a, 26)
        
        if mod_inverse == -1:
            print("Error: 'a' does not have an inverse modulo 26.")
        else:
            for char in message:
                if char.isalpha():
                    if char.isupper():
                        decrypted_text += chr((mod_inverse * (ord(char) - self.key_b - ord('A'))) % 26 + ord('A'))
                    else:
                        decrypted_text += chr((mod_inverse * (ord(char) - self.key_b - ord('a'))) % 26 + ord('a'))
                else:
                    decrypted_text += char
            print(decrypted_text)


# Main function

# Dictionary containing cipher options
ciphers = {
    1: CaesarCipher(),
    2: SubstitutionCipher(),
    3: PermutationCipher(),
    4: AffineCipher(),
}

# Welcome message
print("Welcome to the encryption and decryption calculator.")
print("Please select one of the following ciphers:")

while True:
    # Display menu options
    print("1 - Caesar Cipher")
    print("2 - Substitution Cipher")
    print("3 - Permutation Cipher")
    print("4 - Affine Cipher")
    print("5 - Exit the Program")

    # Get user option
    option = int(input())

    # Check if user wants to exit
    if option == 5:
        print("Thank you for using our Encryption and Decryption Calculator.")
        break

    # Get selected cipher from the dictionary
    cipher = ciphers.get(option)

    # Process the selected cipher
    if cipher:
        print("1 - For Encryption")
        print("2 - For Decryption")
        choice = int(input())

        # Check if the user selected a valid mode
        if choice in [1, 2]:
            try:
                # Set keys for Affine Cipher if selected
                if option == 4 and choice == 1:
                    a = int(input("Enter the value of 'a' for Affine Cipher: "))
                    b = int(input("Enter the value of 'b' for Affine Cipher: "))
                    cipher.set_keys(a, b)

                # Get user input for the message
                message = input("Enter the message: ")

                # Perform encryption or decryption based on the mode
                if choice == 1:
                    cipher.encrypt(message)
                elif choice == 2:
                    cipher.decrypt(message)

            except ValueError as e:
                print(f"Error: {str(e)}")
                
        else:
            print("Invalid mode selection.")

    else:
        print("Please select a valid option.")
```
