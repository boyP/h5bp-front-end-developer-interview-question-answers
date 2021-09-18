# Explain how `this` works in JavaScript

## What is `this`?

`This` references the object that is currently calling the function.

What does that mean? Show an example!

```
const counter = {
    count: 0,
    next: function() {
        console.log(this)
        this.count++
        return this.count
    }
}

counter.next() // Returns 1
```
The `console.log(this)` returns the following
```
{ count: 0, next: [Function: next] }
```
Here, `this` references the `counter` object because `next()` was invoked as a method of the counter object.

With that you should have a general idea of how `this` works. Let's dive a little deeper now.

# The different ways of changing this
There are 4 ways to invoke a function. Directly, indirectly, as a method and as a constructor. The value of `this` changes based on what method you use. 

## Direct invocation
```
function example() {
    console.log(this === window) // true
}

example()
```
Calling a function like we normally do binds the global object (window on web) to the value of `this`. 

What you need to remember is that all functions invoked like `example()` has its `this === window` (on the web, in nodeJS it's the global object). 


Here's a [playground](https://codepen.io/boyP/pen/BaZJGZz?editors=1011) of different scenarios to try for yourself.

## Method invocation

A method is defined as a function bound to the field of an object. In the following example, `next` is considered a method of the object `counter`. 

```
const counter = {
    count: 0,
    next: function() {
        console.log(this)
        this.count++
        return this.count
    }
}

counter.next() // Returns 1
```

The `console.log(this)` returns the following
```
{ count: 0, next: [Function: next] }
```
Here, `this` references the `counter` object because `next()` was invoked as a method of the counter object. 

Here's a [playground](https://codepen.io/boyP/pen/mdwpQzP?editors=0011) of different scenarios to try for yourself.

## Constructor invocation

A constructor is defined using the `constructor` keyword and is only invoked using the `new` keyword. 

```
class Dog {
  constructor(breed) {
    this.breed = breed
    console.log(this, 'this refers to the specific instance of Dog')
  }
  
  getBreed() {
    return this.breed
  }
}

const dog = new Dog('golden doodle')
console.log(dog.getBreed()) // Prints golden doodle
```

Here's a [playground](https://codepen.io/boyP/pen/gORoZXX?editors=0010) of different scenarios to try for yourself.

## Indirect invocation

There are more ways to execute a function than `myFunc()`. Since functions are objects in javascript, it's prototype has the methods `call` and `apply` that can be used to execute the function.

What's special about `call` and `apply`? The first argument is the value used for `this`. 

You can also use the `bind` method to directly set `this`.

```
function getBreed(prefix) {
  return prefix + this.owner + "'s" + " golden doodle"
}

console.log(getBreed.call({owner: 'patrick'}, 'I am ')) // "I am patrick's golden doodle"
console.log(getBreed.apply({owner: 'mark'}, ['I am '])) // "I am mark's golden doodle"

const getMyBreed = getBreed.bind({owner: 'kevin'})
console.log(getMyBreed('I am ')) // I am kevin's golden doodle
```

Here's a [playground](https://codepen.io/boyP/pen/bGRLxaL?editors=0011) of different scenarios to try for yourself.

## Arrow functions
There are also two ways of defining a function. The regular `function` keyword and the ES6 `arrow functions`. The value of this depends on which one you use.

As we saw above, the value of `this` when using the `function` keyword depends on how the function is invoked.

However, functions defined using `=>` inherit the value of `this` from it's parent's context.

That's right, the value of `this` does not depend on how it's invoked when using an arrow function. The value of `this` behaves similarly to block scoped variables.

```
const doesThisEqualWindowObjectArrowFunc = () => {
  return this === window // true
}
```

```
const dog = {
  breed: "golden doodle",
  bark: () => {
    console.log(this === window, '`this` is equal to the window because arrow functions set `this` to the `this` in the parent scope')
    return "Woof!"
  },
  fetch: function() {
    const toy = {
      type: "ball",
      roll: () => {
        console.log(this === dog, 'this should equal dog object because we used an arrow function')
      },
      bounce: function() {
        console.log(this === toy, 'this should equal toy object')
      }
    }
    toy.roll()
    toy.bounce()
  }
}
```

## Strict mode
In strict mode, the value of `this` in functions invoked directly will be undefined instead of the window object.

```
function myStrictFunction() {
  'use strict'
  return this
}

console.log(myStrictFunction() === undefined) // true
```

Functions defined using arrow functions are not affected by strict mode. 

```
const myStrictArrowFunction = () => {
  'use strict'
  return this
}

console.log(myStrictArrowFunction() === window) // true
```


# Common mistakes with using `this`

The most common is not tracking the value of `this` when passing functions as values. 

```
dog.prototype.bark = function() {
  this.timer = setTimeout(function() {
    this.clearThroat();
  }, 1000);
};
```

This anonymous function is invoked by window.setTimeout. Therefore, the value of `this` in `this.clearThroat` is the window object and the function `clearThroat` doesn't exist on the window object. 