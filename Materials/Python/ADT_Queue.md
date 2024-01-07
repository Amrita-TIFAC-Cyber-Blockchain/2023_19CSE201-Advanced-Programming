# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Queue 

```
"""
Author: Ramaguru
Description: This script defines a Queue class with basic queue operations such as enqueue, dequeue, checking if the queue is empty or full, and retrieving the front and rear elements.
It also includes a size method to get the current size of the queue.
"""
class Queue:
    def __init__(self, size):
        """
        Initializes a new Queue with a specified size.

        Parameters:
        - size (int): The maximum size of the queue.
        """
        self.elements = []
        self.size = size

    def is_empty(self):
        """
        Checks if the queue is empty.

        Returns:
        - bool: True if the queue is empty, False otherwise.
        """
        return len(self.elements) == 0

    def is_full(self):
        """
        Checks if the queue is full.

        Returns:
        - bool: True if the queue is full, False otherwise.
        """
        return len(self.elements) == self.size

    def enqueue(self, item2Add):
        """
        Adds an element to the end of the queue if it is not full.

        Parameters:
        - item2Add: The item to be added to the queue.

        Prints:
        - "Queue is Full" if the queue is already full.
        """
        if not self.is_full():
            self.elements.append(item2Add)
        else:
            print("Queue is Full")

    def dequeue(self):
        """
        Removes the front element from the queue if it is not empty.

        Prints:
        - "Queue is Empty" if the queue is already empty.
        """
        if not self.is_empty():
            self.elements.pop(0)
        else:
            print("Queue is Empty")

    def front(self):
        """
        Retrieves the front element of the queue if it is not empty.

        Returns:
        - The front element of the queue.

        Prints:
        - "Queue is empty" if the queue is already empty.
        """
        if not self.is_empty():
            return self.elements[0]
        else:
            print("Queue is empty")

    def rear(self):
        """
        Retrieves the rear element of the queue if it is not empty.

        Returns:
        - The rear element of the queue.

        Prints:
        - "Queue is empty" if the queue is already empty.
        """
        if not self.is_empty():
            return self.elements[-1]
        else:
            print("Queue is empty")

    def get_size(self):
        """
        Gets the current size of the queue.

        Returns:
        - int: The current size of the queue.
        """
        return len(self.elements)


# Example usage:
queue = Queue(6)
queue.enqueue(5)
queue.enqueue(23)
queue.enqueue(99)

print("Size: ", queue.get_size())
print("Front: ", queue.front())
print("Rear: ", queue.rear())

queue.enqueue(44)
queue.enqueue(24)
queue.enqueue(18)
print("Front: ", queue.front())
print("Rear: ", queue.rear())
queue.enqueue(100)
```
