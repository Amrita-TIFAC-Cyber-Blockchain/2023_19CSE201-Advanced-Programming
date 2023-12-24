# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Access Specifiers
- Public
- Protected
- Private

### C++ 
```
class AccessSpecifierExampleClass {
public:
    // Public members are accessible from outside the class
    int publicVar;

    void publicMethod() {
        // Code for the public method
    }

protected:
    // Protected members are accessible from within the class and its derived classes
    int protectedVar;

    void protectedMethod() {
        // Code for the protected method
    }

private:
    // Private members are only accessible within the class
    int privateVar;

    void privateMethod() {
        // Code for the private method
    }
};
```

### Python
```
class AccessSpecifierExampleClass:
    def __init__(self):
        # Public attribute
        self.public_var = 0

        # Protected attribute (convention using a single leading underscore)
        self._protected_var = 0

        # Private attribute (convention using a double leading underscore)
        self.__private_var = 0

    def public_method(self):
        # Code for the public method
        pass

    def _protected_method(self):
        # Code for the protected method
        pass

    def __private_method(self):
        # Code for the private method
        pass
```

Note: __X__, are typically used for special methods or attributes. These are often referred to as "**dunder**" (double underscore) methods. 
