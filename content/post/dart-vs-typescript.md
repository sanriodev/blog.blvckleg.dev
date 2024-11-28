+++
title = 'Similiar yet different: Dart vs Typescript'
date = 2024-09-24T10:21:42+02:00
draft = false
pin = false
+++

# TypeScript and Dart: Taking a look!

## Introduction

In the rapidly evolving world of web and mobile development, choosing the right technology stack is crucial. TypeScript and Dart (likely with Flutter) are two powerful and popular languages that developers often consider. In this blog I will compare my expiriences with TypeScript and Dart, focusing on key features such as async/await, interfaces and abstract classes, null safety, and strong types.

## Overview of TypeScript

### What is TypeScript?

TypeScript is an open-source language developed by Microsoft. It is a superset of JavaScript that adds static types and static type checking, making it easier to write and maintain large-scale applications.

There is an ongoing debate between Typescript and Javascript users to this day, wether Types help with readability or make it worse when doing too much "type gymnastics".

However in my expirience. Typescript is one of those things you think you don't need but as soon as you try it, you can never go without.

### Key Features of TypeScript

- **Static Typing**: TypeScript introduces static types to JavaScript, helping catch errors early in the development process.
- **Compatibility**: TypeScript is fully compatible with JavaScript, allowing developers to gradually adopt it in existing projects.
- **Tooling Support**: TypeScript has excellent tooling support, including autocompletion, refactoring, and navigation.
- **Large Ecosystem**: TypeScript benefits from the enormous JavaScript ecosystem, including libraries, frameworks, and tools.

## Overview of Dart

### What is Dart?

Dart is an open-source, general-purpose programming language developed by Google. It is designed for building web, server, desktop, and mobile applications. Dart is particularly known for its use in the Flutter framework, which allows developers to create natively compiled applications for mobile, web, and desktop from a single codebase.

### Key Features of Flutter

- **Strongly Typed**: Dart is a statically typed language, which helps catch errors at compile time but still allows dynamic type declaration.
- **Asynchronous Programming**: Dart supports async-await, making it easier to handle asynchronous operations.
- **Null Safety**: Dart has sound null safety, which helps prevent null reference errors by ensuring that non-nullable variables cannot be assigned a null value.

## Comparison

### Async/Await

Both TypeScript and Flutter support async/await for handling asynchronous operations.

- **TypeScript**: Async/await in TypeScript is built on top of JavaScript's promises, making it straightforward to use for developers familiar with JavaScript.

```typescript=
 async function fetchData() {
   try {
     const response = await fetch('https://api.example.com/data');
     const data = await response.json();
     console.log(data);
   } catch (error) {
     console.error('Error fetching data:', error);
   }
 }
```

 - **Dart**: Async/await in Dart feels very similiar, making it also very straightforward to use for Typescript or Javascript developers.

```dart=
 Future<void> fetchData() async {
   try {
     final response = await http.get('https://api.example.com/data');
     final data = jsonDecode(response.body);
     print(data);
   } catch (error) {
     print('Error fetching data: $error');
   }
 }
```
### Null safety - Typescripts biggest enemy

So in order to fully cover all the pros and cons of dart's **sound null safety** I would probably have to be a lot better at my job and/or intelligent because having this conversation to the fullest could take a long while.

There are plenty of benefits to **sound null safety**. That is without a doub a fact, especially when it comes to how error prone an application can get when you happen to forget that in typescript a value can be **null** or **undefined** at runtime.

Of course typescript has some form of null safety. Typescript will helps prevent null-related errors with static type checking and so on but it cannot guarantee that a value isn't null or undefined during runtime.

To be fair this isn't really a problem as long as you write safe code but it is just so very easy to write unsafe code in typescript.

#### The problem with typescript and null safety
Now without reading the next little segment. Could you tell where and why the code below is problematic?

```typescript=
async getUserNamesToUpperCase(): Promise<string[]> {
  const users = await fetch('/users');

  return users.map((user) => user.name.toUpperCase());
}
```

Well there could be plenty or errors hidden in that function, without typescript needing to tell you.

1. We as a developer assume that the `fetch()` call here will return an array of some sort of **User objects**. However in reality we can never be certain of that. Assuming `fetch()` returns `undefined` or `null`: `return users.map();` would throw an error since **users is not an array** and it does therefore not implement the `map()` method. Meaning we are trying to call a function signature that is not defined.
```typescript=
const users = null;
//throws a type error
users.map();
```
2. What if `fetch()` returns an empty array.... well that would be different from the above mentioned case of course but it would still crash our program.. just in a different way... If `fetch()` returns an empty array it does in fact implement the `map` method but when trying to access the property `name` for each element (none) in the array it would still throw an error since you obviously can't access a property of `undefined`
```typescript=
const users = [];
//throws a type error
users.map((user) => user.name)
```
3. let's assume `fetch` does indeed return an array of users... for our example containing one user object... but the property `name` is `null` like so. That would again... throw an unexpected error. When we are trying to call `user.name.toUpperCase()`since `null` or `undefined` is not a `string` and does not implement our called function.
```typescript= 
const user = {id: 1, name null};
//throws a  type error
user.name.toUpperCase();
```

#### How dart makes this safer with sound null safety

The way dart makes code like this safer is ***sound null safety***. Meaning no variable that is not explicitly declared us `nullable` **cannot be null**.

```dart=
//this is not possible since the type String is not nullable
//this variable needs to be initialized with a non null value
const String myNullString;

//to declare a variable with the value null you NEED to mark it as nullable

const String? myNullString;
//or
const String? myNullString = null;
```

Now whenever you want to use that variable in any way you either need to mark it's usage as nullable as well `(?)` or mark it as null-safe `(!)`.

for example:

```dart=
void printString(String input) {
print(input);
}

const String? testString = 'test';
/*
will not compile
we cannot possibly call our function with this. 
even though our string has a non null value the type String indicates that it could be null and will therefore not compile
*/
printString(testString);
```

to resolve this issue we can either refactor our function.
So that it is able to take a String? that could possibly be null.

Or we have to check the value of our variable and tell the compiler
that it is indeed not null


- option 1: function expects parameter that can be null 
```dart=
void printString(String? input) {
  if(input != null){
  print(input);
  }
}
```

- option 2: check the variable first to make sure it is not null
```dart=
void printString(String input) {
print(input);
}

const String? testString = 'test';

if(testString != null) {
//tell the compiler that we guarantee null safety with (!)
printString(testString!);
}

```

## Closing remarks

In conclusion, while both TypeScript and Dart support null safety, Dart enforces it by default, requiring developers to handle nullable values explicitly at compile time.

This approach significantly reduces the likelihood of runtime errors caused by null or undefined values. 

TypeScript, on the other hand, offers flexibility but relies on developers to adopt strict null checks and write defensive code, which can lead to oversights and unexpected runtime exceptions.

Therefore, Dart's stricter null safety makes it inherently safer for handling potential null values, especially in larger, more complex projects.
