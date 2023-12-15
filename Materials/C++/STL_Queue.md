# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Queue 
```
#include <iostream>
#include <queue>

using namespace std;

int main() {
    // Declaration of a queue of integers
    queue<int> myQueue;

    // Enqueue elements into the queue
    myQueue.push(10);
    myQueue.push(20);
    myQueue.push(30);

    // Check if the queue is empty
    if (myQueue.empty()) {
        cout << "Queue is empty.\n";
    } else {
        cout << "Queue is not empty.\n";
    }

    // Access the front element of the queue
    cout << "Front element: " << myQueue.front() << "\n";

    // Access the back element of the queue
    cout << "Back element: " << myQueue.back() << "\n";

    // Dequeue the front element from the queue
    myQueue.pop();

    // Size of the queue
    cout << "Size of the queue: " << myQueue.size() << "\n";

    // Print elements of the queue
    cout << "Elements of the queue: ";
    while (!myQueue.empty()) {
        cout << myQueue.front() << " ";
        myQueue.pop();
    }
    cout << "\n";

    return 0;
}
```
