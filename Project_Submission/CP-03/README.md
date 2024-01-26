# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)


## Matrix Calculator
```
"""
Amrita Vishwa Vidyapeetham
TIFAC-CORE in Cyber Security
19CSE201 - Advanced Programming Project
Matrix Calculator
Authors: Agil prasanna P, Deepak Kumar S, Anurag Reddy
Created Date: 16-Jan-2024
Updated Date: 21-Jan-2024
"""

#Importing necessary basic libraries
import math
import time

#A class for performing matrix operation such as scalar and matrix multiplication
#This is mainly used in concept with Compile-Time Polymorphism
#Also includes basic read and print matrix methods
class MatrixOperations:
    def __init__(self, rows, columns):
        self.rows = rows
        self.columns = columns
        self.matrix = [[0] * columns for _ in range(rows)]

    def read_matrix(self):
        for i in range(self.rows):
            print(f"\t{self.columns} Entries for row {i + 1}:")
            for j in range(self.columns):
                self.matrix[i][j] = int(input())

    def print_matrix(self, file):
        for i in range(self.rows):
            for j in range(self.columns):
                print(f"\t{self.matrix[i][j]}", end="")
                file.write(f"\t{self.matrix[i][j]}")
            print()
            file.write("\n")
    
    # Overloading the operator '*' as Scalar Mulitplication
    def __mul__(self, scalar, file):
        result = MatrixOperations(self.rows, self.columns)
        file.write("\n\tScalar Matrix: \n")
        for i in range(self.rows):
            for j in range(self.columns):
                result.matrix[i][j] = scalar * self.matrix[i][j]
            print()
            file.write("\n")
        return result
    
    # Overloading the operator '@' as Scalar Mulitplication
    def __matmul__(self, other, file):
        if self.columns != other.rows:
            print("\nMatrix dimensions incompatible for multiplication\n")
            file.write("\nMatrix dimensions incompatible for multiplication\n")
        file.write("\n\tMatrix Multiplication: \n")
        result = MatrixOperations(self.rows, other.columns)
        for i in range(self.rows):
            for j in range(other.columns):
                for k in range(self.columns):
                    result.matrix[i][j] += self.matrix[i][k] * other.matrix[k][j]
        return result
#Multiple functions such as determinant,cofactor,transpose inorder to calculate Inverse of a matrix (Outside the class)
def determinant(arrayone, k):
    s = 1
    det = 0
    arraytwo = [[0] * 10 for _ in range(10)]
    if k == 1:
        return arrayone[0][0]
    else:
        for c in range(k):
            m = 0
            n = 0
            for i in range(k):
                for j in range(k):
                    arraytwo[i][j] = 0
                    if i != 0 and j != c:
                        arraytwo[m][n] = arrayone[i][j]
                        if n < (k - 2):
                            n += 1
                        else:
                            n = 0
                            m += 1
            det = det + s * (arrayone[0][c] * determinant(arraytwo, k - 1))
            s = -1 * s
    return det

def cofactor(num, f, file):
    arraytwo = [[0] * 10 for _ in range(10)]
    fac = [[0] * 10 for _ in range(10)]
    for q in range(f):
        for p in range(f):
            m = 0
            n = 0
            for i in range(f):
                for j in range(f):
                    if i != q and j != p:
                        arraytwo[m][n] = num[i][j]
                        if n < (f - 2):
                            n += 1
                        else:
                            n = 0
                            m += 1
            fac[q][p] = math.pow(-1, q + p) * determinant(arraytwo, f - 1)
    transpose(num, fac, f, file)

def transpose(num, fac, r, file):
    arraytwo = [[0] * 10 for _ in range(10)]
    inverse = [[0] * 10 for _ in range(10)]
    d = determinant(num, r)
    for i in range(r):
        for j in range(r):
            arraytwo[i][j] = fac[j][i]
    for i in range(r):
        for j in range(r):
            inverse[i][j] = arraytwo[i][j] / d
    print("The Inverse of the matrix: ")
    file.write("\nThe Inverse of the matrix: \n")
    for i in range(r):
        for j in range(r):
            print(f"\t{inverse[i][j]}", end="")
            file.write(f"\t{inverse[i][j]}")
        print()
        file.write("\n")
    file.write("\n")
    file.write("----------------------------------------------------------------------------------------------------------------------------------------------------------------------\n")
#Main function for clarity
def main():
    again = 'Y'
# Time Stamp
    currentTime = time.time()
    timeInfo = time.localtime(currentTime)
    buffer = time.strftime("%Y-%m-%d %H:%M:%S", timeInfo)
# To record all the operations performed using the calculator in a "time.txt" file
    file = open("time.txt", "a")
    if file is None:
        print("Unable to open the file.")
        return 1
#Calculator Menu using Color codes for better interactivity
    while again == 'Y':
        print("\n\t\t\t\t\t\tWELCOME TO THE MATRIX CALCULATOR")
        print("\t\t\t\t\t\t~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~")
        print("Operation Menu")
        print("==============")
        print("\t1. Scalar Multiply")
        print("\t2. Multiply two matrices (Matrix Multiplication)")
        print("\t3. Find inverse of a matrix")
        operation = int(input("Enter your choice: "))

        if operation == 1:
            file.write(f"\n{buffer}\n")
            scalar = int(input("Enter the scalar: "))
            file.write(f"The scalar is : {scalar}\n")
            print(f"The scalar is: {scalar}")
            rowA, colA = map(int, input("Enter the #rows and #cols for matrix A: ").split())
            matrixA = MatrixOperations(rowA, colA)
            print(f"\nEnter elements of Matrix A ({rowA} x {colA}) matrix.")
            matrixA.read_matrix()
            print("\n\tMatrix A\n\n")
            file.write("\n\tMatrix A\n\n")
            matrixA.print_matrix(file)
            print(f"\nThe scalar multiplication between matrix A * ({scalar}) is: ")
            result = matrixA.__mul__(scalar, file)
            result.print_matrix(file)
            file.write("\n----------------------------------------------------------------------------------------------------------------------------------------------------------------------\n")
        elif operation == 2:
            rowA, colA = map(int, input("Enter the #rows and #cols for matrix A: ").split())
            print("Enter the #rows and #cols for matrix B: ")
            rowB, colB = map(int, input().split())
            print(f"\nEnter elements of Matrix A ({rowA} x {colA}) matrix.")
            matrixA = MatrixOperations(rowA, colA)
            matrixA.read_matrix()
            file.write(f"\n{buffer}\n")
            print("\n\tMatrix A\n\n")
            file.write("\n\tMatrix A\n\n")
            matrixA.print_matrix(file)
            print(f"\nEnter elements of Matrix B ({rowB} x {colB}) matrix.")
            matrixB = MatrixOperations(rowB, colB)
            matrixB.read_matrix()
            print("\n\tMatrix B\n\n")
            file.write("\n\tMatrix B\n\n")
            matrixB.print_matrix(file)
            result = matrixA.__matmul__(matrixB, file)
            print("Resultant Matrix:")
            file.write("\n\tResultant Matrix\n\n")
            result.print_matrix(file)
            file.write("\n----------------------------------------------------------------------------------------------------------------------------------------------------------------------\n")
        elif operation == 3:
            matrixA = [[0] * 10 for _ in range(10)]
            rowA, colA = map(int, input("Enter the #rows and #cols for matrix A:\n ").split())
            while rowA != colA:
                print("\033[31m")
                print("\n\tThe Input matrix should be 'n x n' matrix.")
                print("\033[0m")
                rowA, colA = map(int, input("Enter the #rows and #cols for matrix A: ").split())
            print(f"\n\tEnter elements of Matrix A a {rowA} x {colA} matrix.")
            for i in range(rowA):
                for j in range(colA):
                    matrixA[i][j] = float(input())
            file.write(f"\n{buffer}\n")
            file.write("\n\tMatrix A\t\n")
            print("\n\tMatrix A\t\n")
            for i in range(rowA):
                for j in range(colA):
                    print(f"\t{matrixA[i][j]}", end="")
                    file.write(f"\t{matrixA[i][j]}")
                print()
                file.write("\n")
            d = determinant(matrixA, rowA)
            if d == 0:
                print("\033[31m")
                print("\n The Determinant of the input matrix is zero (0), therefore inverse does not exist.\n")
                print("\033[0m")
                file.write("\n The Determinant of the input matrix is zero (0), therefore inverse does not exist.\n")
                file.write("\n----------------------------------------------------------------------------------------------------------------------------------------------------------------------\n")
            else:
                cofactor(matrixA, rowA, file)
        else:
            print("\033[31m\nIncorrect option!. Please choose a number 1-3.\033[0m")

        print("\n\nDo you want to calculate again?  ")
        again = input("\033[32mY\033[0m or \033[31mN\033[0m: ").upper()

    print("\033[32m\nThank You for using our matrix calculator. Have a nice day!!\n\033[0m")
    file.close()

if __name__ == "__main__":
    main()

```
