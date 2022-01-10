# S

## Scope

The scope of a name is the region in which it has effect. Scope is the natural result of the location in which a name is declared - but this can be modified, to some degree, with the access keywords **public, private** and **protected**.

### From outer to inner

#### Top-level scope

This is the largest scope - Java puts no limitations on the outermost scope. The broadness of this scope is really determined by the host system. This is the scope of the top-level packages, and a package name never hides any of its internal names.

#### Package scope

Class and interface declarations, by default, have the scope level of the entire package in which they are declared. If no package is specified (by using the **package** keyword) a class or interface defaults to being a member of the unnamed package - this is a package the same as any other, except it has no name.

#### Compilation-unit scope

This scoping level can be thought of as the class and interfaces in a single source file, and the items referenced externally through the **import** statement. Several classes and interfaces can be defined in one source file, but only one of them (the one with the same name as the file) can have more than compilation-unit scope. The **import** statement can limit the items being imported by being very specific with the single-type import (such as **import java.awt.Graphics**), or it can provide a more inclusive type-on-demand import (such as **import java.awt.**). The latter form includes more names into the scope.

#### Class scope

Variables and constants declared within a class, but outside a method, are avaliable from the point of their declaration to the end of the compilation unit. For example, this code will not compile:
```
public class ScopeTest{  
    int a = b // Error. Forward reference  
    int b = 10;  
}
```
Methods and classes declared inside a classs also have class scope, but with no such forward reference limitation as exists with variables. This code will compile cleanly: 
```
void showHands(){  
    System.out.println(hands());  
}  
String hands() {  
    return "Hands";  
}
```
#### Parameter scope

The scope of a method or constructor parameter is the entire body of the method or constructor.

#### Block scope

Declaration of variables inside a block within a method have the same forward reference limitation as those at the class level. A block can be an entire method or subset of it as defined by opening and closing braces. This code will cause an error:
```
public class ScopeTest() {  
    void poo(){  
      int m = n; // Error. Forward reference.  
      int n = 10;  
    }  
}
```
#### For scope

This is a special case of the block scope. Any variable declared as part of the **for** statement is included in the scope of the body of the loop.
For example:
```
public class ForScopeTest(){  
    public static void main(String[] args){  
      // The variable i is undefined here  
      for(int i = 0; i < 10; i++){  
        System.out.println("i=" + i);  
      }  
      // The variable i is undefined here  
    }  
}
```
The variable **i** is declared inside the parentheses of the **for** statement, which defines its scope as being from the point of its declaration to the end of the code block, but no further. The variable cannot be accessed after the loop has exited.

#### Exception-handler scope

There is a special case of scoping that occurs with the **catch** statement in a **try-catch** block. The parameter - the instance of the **Exception** class that is the argument of the **catch** statement - has the scope of the block associated with **catch**. Syntactically, this is similar to parameter scope. For example, the **exception** variable is avaliable inside this **catch** block:
```
try{  
  ....  
}catch(Exception exception){  
  System.out.println(exception);  
}
```
#### Hidden Names

It is possible for a variable to be hidden from a region of the code that would normally be within its scope. Take this example:
```
public class HiddenScope{  
    static int bob = 333;  
    public static void main(String[] args){  
        bigBob();  
        littleBob();  
    }  
    public static void bigBob(){  
        int bob = 999;  
        System.out.println("bigBob=" + bob);  
    }  
    public static void littleBob(){  
        System.out.println("bigBob=" + bob);  
    }  
}
```
There are 2 **bob**s in this program - one is defined classwide and has the value 333, and the other is defined in the method **bigBob()** and has the value 999. This is the output from the program:
```
bigBob=999  
littleBob=333
```
As you can see, the local declaration of **bob** completely masks the classwide definition.

---

## Serializable

---

## Static

2 things can be declared static in Java - methods and fields.

**A static field**  
This is also called a *static variable* or a *class variable*. There will never be more than one copy of a static field. A nonstatic field is duplicated once for every object, but a static field remains the property of the class, and every object referring to the field refers to the same value. It is not possible to include a static declaration of a variable inside a method - even if the method itself is static.

**A static method**  
A static method is like a static field - it is part of the class, not an object. A static method can only refer to other static entities of the class (static fields or other static methods). A non-static method can us **this** to refer to the current object - a static method does not have a **this** reference. A static method can be called even if there are no instances of the object, because it never refers to an object (unless one is explicitly passed to it).

**Example 1**  
This example demostates that there is only one copy of a static variable. The static variable **colorName** is defined in the class **ColorName**. 2 classes, **BlueName** and **RedName**, inherit the field because they both extend the class **ColorName**. What they really inherit is the ability to address the same field:
```
public class StaticColorName extends ColorName{
    public static void main(String[] args){
        System.out.println(colorName);
        BlueName bn = new BlueName();
        bn.doBlue();
        System.out.println(colorName);
        RedName rn = new RedName();
        rn.doRed();
        System.out.println(colorName);
    }
}
class ColorName{
    public static String colorName = "SCN: ";
}
class BlueName extends ColorName{
    public void doBlue(){
        ColorName cn = new ColorName();
        colorName += "Blue ";
    }
}
class RedName extends ColorName(){
    public void doRed(){
        ColorName cn = new ColorName();
        colorName += "Red";
    }
}
```
The class **StaticColorName** also extends **ColorName**, so it has access to the same field. Here is the output from the program:
```
SCN:  
SCN: Blue
SCN: Blue Red
```
Each of these lines is output from **StaticColorName**, and as you can see, each object modifies the same field value.

**Example 2** 
This example demostrates that static variables can be accessed from static and nonstatic methods:
```
public class StaticShare{
    static int counter;
    String name;
    int myCounter;
    public static void main(String[] args){
        System.out.println("Starting counter: " + StaticShare.getCounter());
        StaticShare s1 = new StaticShare("red");
        StaticShare s2 = new StaticShare("white");
        StaticShare s3 = new StaticShare("blue");
        System.out.println(s1);
        System.out.println(s2);
        System.out.println(s3);
        System.out.println("Ending counter: " + StaticShare.getCounter());
    }
    public StaticShare(String name){
        this.name = name;
        myCounter = ++counter;
    }
    public String toString(){
        return "The name " + name + " has the value " + myCounter;
    }
    public static int getCounter(){
        return counter;
    }
}
```
The **counter** is incremented whenever a new object is created, and the value is copied into the local variable **myCounter** as a sort of unique ID number for each one. The static method **getCounter()** can be called by using the name of the class (being **static**, it is not associated with nay object), and because the **counter** is also **static**, it is able to return the value that will be used by the next instantiation of an object. This is done before any objects are created and again after several objects have been created. The output looks like this:
```
Starting counter: 0
The name red has the value 1
The name white has the value 2
The name blue has the value 3
Ending counter: 3
```

---

## static initializers

There is one place for executable code in Java other than inside a method - inside a **static initializer**. This is a block of code used to set the initial value of a static field.

**No exceptions**  
A static initializer block cannot throw checked exceptions. This cannot be done directly or indirectly (by calling a method that throws an exception). Although a static initializer cannot throw an exception, it can catch one - it is valid to include a **try-catch** block inside the static initializer.

**An unreliable combination**
Becuase static initializer code can instatiate objects and call methods, it is possible to write a program such that the initialization of class A could require that class B be initialized, and class B could, in turn, depend on class A. This cannot be reliably detected by the compiler (because A and B could be compiled separately), so when this happens, some of the variables may simply not be initialized.

**Example**  
This example initializes an array such that each member of the array is the sum of the previous 2 (Fibonacci Sequence):
```
public class StaticInit{
    private static int[] fibo = new int[10];
    private static int fiboTotal;
    static{
        fibo[0] = 0;
        fibo[1] = 1;
        for(int i = 2; i < 10; i++)
            fibo[i] = fibo[i-1] + fibo[i-2];
        fiboTotal = fiboSum();
    }
    public static void main(String[] args){
        for(int i = 0; i < 10; i++)
            System.out.println(fibo[i] + " ");
        System.out.println();
        System.out.println("Total: " + fiboTotal);
    }
    private static int fiboSum(){
        int sum = 0;
        for(int j = 0; j < 10; j++)
            sum += fibo[j];
        return sum;
    }
}
```
The static variables **fibo** and **fiboTotal** are both initialized in the same static initializer block. First, the array is initialized in a loop, and then the static method **fiboSum()** is called to return the sum of the values. The output looks like this:
```
0 1 1 2 3 5 8 13 21 34
Total: 88
```

---
