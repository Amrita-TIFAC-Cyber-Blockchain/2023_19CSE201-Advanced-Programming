# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Syntax
```
class ClassName {
public:
    // Return_Type operatorOperatorSymbol(parameters) {
    //     // implementation
    // }

    // Example: Overloading the <<operator>>
    ClassName operator<<operator>>(const ClassName& other) {
        // implementation
    }
};

```

## Example 1 - Binary Operator
```
#include <iostream>

class Complex {
public:
    double real;
    double imaginary;


    Complex(double r, double i) : real(r), imaginary(i) {}

    Complex operator+(const Complex& other) const {
        return Complex(real + other.real, imaginary + other.imaginary);
    }
};

int main() {
    Complex c1(2.0, 3.5);
    Complex c2(1.5, 4.0);

    Complex result = c1 + c2;

    std::cout << "Result: " << result.real << " + " << result.imaginary << "i" << std::endl;

    return 0;
}
```

## Example 2 - Unary Operator
```
#include <iostream>

class Complex {
public:
    double real;
    double imaginary;


    Complex(double r, double i) : real(r), imaginary(i) {}

    Complex operator-() const {
        return Complex(-real, -imaginary);
    }
};

int main() {
    Complex c1(2.0, 3.5);

    Complex result = -c1;

    std::cout << "Result: " << result.real << " + " << result.imaginary << "i" << std::endl;

    return 0;
}
```

## Example 3 - Comparison Operator
```
#include <iostream>

class Complex {
public:
    double real;
    double imaginary;


    Complex(double r, double i) : real(r), imaginary(i) {}

    bool operator==(const Complex& other) const {
        return (real == other.real) && (imaginary == other.imaginary);
    }

};

int main() {
    Complex c1(2.0, 3.5);
    Complex c2(2.0, 3.5);

    if (c1 == c2) {
        std::cout << "The complex numbers are equal." << std::endl;
    } else {
        std::cout << "The complex numbers are not equal." << std::endl;
    }

    return 0;
}
```
