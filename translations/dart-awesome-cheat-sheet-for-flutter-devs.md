# Dart Awesome Cheat Sheet for Flutter Devs

[![Temidayo Adefioye](https://miro.medium.com/fit/c/48/48/1*EHweqnww2uWcqa7d_90mZg.jpeg)](/@temidjoy?source=post_page-----d8cb52c978e1----------------------)

![](https://miro.medium.com/max/1766/1*oikrjJQi1b5JTpUM0LsQxw.png)

If you are really interested in cross-platform development, you know about a new kid in a block — Flutter. Google’s new mobile app SDK which allows developers to write beautiful, natively compiled applications for mobile, web, and desktop right from a single codebase. There has been an increasing number of tech companies who have decided to develop their multi-platform apps using flutter framework. As a result, the demand for flutter developers in the tech industry has drastically increased over the past 2 years. In order to meet this demand, it’s essential we have a huge community of passionate flutter devs willing to develop world class apps without any sort of programming barriers.

In this article, I will share with you **4 major dart cheats** that will help you quickly get started with flutter development.

# String interpolation

Every language has its own way of interpolating two or more words or characters. In dart, you can put the value of an expression inside a string as follows:

``` dart
int x=6;
int y=2;
String sum = '${x+y}';          // result is 8
String subtraction = '${x-y}';  // result is 4
String upperCase = '${"hello".toUpperCase()}'; // result is HELLO
String concatXY = '$x$y'; // result is '62'
```

# Functions

Dart lang is an OOL(Object-oriented language). In this language, functions are objects and have a type, Function. This implies that functions can be assigned to variables or passed as args to other functions. Interestingly, you can also call an instance of a class as if it were a function. That’s awesome, right?

``` dart
String fullName() {
    String firstName = "Temidayo";
    String lastName = "Adefioye";
    return '$firstName $lastName'; // returns 'Temidayo Adefioye'
}
int length(String text) {
    return text.length; // returns length of text
}
```

The above function can be rewritten in a more concise way:

``` dart
    int length(String text) => return text.length; // returns length of text
```

The above approach is applicable where functions contain just ONE expression. This is also referred to as shorthand syntax.

# Null-aware Operators

Handling null exceptions in app development is very essential, as this allows you to create a seamless experience for your app users. Dart provides you with some handy operators for dealing with values that might be null. One is the ??= assignment operator, which assigns a value of a variable only if that variable is currently null:

``` dart
int x; // The initial value of any object is null
x ??=6;
print(x); // result: 6

x ??=3;
print(x); // result is still 6

print(null ?? 10); // result: 10. Display the value on the left if it's not null else return the value on the right
```

# List Arrays

Perhaps the most common collection in nearly every programming language is the array or ordered set of objects. Please note that Dart arrays are Lists.

``` dart
var numList = [1,2,3,5,6,7];
var countryList = ["Nigeria","United States","United Kingdom","Ghana","IreLand","Germany"];
String numLength = numList.length; // result is 6
String countryLength = countryList.length; // result is 6
String countryIndex = countryList[1]; // result is 'United //States'
String numIndex = numList[0]; // result is 1

countryList.add("Poland"); // Adds a new item to the list.

var emailList = new List(3); // Set a fixed list size 
var emailList = new List<String>(); // instance of a list of type //String
```

Would that be all?

![](https://miro.medium.com/freeze/max/30/1*uGJysDvESMupwwDUQJJz0A.gif?q=20)
![](https://miro.medium.com/max/650/1*uGJysDvESMupwwDUQJJz0A.gif)

No!

I have carefully curated an awesome cheat sheet for developers who are interested in developing apps using flutter and dart.

You can find this cheat sheet [here](https://github.com/Temidtech/dart-cheat-sheet)

In the coming weeks, the sheet will be updated with more awesome dart cheats.

Happy Fluttering!