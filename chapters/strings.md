== NSString

You can still use C style strings inf you program if you like, but that will not be very useful especially when you start working with other classes in the Foundation and Cocoa APIs because most of them require objects. C style strings are not objects, they are just an array of characters.

Strings are very common when writing useful programs, that is why there is a Foundation class for it. The NSString.

===  Creating Strings

NSString is an object, so you can pretty much create them like any other object

----
NSString *string = [[NSString alloc] init];
----

But this is not a terribly useful thing to do because string objectss are immutable. An immutable object is one which when you create them, you can no longer modify its contents. So the code above created an empty string that you can no longer update. You won't be using that kind of code.

The most common way to create a string object is to use the string literal

----
NSString *string = @"Hello World";
----

The string literal pretty much looks like a C style string but with an `@sign` before it. The @ character is a compiler directive which tells it that we are dealing with an Objective C type, not a C style string. And because NSString are objects, we can send it messages.

----
[@"The quick brown fox" length];

NSString *string = @"Hello World";
NSLog(@"The length of the string is %@", [string length]);
----

There are other ways to create a string object, you can create them by reading them from a file on disk or a network socket. But we won't look at them right now.

=== Working with Strings

Some of the things we can do with Strings are the following

----
NSString *s = @"My String";
NSLog(s);
s = [s uppercaseString];
NSLog(s);
s = [s lowercaseString];
NSLog(s);
s = [s capitalizedString];
NSLog(s);
----

These are pretty straightforward methods, they transform the string into something else. But note that none of methods we called above modified the original string object. The methods uppercase, lowercase and capitalized that we called all resulted in the creation of a new string object. We simply reused the reference variable everytime we transformed the original string.

=== Comparing

Comparing strings cannot be performed using the equivalence operators. We cannot compare strings the way we would compare two integers.

----
NSString *a = @"Hello";
NSString *b = @"Hello";
if (a == b) {
  // Do Something
}
----

The code above will not work as expected. It will compile, sure, but what you would be comparing are memory addresses of variables a and b, not their string contents. The proper way to compare strings for equivalence is shown in the following code

----
if ([a isEqualTo: b]){
  NSLog(@"Strings match");
}
----

The isEqualTo method of the NSString performs the necessary comparisson between the two strings and returns true if the strings are identical in contents. It returns false otherwise.

=== Searching Strings

When you need to find out if a specific string is within another string, you can use the rangeOfString method

----
NSString *origstring = @"The quick brown fox jumped";

NSString *search = "brown fox";
NSRange range = [origstring rangeOfString:search];
if (range.location == NSNotFound) {
  // search is not within originstring
}
else {
  // found it
}
----

NSRange is not an object, it is a struct that is why we did not have to use the pointer operator for its variable. The rangeOfString method of the NSString returns an NSRange structure. NSNotFound is one the constants defined in the Foundation toolkit.

If you need to perform an case insensitive search for strings, use the following code

----
NSRange range = [origstring rangeOfString : search
                 options:NSCaseInsensitiveSearch]
----

The NSCaseInsensitiveSearch is just one of the options you can pass on rangeOfString method.

=== Adding Strings

This is tricky and probably frustrating if you are coming from a more modern language like Java or C# because concatenating strings in Objective C is not as simple as string1 + ~string2.

----
NSString *a = @"Hello ";
NSString *b = @"World";

NSString *c = a + b;
----

The third line of code above is illegal to do in Objective C. You cannot add strings this way. You could add the strings a and b with

----
NSString *c = [a stringByAppendingString: b];
----

or using this code

----
NSString *c = [NSString stringWithFormat: @"%C %C", a,b];
----

==== NSMutableString

When you need a quick way to add strings, using the stringByAppendingString or the stringWithFormat method may suffice. But these are less than ideal if you need to concatenate a lot of strings. Recall that NSString is an immutable object. Everytime you append a string, you are creating a new object, and that is expensive. It will hurt the performance of the application.

Another way to concat strings is to use the mutable version of NSString.

----
NSMutableString *c = [NSMutableString stringWithString: a];
[c appendString:b];
----

=== Working with AlphaNumeric numbers

There maybe times when you need to extract numeric values from strings, e.g. if you read the string from a configuration file and you need to work with the values inside the file.

----
NSString *a = @"1";
NSString *b = @"2.0";

int i = [a intValue];
float f = [b floatValue];
----
