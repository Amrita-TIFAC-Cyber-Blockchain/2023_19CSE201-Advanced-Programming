# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Stack

```
"""
Author: Ramaguru
Description: This script defines a Stack class with fundamental stack operations, including push, pop, checking if the stack is empty or full, and peeking at the top element. The class also includes a size method to get the current size of the stack.
"""

class Stack:
    def __init__(self, size):
        """
        Initializes a new Stack with a specified size.

        Parameters:
        - size (int): The maximum size of the stack.
        """
        self.elements = []
        self.size = size

    def is_empty(self):
        """
        Checks if the stack is empty.

        Returns:
        - bool: True if the stack is empty, False otherwise.
        """
        return len(self.elements) == 0

    def is_full(self):
        """
        Checks if the stack is full.

        Returns:
        - bool: True if the stack is full, False otherwise.
        """
        return len(self.elements) == self.size

    def push(self, item2Add):
        """
        Adds an element to the top of the stack if it is not full.

        Parameters:
        - item2Add: The item to be added to the stack.

        Prints:
        - "Stack is Full" if the stack is already full.
        """
        if not self.is_full():
            self.elements.append(item2Add)
        else:
            print("Stack is Full")

    def pop(self):
        """
        Removes the top element from the stack if it is not empty.

        Prints:
        - "Stack is Empty" if the stack is already empty.
        """
        if not self.is_empty():
            self.elements.pop()
        else:
            print("Stack is Empty")

    def top(self):
        """
        Retrieves the top element of the stack if it is not empty.

        Returns:
        - The top element of the stack.

        Prints:
        - "Stack is empty" if the stack is already empty.
        """
        if not self.is_empty():
            return self.elements[-1]
        else:
            print("Stack is empty")

    def get_size(self):
        """
        Gets the current size of the stack.

        Returns:
        - int: The current size of the stack.
        """
        return len(self.elements)


# Example usage:
stack = Stack(5)
stack.push(10)
stack.push(25)
stack.push(5)

print("Size: ", stack.get_size())
print("Top: ", stack.top())

stack.push(30)
stack.push(15)
stack.push(20)
print("Top: ", stack.top())
print("Size: ", stack.get_size())
stack.push(40)
```
