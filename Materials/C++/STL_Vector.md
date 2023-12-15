# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)


## Vector
```
#include <iostream>
#include <vector>

using namespace std;

int main() {
    // Declaration of a vector of integers
    vector<int> myVector;

    // Add elements to the vector
    myVector.push_back(10);
    myVector.push_back(20);
    myVector.push_back(30);

    // Check if the vector is empty
    if (myVector.empty()) {
        cout << "Vector is empty.\n";
    } else {
        cout << "Vector is not empty.\n";
    }

    // Access elements using indexing
    cout << "First element: " << myVector[0] << "\n";

    // Access elements using iterators
    cout << "Elements of the vector: ";
    for (auto it = myVector.begin(); it != myVector.end(); ++it) {
        cout << *it << " ";
    }
    cout << "\n";

    // Size of the vector
    cout << "Size of the vector: " << myVector.size() << "\n";

    // Clear the vector
    myVector.clear();

    // Check if the vector is empty after clearing
    if (myVector.empty()) {
        cout << "Vector is empty after clearing.\n";
    } else {
        cout << "Vector is not empty after clearing.\n";
    }

    return 0;
}
```
