
== Introduction


Objective-C is one of the two programming languages we can use to create applications both for OSX and for iOS devices. The other one, the more modern one is Swift.  There are other languages that can be used to create OSX apps like Ruby and Python but Objective-C and Swift remains the dominant languages.

Objective-C is a small set of additions to the C language. It is quite different from the other CFOL (C Family of Languages) such as C++, C# and Java because unlike those languages, Objective-C was not built to supplant  the C language. It was not built from the ground up while taking inspiration from the C language. 

Objective-C is actually the C language itself but with some minor additions.   Any code written in the C language will compile and run with Objective-C. Lets take a look at a small Objective-C program.

----
/*

Compile using:
  
  clang -framework Cocoa Hello3.m

  You cannot simply compile this using -framework Foundation
  because it uses the Appkit/Appkit.h, this requires the 
  Cocoa framework

*/

#import <Foundation/Foundation.h>
#import <Appkit/Appkit.h>

int main() {

  NSAutoreleasePool * pool = [[NSAutoreleasePool alloc] init];
  [NSApplication sharedApplication];

  NSRunAlertPanel(@ "One", @ "Two", @ "Three", nil, nil);

  [pool drain];
}
----

Firstly, the source file has an extension of  .m. Objective-C programs are customarily written on a file with the .m extension rather than .c for C language nor .cpp for C++. The sample code above isn’t the simplest Objective-C program that can be written, I used to highlight the differences (and similarities) between Objective-C and the C language.

At first glance, some things you can probably make out and understand but some others look foreign to you. You may recognize the int main() function and perhapsthe #import directive but the square brackets maybe a bit jarring. We will get into to the details of an Objective-C program soon enough but for now, I would like to point out that where you see the square brackets is where Objective-C departs from the C language. The parts that use square brackets give them away as Objective-C codes.

=== Setup and Installation

We will need XCode to build Cocoa programs. We will also need to download some additional tools like the command line tools once we've installed XCode.

You can get XCode in a couple of ways. The more straightforward and simpler method of installing it is to download it from the App Store. Starting from OSX Lion, you can acquire software from the App Store. 

Launch the App Store from your desktop, launching a Finder window can do this, and then click "Applications" on the left navigation pane and you can double click on the App Store. Alternatively, you can launch Spotlight (cmd spacebar) and type "App Store". Click "Categories" then choose "Developer". XCode should be pretty visible from there.

Alternatively, you can just type "XCode" on the search bar of the App Store window

image:/images/appstore.png[AppStore]

Another way to install XCode is to go to https://developer.apple.com[developer.apple.com]. You can register for free, you will need to create an Apple ID for the registration and use that to login. You do not need to pay the 100 USD in order to register. Once registered, you can download XCode. 

It is a good idea to become a member of developer.apple.com even if you will not enroll in the developer program. It is a good source for technical documentation and information 




