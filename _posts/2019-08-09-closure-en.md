---
layout: post
title: Closures, Partial application and currying (Part 1/2) (Swift)
author: Damien
date: '2019-08-13'
category:
    - tutorial
summary: Introduction of closures
thumbnail: blog/yoda_closures.jpg
---


## Introduction

Are you afraid by this title?
Do not go away and take time to demystify these concepts, understanding how it works.

## Welcome to the fantastic world of closures

Look at [Swift](https://docs.swift.org/swift-book/LanguageGuide/Closures.html) documentation and what we have about closures.

> "Closures are self-contained blocks of functionality that can be passed around and used in your code."

Is not it like a toolbox?
In this box, we could have the tools, use them to get/modify the result.
This seems a bit general, however the documentation keeps going:

> "Closures can capture and store references to any constants and variables from the context in which they are defined."

I think we need to clarify some notions to have a better understanding of the theory.
A closure is like an environment, a memory which keeps values that we have defined in.
Variables are only known in the scope where you declared them.
For instance:

```swift
func foo() {
  let x = 3
  // 3
  print(x)
}
// Use of unresolved identifier 'x'
print(x)
```

In this example, we have a function "*foo*" which declare a variable a variable "*x*".
This variable is only known inside the function.
When we get out, we do not know "*x*" anymore.
We have the same behavior for a closure.
Hence, we have variables, and functionalities that are just functions.

Later, we will see more information about closures.
For now, we focus on how we can use them in Swift.

What is Swift tells us about the syntactic representation of closures?

> "Closure expression syntax has the following general form:"

```swift
{ (parameters) -> return type in
    statements
}
```

One of the main defaults which is difficult with closure is its syntax.
We need to identify each component to understand how it works.  

Let's take an example:

```swift
let clos = {(x: Int, y: Int) -> Int in
  return x + y
}

// Print 42
print(clos(20,22))
```

We can see a closure as a function without a name, calling from the variable which is bound to "*clos*".
Same as a function, closures have inputs, its type and the output type.
The key word "*in*" replaces the separation between the signature and the body.
The key word "*return*" is the same as usual.
If you decide to call this closure, you can make it use the name "*clos*" as a function name.
You add inputs and you are ready to go!
You easily understand that we just make an addition, but there is nothing exciting.

Let's look at another example, using a closure in function inputs.

```swift
func apply(_ x: Int,_ y: Int, op: (Int, Int) -> Int) -> Int {
  return op(x,y)
}
```

The "*apply*" function takes two Integer and will apply an operation on them.
Do you understand the signature "*op: (Int, Int) -> Int* "?
It means that the label "*op*" is waiting a function or a closure which takes two Integers as inputs and return an Integers.
Just a coincidence (or not), our last closure has the same signature.
So, we can call our function like this:

```swift
// Retourne 42
print(apply(20, 22, op: clos))
```

Are you beginning to understand?
We have a way to make it easier, without declaring the closure before:

```swift
apply(20, 22, op: {(x: Int, y: Int) -> Int in
  return x + y
})
```

Any functions/closures having the signature "*(Int, Int) -> Int* " can be used as a parameter for "*op*".

Do you want syntactics tips to help you?
Let's go !

```swift
apply(20, 22, op: {(x, y) in
  return x + y
})
```

(Traduction en cours)

Did you see that we omit type parameter?
Why does it work so?
Thanks to the type [inference](https://en.wikipedia.org/wiki/Type_inference)! (Not a black magic)
At the time we declared the function "*apply*", Swift already know the waiting type, seeing that we gave them at the function declaration.
Swift automatically makes the association, and it is wonderful no?
However, we are far to see all the things that we can do again!
Next!

```swift
apply(20, 22, op: {(x, y) in
  x + y
})
```

Does not need a return?
Indeed, when there is any ambiguity about the return value, Swift understands that the operation "*x+y*" is just the return value.
After the "*in*" parameter, you can add as many parameters as many codes as needed.
It is not finished yet, we have a way to do better!

```swift
apply(20, 22, op: {$0 + $1})
```

Have I been too far?
Test from yourself and you will see that it works!
In this piece of code, we do not distinguish the signature and the body of our closure.
From the same principle of the type inference, Swift can guess what we are waiting.
How can we get value parameters?
"*$0* " et "*$1* " are generic names that Swift use by default, taking input values.

Are you a bit lost?
Take back the signature "*op: (Int, Int) -> Int* ".
Here "*$0*" is just the first Integer and "*$1*" the second.
The order of input parameters corresponds to the order of the parameters in the signature function.
Therefore, Swift has binding values with predefined names.
Last syntactic thing before I left you with closures!

```swift
apply(20, 22) {$0 + $1}
```

When the last parameter of your function is a closure, you can just call like this.
Do not even need to give the parameter names, only need to write a closure between curly braces.
All the last closures work with this syntax.
Nevertheless, do not forget the readability of your code!

This part is now ended, keep going on the next part of partial application!

[Partie 2: Partial application and currying](../../../2019/08/15/partial-application-en.html)
