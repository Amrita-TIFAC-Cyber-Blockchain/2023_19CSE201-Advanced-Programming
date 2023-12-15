# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Polymorphism - Function Overloading
```
#include <iostream>
#include <string>
using namespace std;

class Drawer {
public:
    // Draw function for integers
    void draw(int value) {
        cout << "Drawing integer: " << value << endl;
    }

    // Draw function for doubles
    void draw(double value) {
        cout << "Drawing double: " << value << endl;
    }

};

int main() {
    Drawer drawer;

    // Example usage with different parameter types
    drawer.draw(44);
    drawer.draw(3.14);

    return 0;
}
```

## Templates
```
#include <iostream>
#include <string>
using namespace std;

class Drawer {
public:
    // Template function for draw
    // template <class T> indicates creating template with a placeholder T which refers the typename
    template <typename T>
    void draw(T value) {
        cout << "Drawing: " << value << endl;
    }
};

int main() {
    Drawer drawer;

    // Example usage with different parameter types
    drawer.draw(44);
    drawer.draw(3.14);

    return 0;
}
```
