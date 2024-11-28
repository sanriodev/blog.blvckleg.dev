+++
title = 'Similiar yet different: Dart vs Typescript'
date = 2024-09-24T10:21:42+02:00
draft = false
pin = true
+++

# TypeScript and Dart: So similar yet so different

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

  ```typescript
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

```dart
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

To be fair this isn't really a problem as long as you try to write safe code but it is just as easy to write unsafe code in typescript.

Now without reading the next little segment. Could you tell where and why the code below is problematic?

```typescript
async getNames(): Promise<string[]> {
    const names = await fetch(''/someurl');

    return names;
}
```
