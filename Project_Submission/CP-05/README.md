# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

### Encryption and Decryption Calculator
```
'''
Amrita Vishwa Vidyapeetham
TIFAC-CORE in Cyber Security
19CSE201 - Advanced Programming Project
Encryption and Decryption Calculator
Authors: Amal Ritessh A P, Ananth R, Mukund K, Shravan Krishnan.G
Created Date: 13-January-2023
Updated Date: none  
'''

class display:
    """this class is used to display the output,
    it is the parent class.

    this function also uses file to store its content in info.txt
    """
    def display(self,ans,mode,mode_operation):
        print(mode,"Cipher:")
        print(mode_operation +": "+ans)
        with open('info.txt', 'a') as file:
            file.write(mode+" Cipher:\n")
            file.write(mode_operation +": "+ans+"\n")


class shift_cipher(display):
    """
    this is an derived class from display class (parent class).
    it is used to calculate the encryption and decrption of an text.

    it uses shift cipher algorythim.
    """
    def encryption(self,val,key):
        val=list(val)
        ans=""
        for i in range(len(val)):
            ans=ans+chr(ord(val[i])+key)
        self.display(ans,"Shift","Encryption")
        
    def decryption(self,val,key):
        val=list(val)
        ans=""
        for i in range(len(val)):
            ans=ans+chr(ord(val[i])-key)
        self.display(ans,"Shift","Decryption")

class vigenere_cipher(display):
    """
    this is an derived class from display class (parent class).
    it is used to calculate the encryption and decrption of an text.

    it uses vigenere cipher algorythim.
    """
    def encryption(self,val,key):
        k_len=len(key)
        ans=""
        for i in range(len(val)):
            ans=ans+chr(ord(val[i])+ord(key[i%k_len]))
        self.display(ans,"Vigenere","Encryption")
        
    def decryption(self,val,key):
        k_len=len(key)
        ans=""
        for i in range(len(val)):
            ans=ans+chr(ord(val[i])-ord(key[i%k_len]))
        self.display(ans,"Vigenere","Decryption")

while(True):
    """this is the main function.

    an menu driven option to access the above classes.
    """
    print("1 - Shift Cipher \n2 - Vigenere Cipher \n3 - exit \n")
    mode=int(input())
    
    if mode==1:
        s = shift_cipher()
        while(True):
            print("1 - Encryption \n2 - Decryption")
            mode_operation=int(input())
            if mode_operation==1:
                  print("Plain text: ",end="")
                  plaintext=input()
                  print("Key: ",end="")
                  key=int(input())
                  s.encryption(plaintext,key)
                  break
            elif mode_operation==2:
                print("Cipher text: ",end="")
                ciphertext=input()
                print("Key: ",end="")
                key=int(input())
                s.decryption(ciphertext,key)
                break
         
    elif mode==2:
        v = vigenere_cipher()
        while(True):
            print("1 - Encryption \n2 - Decryption")
            mode_operation=int(input())
            if mode_operation==1:
                  print("Plain text: ",end="")
                  plaintext=input()
                  print("Key: ",end="")
                  key=input()
                  v.encryption(plaintext,key)
                  break
            elif mode_operation==2:
                print("Cipher text: ",end="")
                ciphertext=input()
                print("Key: ",end="")
                key=input()
                v.decryption(ciphertext,key)
                break
            
    elif mode==3:
        print("Exit")
        break

    else:
        print("Invalid Input")
```
