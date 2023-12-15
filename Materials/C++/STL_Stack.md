# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Stack 
```
#include <iostream>
#include <stack>

using namespace std;

int main() {
    // Declaration of a stack of integers
    stack<int> myStack;

    // Push elements onto the stack
    myStack.push(10);
    myStack.push(20);
    myStack.push(30);

    // Check if the stack is empty
    if (myStack.empty()) {
        cout << "Stack is empty.\n";
    } else {
        cout << "Stack is not empty.\n";
    }

    // Access the top element of the stack
    cout << "Top element: " << myStack.top() << "\n";

    // Pop the top element from the stack
    myStack.pop();

    // Size of the stack
    cout << "Size of the stack: " << myStack.size() << "\n";

    // Print elements of the stack
    cout << "Elements of the stack: ";
    while (!myStack.empty()) {
        cout << myStack.top() << " ";
        myStack.pop();
    }
    cout << "\n";

    return 0;
}
```
