
== Properties


One the basic and common tasks in any object oriented language is the creation of value objects. C# programmers call this POCOS and Java programmers refer to these value objects as POJOS, plain ole C# objects and plain ole Java objects respectively.

There are a couple of ways to construct value objects in Objective-C. Some of these ways are frowned upon and highly discouraged, some have become mainstream practice like the use of @properties and some are pretty much the run-off-the-mill getters and setters. Will take a look at some examples on how to do each of them

=== Direct manipulation of public ivars

I don't have to tell you that this is the bad practice and quite frowned upon. It violates the principles of encapsulation which in turn violates the principles of cohesion and coupling. The less internal state an object exposes to the world around it, the better. But anyway, here's how the code will look like

----
#import <Foundation/Foundation.h>

@interface Person : NSObject{
  @public NSString *lastname;
  @public NSString *firstname;
}
@end

@implementation Person
@end

/// main
int main(int argc, const char *argv []){
  Person *p = [Person new];
  p->firstname = @"Ted";
  p->lastname = @"Hagos";
  NSLog(@" Name is %@ , %@", p->lastname, p->firstname);
}
----

Objective-C being a superset of the C language, you can pretty much access the members of an object using the arrow operator (p is a pointer to the Person object, remember?). Now that you've seen how to do it, try your best to forget about this. This example was coded purely for reference purposes only.

As a side note, the ivars lastname and firstname were defined as @public because by default, when ivars are defined on an interface file, the default access modifier is @protected, which means it is only reachable or accessible from either the class that defined the ivars and its subclasses. 

=== Straight up Getters and Setters

The most common way of coding value objects is of course, creating methods within the class to manipulate its internal ivars.

----
@interface Person : NSObject
-(void) setLastname:(NSString*) arg;
-(void) setFirstname: (NSString*) arg;
-(NSString*) lastname;
-(NSString*) firstname;
@end
----

On the interface file of the Person class, I only defined a pair of methods that sets and gets the lastname/firstname. Notice that the interface file does not contain any definition of the ivars; if ivars are going to be private anyway, they are better written out on the implementation file, not the interface.

Now, the implementation file

----
@implementation Person{
  @private NSString *lastname;
  @private NSString *firstname;
}
-(void) setLastname:(NSString*) arg{lastname = arg;}
-(void) setFirstname: (NSString*) arg {firstname = arg;}
-(NSString*) lastname {return lastname;}
-(NSString*) firstname {return firstname;}
@end
----

No surprises here, they are basically just setting and getting the lastname and firstname private variables of Person object. You may have noticed that the name of the getter method is the same as the name of ivar. This is not required but it is quite idiomatic to do this in Objective-C. Don't worry that the runtime will be confused because we used the same name in the ivar and method, it is smart enough to tell the difference.

The next code snippet shows the main function which sets and gets the members of the value object.

----
int main(int argc, const char *argv []){
  Person *p = [Person new];
  [p setFirstname:@"Ted"];
  [p setLastname: @"Hagos"];
  NSLog(@" Name is %@ , %@", [p lastname], [p firstname]);
}
----

There is nothing wrong coding value objects this way, except that Objective-C has a more idiomatic way of creating value objects using properties

=== Properties

Objective C has language level support for creating value objects. Instead of manually creating accessor methods, we can declare ivars as properties and even synthesize, that means generate, accessor methods for these ivars.

Let's rewrite the Person value object again. We will start by defining lastname and first as properties on the interface file. We don't have to write anything on the implementation file, but we still have to provide it even if it is empty

----
#import <Foundation/Foundation.h>

@interface Person : NSObject
@property NSString *lastname, *firstname;
@end

@implementation Person
@end
----

In older versions of XCode (and the clang compiler that came with it), we actually had to write the `@synthesize` statement on the implementation file, like this

----
@implementation Person
@synthesize lastname, firstname
@end
----

But now, if you are using XCode version 4 or higher, we don't have to write the synthesize statement anymore. The compiler is smart enough to figure out that we would like to generate or synthesize accessor methods for our properties. In order to use our value object, we can write the following code inside the main function

----
int main(int argc, const char *argv[]){
  @autoreleasepool {
    Person *p = [Person new];
    [p setLastname: @"Doe"];
    [p setFirstname: @"John"];
    NSLog(@"The name is %@, %@", [p lastname], [p firstname]);
  }
  return 0;
}
----

The compiler automatically synthesized accessor methods for the properties we declared. Notice the convention it is using when generating the accessors. For the getter accessor, the method name it will synthesize is exactly the same name as the declared property; remember earlier that I said that practice is quite idiomatic in Objective-C, even the synthesizers are following that. As for the setter accessor, the method name will be set + name of property and it will be camel-cased or snake-cased, the first letter of the property name will be the only one capitalized.

==== Basic Mechanics of Property Declarations

Properties, unlike *ivars* are declared outside the curly braces

----
@interface Person : NSObject
@property NSString *lastname;
@property NSString *firstname;
@end
----

The above code snippet is the proper way to define properties. It has the `@property` keyword followed by a type then the name of variable.

The following code will give you an error

----
@interface Person : NSObject {
  @property NSString *lastname;
  @property NSString *firstname;
}
@end
----

Properties cannot be declared the way you would declare ivars. If you are using XCode to write your programs, you would see various error flags on the IDE already at this point. So don't worry, XCode will let you know as soon as you do something illegal.

===== Attributes

Properties may be declared with attributes, you might see some declarations like this

----
@property (weak) NSString *lastname;
----

The attributes of the properties has got something to do with reference counting and memory management of the Objective-C runtime. The following is a list of some property attributes you may use

atomic::
  has something to do with thread safety. All properties are **implicitly** atomic. If you don't specify an attribute, it will be atomic by default

non-atomic:: 
  this is not thread-safe. It has its uses. A non-atomic property may have less performance impact during runtime

strong::
  Again, this has something to do with memory management

weak::
  opposite of strong

retain::
  similar to strong

assign::
  The setter method performs a simple assignment of the property without using `copy` or `retain`. This is the default setting. This is similar to the weak attribute except that if the affected property is released, the value of the property will be set to nil

readwrite::
  It means the compiler will synthesize or generate both getter and setter accessors. This is the default

readonly::
  The compiler will only synthesize a getter accessor method

getter=methodName::
  if you want to specify a method name other than the default method name that will be synthsized by the compiler. The default getter accessor method name will be `set` + Nameofproperty. Note that the first character of the property name is capitalized

setter=methodName::
  if you want to specify a method name for the setter accessor other than the default method name that will be synthesized by the compiler. The default setter accessor method name is the same as the name of property

===== Property backed Instance Variables

When you declare a property, the compiler actually defines instance variables for you. In the background. This will be invisible to you by default, but you can mess around with it if you really like. Let's go back to the Person class example and have another look.

----
@interface Person : NSObject 
@property NSString *lastname;
@property NSString *firstname;
@end
----

In this example, we are creating two properties namely **lastname** and **firstname**. The compiler will synthesize accessor methods that will allow us to get and set these properties. It will also create  two ivars for us, namely `_firstname` and `_lastname`. These two variables are called property backed instance variables. It is as if, we have written our code like the following.


----
@interface Person : NSObject {
  NSString *_lastname;
  NSString *_firstname;
}
@property NSString *lastname;
@property NSString *firstname;
@end
----

This is all neat and cool, but how do we know those variables were actually created. Well, we can mess with it during initialization. Let's modify the implementation code for the Person class.


----
@implementation Person

-(id) init {
  self = [super init];
  if(self != nil) {
    _lastname = @"Nobody";
    _firstname = @"I am ";
  }
  return self;
} 
@end
----

We have overridden the default init for the Person class and in it, we initialized the property backed instance variables. This means, when we create a Person object, we don't have to call the setter methods because the lastname and firstname properties would have been initialized to some default values already. Let's test that using the following code

----
int main(int argc, const char *argv[]){
  @autoreleasepool {
    Person *p = [Person new];

    NSLog(@"The name is %@, %@", [p lastname], [p firstname]);
  }
  return 0;
}
----

=====  Specifying Custom Accessor Methods

If for some reason, you are not happy with the conventions of Objective-C on it generates accessor methods, you can change names of the accessor methods that it will generate. You simply need to specify the name of method on attribute declaration of the property

----
@interface Person : NSObject 
@property (weak, nonatomic, getter=getLastname)NSString *lastname;
@property (weak, nonatomic) NSString *firstname;
@end
----

The properties lastname and firstname on the code sample above were declared as weak and nonatomic. You can have more than attribute specified for a property, you simply need to separate them out by commas. Also notice that the **getter** method name was specified for the lastname property. The syntax to specify accessor methods for properties is as follows

----
@property (getter=nameofaccessormethod) propertyname
@property (setter=nameofaccessormethod) propertyname
----

You can can then access the lastname property using the syntax

----
[p getLastname];
----

instead of the default

----
[p lastname];
----


