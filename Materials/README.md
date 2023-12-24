# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Object Oriented Programming - Comparison with C Program

### C Progarm

car.c
```
#include <stdio.h>
#include <string.h>

// Car structure (like a class in OOP)
struct Car {
    char make[50];
    char model[50];
    int year;
    float price;
};

int main() {
    // Creating an object of the Car structure
    struct Car myCar;
    
    // Setting attributes for the car object
    strcpy(myCar.make, "Jaguar");
    strcpy(myCar.model, "XF");
    myCar.year = 2022;
    myCar.price = 7160000.00;
    
    // Displaying car details
    printf("Car Details:\n");
    printf("Make: %s\n", myCar.make);
    printf("Model: %s\n", myCar.model);
    printf("Year: %d\n", myCar.year);
    printf("Price: $%.2f\n", myCar.price);

    return 0;
}
```

### C++ Program

car.cpp
```
#include <iostream>
#include <string>
using namespace std;

// Car class with attributes and methods
class Car {
public:
    string make;
    string model;
    int year;
    float price;

    // Method to display car details
    void displayDetails() {
        cout << "Car Details:" << endl;
        cout << "Make: " << make << endl;
        cout << "Model: " << model << endl;
        cout << "Year: " << year << endl;
        cout << "Price: $" << price << endl;
    }
};

int main() {
    // Creating an object of the Car class
    Car myCar;
    
    // Setting attributes for the car object
    myCar.make = "Jaguar";
    myCar.model = "XF";
    myCar.year = 2022;
    myCar.price = 7160000.00;

    // Calling the method to display car details
    myCar.displayDetails();

    return 0;
}
```

### Python Program

car.py
```
class Car:
    def __init__(self):
        self.make = ""
        self.model = ""
        self.year = 0
        self.price = 0.0

    # Method to display car details
    def displayDetails(self):
        print("Car Details:")
        print("Make:", self.make)
        print("Model:", self.model)
        print("Year:", self.year)
        print("Price: ${:.2f}".format(self.price))

# Creating an object of the Car class
myCar = Car()

# Setting attributes for the car object
myCar.make = "Jaguar"
myCar.model = "XF"
myCar.year = 2022
myCar.price = 7160000.00

# Calling the method to display car details
myCar.displayDetails()
```
