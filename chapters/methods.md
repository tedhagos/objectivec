
== Methods



Methods can be defined either as belonging to the class or scoped to the instance of the class (the object). Unlike Java or C#, where you write a method the way you would write a function, that is not the case in Objective C. Methods are written distinctly different than regular functions.

=== Static and Instance Methods

An instance method, that is a method scoped to the object rather it's class, is written with a minus sign in the beginning. A method scoped to the class (a static method) is written out with a plus sign instead

----
@interface Person : NSObject 
  -(void talk);
@end

@implementation Person
  -(void) talk {
    NSLog(@"Person talking");
  }
@end

int main() {
  @autoreleasepool {
    Person *p = [[Person alloc] init];
    [p talk];
  }
  return 0;
}
----

The differences and eccentricities of Objective C methods as compared to some other CFOL languages does not end with how the method is written. It also extends to how the method is invoked.

Objective C has this concept of message sending and message receiving. If you want an object that contains the method to execute that method, you send it a message, and that message is the name of the method. The code example above needs to be dissected a little bit so we can map it to our existing knowledge of classes and objects (for those coming from either C# or Java).

To create an object, we use the following syntax

----
Person *p = [[Person alloc] init];
----

The sample snippet above is quite idiomatic, you will see lots of those codes from other people's examples and book text. It is a nested way of method calling. If we were doing in Java or C#, maybe it would look like this

----
Person *p = Person.alloc().init();
----

Of course the snippet above is contrived, but that's how it might look if it were written by either a C# or a Java programmer. Let's rewrite our little code for creating an object so it becomes a bit more verbose.

----
Person *p; // <1>
p = [Person alloc]; //<2>
p = [p init]; // <3>
----

<1> Declares a variable that will hold the reference to a Person object. It is of type Person and it is a pointer because all objects in Objective-C are of pointer types.

<2> Calls the class method alloc against the Person class. We are simply allocating memory for our would-be object. Notice two things in here

<3> I did not have to use the pointer operator anymore. You only use the pointer operator (when working with objects) when declaring them. That is the only time you use the asterisk. You don't use the asterisk when assigning something to the variable

When we call a method against a class or object, we use the bracket notation. The receiver of the message and the message itself are enclosed on a pair of square braces
Third line of the sample code calls the init method of our now allocated object. You can think of the init method as some sort of a constructor equivalent in C# or Java.

=== Arguments

When you want methods to take on some arguments or parameters, here's the syntax on how to do it

----
-(void) talk: (NSString*) args;
----

By now you would have noticed that a pair of parens is not part of a method signature in Objective-C, and simply putting the arguments inside the pair of parens is not the way to write a method that accepts arguments.

When a method must take one argument, a colon is appended to right of the method name. The type of the argument is then written next. The argument type is enclosed in parens. Afterwhich, the name of the argument is written last.

----
-(void) foo;
-(void) goo: (int) args;
-(void) boo: (float) args;
-(void) coo: (BOOL) args;
-(void) doo: (NSString*) args;
----

Code snippet above shows how to declare methods that take on a single argument. Going back to our Person class example, if we were to accept an argument on the talk method, here's how it will be written and called.

----
@interface Person : NSObject
-(void) talk: (NSString*) args;
@end

@implementation Person
-(void) talk: (NSString*) args {
  NSLog(args);
}

+(id) new {
  NSLog(@"Overriding new method");
  return [[self alloc] init];
}
@end

int main(int argc, const char *argv[]) {
  Person *p;
  p = [Person alloc];
  p = [p init];
  [p talk: @"Hello there"];
}
----

=== Multiple arguments

Writing a method that takes on multiple arguments is one of the things that weirds out a newcomer to Objective-C. Each argument actually needs to be written out with it's own method signature. Take a look at a sample method name in the snippet below

----
-(void) boo: (int) a Coo:(int) b Doo:(int) c;
----

The full name of the method is booCooDoo. The method takes on 3 arguments, each is prepended a small part of the method name.

This might seem weird at first, but it is actually expressive and verbose. One of the downsides of this kind of mechanism for writing methods is that you can easily write methods with very long names; and Objective-C is notorious for this.

Here's the complete snippet for booCooDoo

----
#import <Foundation/Foundation.h>

@interface Obj : NSObject
-(void) boo: (int) a Coo:(int) b Doo:(int) c;
@end 

@implementation Obj
-(void) boo:(int) a Coo:(int) b Doo:(int) c {
  NSLog(@"The params are %i, %i and %i", a,b,c);
}
@end

int main(int argc, const char *argv[]){
  @autoreleasepool {
    [[Obj new] boo: 1 Coo:2 Doo:3];
  }
}
----

=== Optional Arguments

There is no language level support optional or named arguments in Objective-C, so if you are quite used to languages like C# or Python where you can define optional and named arguments, no such joy in Objective-C. Look at the bright side though, you can make your methods very explicitly named.



==== Method Overloading

Same argument as optional arguments.

Make plenty of examples though

