---
layout: guide
---

# C++ introduction for the mbed developer

The target audience for this guide is people who have not used C++ before and it will only cover the parts of C++ that is relevant for simple mbed programming.
Understanding of programming languages in general is expected.

# This code example

This guide will use the following code example called blinky to explain how to use C++ with mbed.
This program flashes a LED on the development board on and off.

```cpp
#include "mbed.h"

DigitalOut myled(LED1);

int main() {
    while(1) { // this is an infinite loop
        myled = 1;
        wait(0.4);
        myled = 0;
        wait(0.4);
    }
}
```

# Where general code is written

When writing your code you write it inside the `int main()` function.
This is the starting point for your program and every program will execute this function first.
In our example the program will start by running the `while` loop.

# Objects

Objects in C++ works mostly like objects in other languages like Java.

## Initialize object
To initialize an object you declare the object.

```cpp
DigitalOut myled(LED1);
```

This line creates a new object of type `DigitalOut` named `myled`.
Writing that line will call the constructor of the object.
In our case we are sending the constructor the parameter `LED1`.
After this line we have the variable `myled` initialized with the parameter `LED1`.

## Accessing attributes in objects

To access an attribute on an object you use the dot notation.

```cpp
// access myAttribute of myObject
myObject.myAttribute;

int amount = myObject.count;

// amount will now have the value of myObject.count
printf("%d", amount);
```

## Calling methods on objects

There are two ways of calling a method.
If your object is a pointer, you can call a function on an object using the arrow notation `->`.
If your object is not a pointer you can use the dot notation `.`.
Methods can either return a value or not return a value.
A value can be any value in C++ (int, char, object, struct, template....etc).

```cpp
MyObject *myObject = new MyObject();

// add a value to our object
myObject->setValue(0x12);

// get the value from our object
int value = myObject->getValue();

// value will be 0x12
printf("%x", value);

MyObject otherObject;

otherObject.setValue(0x12);

int otherValue = otherObject.getValue();

// otherValue will be 0x12
printf("%x", otherValue);
```

## Operator overloading

In C++ you may overload operators.
This is very common when working with objects.
Operator overloading means that you can define code that will say how an operator works with two objects.

For instance, say we have the object `Point` that has an x and y coordinate and a constructor to initialize a point with two numbers.
This object also has overloaded the `+` function which returns a new point with both values added together.

```cpp
Point a(2, 1);
Point b(4, 2);

Point c = a + b;

// c will now have the values (6, 3)
```

Almost all operators can be overloaded.
You can see in our example code the line

```cpp
myled = 1;
```

Here the `=` operator is overloaded and is used to set the value of the led.
So when you change the value of `myled` here you are actually setting the value of the led.

# Defines in C++

A define is a way of giving names to constant values in C++.
They look like this:

```cpp
#define MYCONSTANT 0x12

// value will now be 0x12
int value = MYCONSTANT;
```

What happens is that the compiler will change every occurence of `MYCONSTANT` to `0x12` before continuing the compiling.
This stage is called preprocessing.
You have also some more precompile options.

```cpp
#if SOMETHING // true if SOMETHING > 0

#ifdef SOMETHING // true if SOMETHING is defined (i.e #define SOMETHING)

#ifndef SOMETHING // true if  SOMETHING is NOT defined

#else //else clause

#endif // ends the if
```

These are the most common preprocessing tags in C++.

# Includes in C++

The first few lines of most C++ programs contain the mystic phrase `#include ...`.
Include means to put content of one file into another.
This is roughly equivalent to Java's `import`.

The following code puts the main mbed file into our project.

```cpp
#include "mbed.h"
```

From `mbed.h` we get objects such as `DigitalOut` and constants like `LED1` that we are now familiar with.
Without this include we were not able to use these objects and constants.

If we want to use for instance a vector (known as ArrayList in Java and list in Python) for keeping a lot of values we need to include it.

```
#include <vector>

std::vector<int> vec;

vec.push_back(0x12);
vec.push_back(0xAA);
vec.push_back(0x00);

// 0x12
printf("%d", vec[0]);

// 0xAA
printf("%d", vec[1]);

// 0x00
printf("%d", vec[2]);
```

## Crocodile mouths or quotation marks?

There are two ways of writing an include.

```
#include <vector>
#include "mbed.h"
```

The difference here is that `<>` checks for the name in all directives pre-assigned by the compiler.
`""` checks in the same directory as your project.

So in our example `vector` is a system standard library and will be gotten from where the compiler says that `vector` is located.
On the other hand `mbed.h` is gotten from our version of mbed in our project.
This is nice because a breaking change in mbed will not necessarily mean that your project breaks.

## Header guards

When including a file what acutally happens is that the contents of the file is copied into the file that includes it.
To avoid things being defined twice there is a thing called header guards.
This is a small piece of code that makes sure that things are only declared the first time this file is included.

```
#ifndef __MYFILE__HEADER__H__
#define __MYFILE__HEADER__H__

// declare stuff here

#endif
```

So what is happening here?
Well we first check if a ridiculously specific name is defined, if not, we define it.
Right after it is defined we run the code in `// declare stuff here`.
The next time someone includes the same file, the ridiculously specific name will be defined, and we won't run the code below.

If all header files have this, it will always be safe to include them wherever you need them.

# Printing in C++

The function `printf()` prints output to the console where the program is being run.
The following code snippet will print "Hello World" to the console.

```
printf("Hello World");
```

If you want to print variables you will need to use special characters to print these and give the variables as arguments afterwards.
`%s` says that a string should replace these characters and `%d` says a number should replace these characters.

```
#include <string>

int i = 20;

printf("The number is %d", i); // This prints "The number is 20"

string str1 = "Hello";

printf("The first string is %s", str1);
```