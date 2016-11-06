
== Classes and Objects


Objective C adds language level support to the C language in order for us to do object oriented programming. While the syntax is straightforward, it may take a bit of getting used to if you are coming from other OO languages like Java or C#.


=== Parts of a class

There is actually no class keyword in Objective C, oddly enough. Classes are constructed using two files, an interface file and an implementation file. The interface file contains method signatures, property declarations and instance variables or ivars. The implementation file contains the actual implementation of the method signatures declared in the corresponding interface file.

A header file for a class declaration typically contains a single definition for an interface and looks like so.

----
#import <Foundation/Foundation.h>

@interface Person : NSObject {
  NSString *lastname;
  NSString *firstname;
}
-(void) talk;
@end
----

The header section of a class is defined by a pair of @interface and @end declarations. The name of the class is programmer defined and is written immediately after the @interface keyword.

The colon after the class name means that this class is a subclass, we are inheriting from something. In this particular case, we are inheriting or extending the NSObject class. Objective C follows a single rooted class inheritance mechanism and inheriting from something is mandatory. Unlike in other OO languages, the compiler will not assume that you want to inherit from a default superclass if you omit the declaration of a base class in the interface file. You need an explicit declaration of a base class when defining a new class in Objective C.

In our example, we inherited from NSObject, you can inherit from something else but NSObject is one of the two root classes in Objective C. The other one is NSProxy, which you will rarely use if ever. Almost all classes in the Foundation framework either directly or indirectly extend NSObject. Most of the classes you will define will probably extend NSObject too.

There are two intance variables or ivars defined in the header section. There are no explicit declaration of an access descriptor which means, the compiler will treat these ivars as @protected. We will discuss access modifiers for ivars in the next section.

The last part of the header file is a declaration of an abstract method. An abstract method is member function without a method body. It does not have definition. It is not a concrete method, hence, abstract. An abstract method is basically like a statement, it is terminated by a semi colon.

A header file is customarily written and saved in a file with a .h extension and the file name is typically and idiomatically the same name as the classname. In the case of our example code, the interface file will be saved in a file named Person.h.

Every header declaration for a class needs to be paired by a corresponding implementation bearing exactly the same name in the header definition. Our corresponding implementation could be written as follows.

----
#import <Foundation/Foundation.h>
#import "Person.h"

@implementation Person

-(void) talk {
  NSLog(@"Hello World");
}
@end
----

The import statement on the second line looks a bit different from the import on the first line. The Foundation headers are surrounded by angle brackets because these header files are built-in to language. For those kinds of headers, you use the angle brackets. In our case here, we need to import a header file that created on our own. For those kinds of files, you surround them with double quotes. We have to import the header definition for Person because the compiler will need that information.

After the import statements, the keyword @implementation together with the name of class is used denoting that this block contains the concrete definitions for the methods defined in the header file.

The method talk is not terminated with a semicolon but rather it is appended by a block. The block constitutes the body of the method. It makes it concrete.

==== Co-locating header and implementation files

The separation of header and implementation files, while idiomatic and good practice, is not required. You are free to write both the header definition and the implementation in a single file. If you do so, the extension of the file is customarily a .m file, and implementation source file. Here's an example of that

----
#import <Foundation/Foundation.h>

// class Person ------------------------------------
@interface Person : NSObject {
  NSString *lastname;
  NSString *firstname;
}
-(void) talk;
@end

@implementation Person
-(void) talk {
  NSLog(@"Hello World");
}
@end

// main --------------------------------------------
int main(int argc, const char *argv []){
  Person *p = [Person new];
  [p talk]
}
----

=== Inheritance

Objective C follows a single rooted class inheritance mechanism. You cannot inherit from more than one superclass. The basic syntax for extending a class is as follows

----
@interface Person : NSObject {
  NSString *lastname;
  NSString *firstname;

  @private NSString *foo;
}

-(void) talk;
-(void) walk;
@end

@interface Employee : Person
@end
----

To inherit from an existing class, use a colon to signify class extension followed by the name of an existing class, this will in turn become your super or base class.

The subclass or the child class inherits the following things:

All variables and methods that are public, which means all methods that are declared on the header file. The implicit and only access modifier for a method on a header file is public
All variables that are declared @protected in the superclass. When an ivar is defined on the header without any access modifier, it implicitly has protected access. Which means is only reachable from within the class that defined it and its subclasses. Variables with protected access are not reachable outside the class

=== Instance Variables

Instance variables or ivars can be declared either on the header or the implementation file. When ivars are defined on the header, they are implicitly `@protected`. And when they are defined on implementation file, they are implicitly `@private`.

----
#import <Foundation/Foundation.h>

// class Vehicle ------------------------------
@interface Vehicle : NSObject 
-(NSString*) platenumber;
-(void) setPlatenumber: (NSString*) args;
@end

@implementation Vehicle {
  NSString *platenumber;
}
-(NSString*) platenumber {
  return platenumber;
}
-(void) setPlatenumber:(NSString*) args {
  platenumber=args;
}
@end

// class Car -----------------------------------
@interface Car : Vehicle 
@end
@implementation Car
// this method will fail compilation because
// the ivar platenumber is not reachable from
// Car class
-(NSString*) platenumber {
  NSLog(@"Car polymorph");
  return platenumber;
}
@end

// main ----------------------------------------

int main(int argc, const char *argv[]) {
  @autoreleasepool {
    Car *v = [Vehicle new];
    [v setPlatenumber: @"1234"];
    NSLog(@"Car's plate number is %@", [v platenumber]);
  }
  return 0;
}
----

The code above will fail compilation. The compiler will immediately see that that platenumber ivar defined in the Vehicle class is private in scope (because it is defined in the implementation file without a qualifying access modifier).

The corrected version of the code sample above will not be given but instead left for reader as an exercise. Refactor the code so that platenumber ivar is inherited by the Car subclass.

===  Methods

You can find declarations of method in either or both the header and implementation files. Methods are usually declared on the header file, the interface part that is. This declaration only contains the following

Method type::
  Whether the method belongs to the object or its class. A minus sign means it is an instance method and plus sign means it belongs to to class

Return type::
  This could be either a primitive type or a reference type

Method name::
  You get to define this part, the usual rules in naming variables  apply to method names as well

Method arguments:: 
  Methods can accept optional arguments. Having methods accept some params is a bit different in Objective C, we will cover them in a separate section


When a method is declared on the header, it needs to have a corresponding definition on the implementation file

----
@interface Person : NSObject
-(void) walk;
@end

@implementation Person 
-(void) walk {
  NSLog(@"Person walking");
}
@end
----


The header files is supposed to contain only the declaration of methods. That means the only thing that can follow the method name is a semicolon, that makes it abstract. The method definition on the implementation file replaces the semicolon with a pair curly braces. The block constitutes the definition of the method, which makes the method concrete and not abstract anymore.

Declaring methods on the interface file is known as forward declaration and is quite idiomatic of C/C++ practice. Starting with XCode 4 (and the associated clang compilers), forward declarations seemed to have mattered less. So the compiler will let you get away with methods that are written only on the implementation file. It is still encouraged though to follow the practice of providing forward declarations.


