
== Just Enough C


Objective C is still C. It just has a thin layer of code that adds object orientation plus a bunch of really powerful code libraries like Foundation and Cocoa. 

This section is not intended to be a proper introduction to the C language. But we will cover enough grounds so we can be ready for Objective C. 


=== Compilation and Program Structure

C is a compiled language. You write your program as text; to run the program, things proceed in two stages. First your text is compiled into machine instructions; then those machine instructions are executed. Thus, as with any compiled language, you can make two kinds of mistake

- Any purely syntactic errors (meaning that you spoke the C language incorrectly) will be caught by the compiler, and the program won’t even begin to run.
- If your program gets past the compiler, then it will run, but there is no guarantee that you haven’t made some other sort of mistake, which can be detected only by noticing that the program doesn’t behave as intended.

The C compiler is fussy, but you should accept its interference with good grace. The compiler is your friend: learn to love it. It may emit what looks like an irrelevant or incomprehensible error message, but when it does, the fact is that you’ve done some‐ thing wrong and the compiler has helpfully caught it for you. Also, the compiler can warn you if something seems like a possible mistake, even though it isn’t strictly illegal; these warnings, which differ from outright errors, are also helpful and should not be ignored.

I have said that running a program requires a preceding stage: compilation. But in fact there is another stage that precedes compilation: preprocessing. Preprocessing modifies your text, so when your text is handed to the compiler, it is not identical to the text you wrote. Preprocessing might sound tricky and intrusive, but in fact it proceeds only according to your instructions and is helpful for making your code clearer and more compact.

==== Code Structure

----
//
//  main.m
//  HelloWorld
//
//  Copyright (c) 2015 Teddy Hagos. All rights reserved.
//

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
  @autoreleasepool {
      // insert code here...
      NSLog(@"Hello, World!");
  }
    return 0;
}
----

- Every statement ends with a semi-colon. The compiler is unforgiving on this one. If you are coming from a language like JavaScript where you may or may not put the semi-colon, you need to be mindful of the C’s compiler behavior about terminating statements.  
- Single line comments are written as two forward slashes. Everything to its right until the end of the line is ignored by the compiler
- Multi-line comments are achieved by enclosing the text with a pair of /*   */. 
- C, and hence, Objective-C is a case sensitive language
- The main() function is mandatory , even for C. If you try to compile a code without int main() or void main() as the program entry point, the compiler will give you grief



----
/*
This code won't compile nor link.
The entry point main cannot be implicit
*/

#import <stdio.h>

//int main(){

  int i = 10;
  printf("Hello %d", i);
//  return 0;
//}
----


==== Compiler History

Originally, XCode’s compiler was the free open source GCC http://gcc.gnu.org[gcc.gnu.org]. Eventually, Apple introduced its own free open source compiler, LLVM http://llvm.org[llvm.org], also referred to as Clang, thus allowing for improvements that were impossible with GCC. Changing compilers is scary, so Apple proceeded in stages:

- In XCode 3, along with both LLVM and GCC, Apple supplied a hybrid compiler, LLVM-GCC, which provided the advantages of LLVM compilation while parsing code with GCC for maximum backward compatibility, without making it the de‐ fault compiler.
- In XCode 4, LLVM-GCC became the default compiler, but GCC remained available.
- In XCode 4.2, LLVM 3.0 became the default compiler, and pure GCC was withdrawn.
- In XCode 4.6, LLVM advanced to version 4.2.
- In XCode 5, LLVM-GCC has been withdrawn; the compiler is now LLVM 5.0, and the transition from GCC to LLVM is complete.

=== Data Types

The data types that Objective-C uses are, as you might expect, the data types of C. The only added types are the id type and object types -- the kind you create out of classes. That will be discussed more on sections of their own. 
 
Objective-C is statically typed, which is why we need to be explicit about the kind of variable we will use in our program. Static typing allows the compiler to catch errors like when you declare that a variable should be used for counting (whole numbers, int) then somewhere in your program you assigned it a value that is more appropriate for measuring (numbers with decimal portions, float or double). The compiler can catch that error very early because we declared the kind of variables that we will use.

==== Native Types

These kinds of variable are pre-defined in the language or the compiler. You can think of them as built-in variable types. You can pretty much use them in any function or statement because they do not require inclusion or availability of any special type definition in advance.  Variables that are not native types are generally called user-defined types. These are the kinds you define on your own like classes, structure, union and function etc.
 
There are a small number of native types, most of them are of the numeric kind but they can used to store characters, truthiness, falseness and memory addresses (pointers). 

==== Integers

Integers are whole numbers. The kind you use for counting. There are several integer types in Objective-C. These are char, int, short, long and long long. 

char::
  1 byte, 128 to 127 or 0 to 255

unsigned Char::
  1 byte, 0 to 255

signed Char::
  1 byte, -128 to 127

int::
  2 or 4 bytes, 32768 to 32,767 or -2,147,483,648 to 2,147,483,647

unsigned Int::
  2 or 4 bytes, 0 to 65,535 or 0 to 4,294,967,295

short::
  2 bytes, -32,768 to 32,767 

unsigned short::
  2 bytes, 0 to 65,535

long::
  4 bytes, -2,147,483,648 to 2,147,483,647

unsigned long::
  4 bytes, 0 to 4,4294,967,295

==== Floats

These are the kinds you use for measuring. Float points are the computers approximation of a real number.  Double or float, but it is possible to say long double

float::
  4 bytes, 1.2E-38 to 3.4E+38, accurate to 6 decimal places

double:: 
  8 bytes, 2.3E-308 to 1.7E+308, accurate to 15 decimal places

long double::
  10 bytes, 3.4E-4932 to 1.1E+4932, accurate to 19 decimal places

The code sample below shows a typical usager on how to declare common variable types

----
#include <stdio.h>
#include <stdbool.h>

int main() {
  
  int a = 100;
  float b = 100.00f;
  double c = 100e+2;
  char d = 'g';
  bool e = true;
  long long f = 100;
  
  
  printf("a  = %i size = %lu bytes\n", a, sizeof(a));
  printf("b  = %f size = %lu bytes\n", b, sizeof(b));
  printf("c  = %e size = %lu bytes\n", c, sizeof(c));
  printf("d  = %c size = %lu bytes\n", d, sizeof(d));
  printf("e  = %i size = %lu bytes\n", e, sizeof(e));
  printf("f  = %lld size = %lu bytes\n", f, sizeof(f));
  
  
  return 0;
}
----

The code below displays the smallest and highest values that the primitive data types can hold

----
#include <stdio.h>
#include <limits.h>

int main() {
  
  printf("Smallest signed char %d\n", SCHAR_MIN);
  printf("Largest signed char %d\n", SCHAR_MAX);
  printf("Largest unsigned char %d\n", UCHAR_MAX);
  
  printf("Smallest signed short %d\n", SHRT_MIN);
  printf("Largest signed short %d\n", SHRT_MAX);
  printf("Largest unsigned short %d\n", USHRT_MAX);
  
  printf("Smallest signed int %d\n", INT_MIN);
  printf("Largest signed int %d\n", INT_MAX);
  printf("Largest unsigned int %d\n", INT_MAX);
  
  return 0;
}
----

==== Booleans

In C99, the boolean type was added to the language. Although technically it is still represented as an int but the only allowed values are just 0 and 1 (0 for true and 1 for false).

----
#include <stdio.h>

int main() {

  _Bool a = 0;

  if(a) {
    puts("A is true");
  }

  return 0;
}
----

If you include the stdbool.h header, you can use the bool type and the constants true or false in the code.

----
#import <Foundation/Foundation.h>

int main() {

  BOOL a = true;
  _Bool b = false;
  bool c = true;

  if(a) {
    puts("A is true");
  }

  return 0;
}
----

If you use the Foundation framework, you can use either _Bool, bool or BOOL. 

=== Variable Declarations

C is a statically and strongly typed. Which means before you can use data that is referenced by a variable, that variable must have been properly defined and initialized. During the early days of C, the days of K & R, you cannot use variables unless it has been both defined and initialized. Later compilers relaxed this rule, so now, we can actually write codes like this

----
#include <stdio.h>

int main() {

  int a;
  printf("The value of a is %d", a);
  return 0;
}
----

This will actually compile and run. You need to remember that C, unlike other languages you may have used e.g. Java or C#, does not initialize variable to a default value. In the code example above, the variable a will not be automatically initialized to zero. 

What the code did was to actually grab an area of memory, called it variable a and then displayed whatever value is already on the location of the variable a. Moral lesson in this code is – always initialize your variables.


----
#include <stdio.h>

int main() {

  int a;
  printf("The value of a is %d\n", a);
  printf("The size of a is %lu bytes", sizeof(a));
  return 0;
}
----

=== Strings 

The Foundation framework in Objective-C has a very robust and sophisticated way for handling strings. However, you might encounter some legacy codes in the course of your programming that are not using the NSString, but instead are using C Style strings. It’s a good idea to have some basic grounding with C Style strings 

----
#import <Foundation/Foundation.h>

int main(int args, const char *argv[]) {
  
  char string [] = "Hello";
  printf("%s", string);
  
  return 0;
}
----

As an aside, notice that I did not `include` the stdio.h anymore, instead I #imported the Foundation header file. The foundation framework takes care of a lot of things for us, even the legacy printf is included in there.

C Style strings is just an array of character.

----
#import <Foundation/Foundation.h>

int main(int args, const char *argv[]) {
  
  char string [] = "Hello";
  printf("%s has a length of %lu", string, strlen(string));
  
  
  return 0;
}
----

strlen is a function that is included in string.h, but since I imported the foundation framework, I’ve got that one covered as well. It’s very to see what strlen does, it returns the number of characters in the char array (string).

If we wanted to concat two strings,  we might try something like this

----
char s1 [] = "Hello ";
char s2 [] = "World\n"
printf(“%s”, s1 + s2 );
----

Unfortunately, the code above will not work. Strings in C are a bit finicky, they are not so simple to work on.  To combine two strings, we need two string functions, the strcpy and strcat.

----
#import <Foundation/Foundation.h>

int main(int args, const char *argv[]) {
  
  char s1 [] = "Hello ";
  char s2 [] = "World\n";
  char s3 [100];
  
  strcpy(s3, s1);
  strcat(s3, s2);
  
  printf("%s has a length of %lu", s3, strlen(s3));
  
  
  return 0;
}
----


=== Objective-C Strings

This part of the training is supposed to be concentrating on C, but we will make an exception. C Style strings are quite difficult to work with. The NSString which is a part of the Foundation framework is more advanced and robust.  So that is what we will use. In the course of your Mac programming (or iOS programming), you will rarely have to work with C Style strings

NSString is an object, and all objects in Objective-C are pointers, hence, you will work with pointers when dealing with objects. To define an NSString, the syntax is

----
NSSTring *mystring;
----


To initialize a string object (from now on, when I say string object, I mean an NSString object, not a C Style string). The following codes can be used.

----
NSString *mystring = @”Hello World”;
----

Notice that there is an at sign before the enclosed quotes? That is a clue to the compiler (compiler directive actually) which tells it that “Hello World” is not a C Style string, it’s an Objective-C construction.  If you remove the at sign by mistake, it will be treated as a C Style string – you will get an error.


=== Pointers


=== Structs

Structs is one way for C to create a user defined data type (UDT).  It provides a way for us to aggregate a fixed set of data members into a single data type.  Here’s how it might look like

----
struct employee {
  int id;
  const char *name;
  const char *role;
};
----

That looks a lot like the construction of class, if you already have a background in an OO language like Java, 
C# or C++. But remember, this is C. This language was out in 1970, Structs predates classes. 

A struct is defined using the struct keyword followed by the name of the structure. You will supply the name of the structure, after all, it is your user defined type. The aggregated data are enclosed in the struct block and the struct declaration is terminated by a semi-colon. It is considered a statement.

The struct can contain any valid data type. It can even contain other structs.

This is how you might use it in code

----
struct employee {
  int id;
  const char *name;
  const char *position;
};

int main() {

  struct employee john = {28, "John Doe", "Boss"};
  printf("%s is the %s and his id is %i", john.name, john.position, john.id);

  return 0;
}
----

if we were  to access the members of the struct using a pointer, instead of directly accessing it via the struct variable, the code might look like the one below


----
struct employee {
  int id;
  char *name;
  char *position;
};

int main() {

  struct employee john = {28, "John Doe", "Boss"};
  struct employee *ep = &john;
 
  printf("%s is the %s and his id is %i", ep->name, ep->position, ep->id);

  return 0;
}
----

This is a very common idiom because you might have a lot of employee objects in your code. You can probably do a construction where you assign each employee object to the pointer in a loop. 

If you will be creating quite a lot of employee objects,  you can also use the typedef keyword to simplify coding a little bit

----
typedef struct employee {
  int id;
  char *name;
  char *position;
} employee_t;

int main() {

  employee_t john = {28, "John Doe", "Boss"};
  employee_t *ep = &john;
 
  printf("%s is the %s and his id is %i", ep->name, ep->position, ep->id);

  return 0;
}
----













