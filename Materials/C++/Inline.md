# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Syntax 
```
inline returnType functionName(parameters) {
    // function body
}
```

## Example 
```
// source1.cpp
#include <iostream>

// Declaration of an inline function
inline int square(int num);

int main() {
    int result = square(5);

    std::cout << "Square in source1: " << result << std::endl;

    return 0;
}

// Definition of the inline function
inline int square(int num) {
    return num * num;
}
```
