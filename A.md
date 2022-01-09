# A

## abstract

> Modifier used in the declaration of a class, interface, or method.

It tells the compiler an actual instance of the item being defined will never occur.

Declaring something as abstract defines the item as an incomplete type,
for which the details will be supplied later.

To specify a class or method is not abstract - sometimes it is referred to as being concrete.

---
### abstract method

An abstract method is one that does not have a body defined for it. 

Eg.
`abstract int distance();
abstract void invert(int selection);`

The declaration of an abstract method has exactly the same format as a normal method declaration,

except:
- It is always preceded by the modifying keyword **abstract**.
- It never has a body

An abstract method can only be defined as part of an abstract class or part of an interface.
In fact, in defining an abstract method forces the class in which it appears to be abstract, and the class must be declared as such.

A method cannot be both **abstract** and **static**. This is so because an abstract method does not have a body, and a static method has its body resident inside the class definition.

---
### abstract interface

The keyword **abstract** is optional in the definition of an interface. The abstract keyword can be included in the declaration of an interface, but it makes no difference - all interfaces are abstract whether or not they are declared as such.

---
### abstract class

An abstract class is declared by using the keyword **abstract** as a modifier in its definition. An abstract class is considered an incomplete class by the compiler - one that cannot be instantiated.

Four things can cause a class to become abstract:
- A class that contains one or more abstract methods must be delcared as an abstract class.
- If a class extends an abstract class and does not implement methods with bodies for all of the superclass's abstract methods, it must be declared as an abstract class.
- If a class implements an interface but does not supply methods for all the methods defined in the interface, it must be declared as an abstract class.
- A class complete in all respects, but declared using the **abstract** keyword, is an abstract class.

The first 3 of these examples are actually the same. They all produce a class that contains an abstract method.

While the inclusion of an abstract method effectively does force a class to become abstract, the keyword **abstract** is still required to be used as part of the class definition. This acutally handy as an error-checking mechanism to ensure the programmer creating an abstract method. The presence of the keyword **abstract** removes all ambiguity.

An abstract class can contain **static** methods. These **static** methods, if they are also declared **public**, can be executed from other classes by preceding the method name with the name of the abstract class. This is reasonable because a static method resides inside the class definition and does not require the instance of an object for execution. One nice side effect of this is an abstract class can have a **main()** method enabling it to be executed directly as an application.

A class cannot be declared as both **abstract** and **final**. These 2 declarations are in conflict because a final class cannot be subclassed, and subclassing is all that can be done with an abstract class.

---

## access

> Access is part of scope.

The scope of a name is determined by *where* it is declared, while the access of a name is *how* it is declared.

The access qualifiers can be used to modify the scope. Four access qualifiers exist: *public, private, protected* and the default access (aka *package* access)

### public 

- A public class or interface can be accessed by any code that can access the package in which the class is declared.
- A public class or interface member (variable, method, or constructor) can be accessed by any code that can access the class or interface in which it is declared.
- A public member (variable, method, or contructor) can be accessed directly from a subclass without qualification. If the subclass has overriden the name in the super class, it can still be accessed with an expression of the form **super**.name.
- A public constructor can be accessed from a subclass by the expression **super()**.

### private

- A class or interface member (variable, method, or constructor) declared as privatre can be accessed only from within the compilation unit in which it is declared.

### protected

- The protected constructor of a class can be accessed by code in the same package in which the class is decalred - or by the code in any subclass of the class in which the protected constructor is declared.
- A protected ember (variable, method, or constructor) can be accessed directly from a subclass without qualification. If the subclass has overriden the name in the super class, it can still be accessed with an expression of the form **super**.name.
- A protected constructor can be accessed from a subclass by the expression **super()**
- A protected variable or method is inherited by any subclass of the class in which it is declared.

### The default

If no access modifier is declared, the default is assumed.
- A class that has no access declaration can be accessed only within the package in which it is declared.
- Interface members (method and variables) default to **public**.
- A constuctor can be accessed from a subclass by the expression **super()** only if the subclass is in the same package.
- A variable or method with defualt acess is inherited only by subclasses that are in the same package as the class in which it is declared.

---
