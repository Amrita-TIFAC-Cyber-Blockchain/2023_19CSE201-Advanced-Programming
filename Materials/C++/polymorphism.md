# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)


## Polymorphism
Polymorphism is the ability where a single function, class, or operator to operate on different types of data. 
There are two main types of polymorphism in C++: 
- Compile-time Polymorphism (also known as static polymorphism)
- Runtime Polymorphism (also known as dynamic polymorphism).

### Compile-Time Polymorphism:
Compile-time polymorphism is achieved through function overloading and operator overloading. 


#### Function Overloading
In function overloading, multiple functions with the same name but different parameter lists are defined.

```
#include <iostream>
using namespace std;

// Class representing math operations
class MathOperations {
public:
    // Function to add two integers
    int add(int a, int b) {
        return a + b;
    }

    // Function to add two doubles
    double add(double a, double b) {
        return a + b;
    }
};

int main() {
    // Create an instance of the MathOperations class
    MathOperations math;

    // Perform addition of integers and display the result
    cout << "Sum of integers: " << math.add(5, 10) << endl;

    // Perform addition of doubles and display the result
    cout << "Sum of doubles: " << math.add(3.5, 2.7) << endl;

    return 0;
}
```

#### Operator Overloading

```
#include <iostream>
using namespace std;

// Class representing a complex number
class Complex {
private:
    // Private members for the real and imaginary parts
    double real;
    double imag;

public:
    // Constructor to initialize the complex number
    Complex(double r, double i) : real(r), imag(i) {}

    // Overloaded + operator to add two complex numbers
    Complex operator+(const Complex& other) const {
        // Create a new Complex object with the sum of real and imaginary parts
        return Complex(real + other.real, imag + other.imag);
    }

    // Display function to print the complex number
    void display() {
        cout << real << "+i " << imag << endl;
    }
};

int main() {
    // Create instances of the Complex class
    Complex c1(2.5, 3.7);
    Complex c2(1.8, 2.2);

    // Use the overloaded + operator to add complex numbers
    Complex result = c1 + c2;

    // Display the result of the addition
    cout << "Result of addition: ";
    result.display();

    return 0;
}
```

### Run-Time Polymorphism
