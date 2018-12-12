# Object Methods, Constructing, and Destructuring

## Why are you Learning This?

Since objects are the core concept behind Object Oriented Programming - what you are learning here at NSS - this chapter is going to prepare you for working with object methods which allow you to search and manipulate object with more ease.

You are also going to learn a new kind of `for` loop in JavaScript. It's the [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) loop.


## Object Methods

Objects have some helpful methods that will allow you to iterate the collection of key/value pairs.

You are going to start with a simple object representing a Ford Mustang.

```js
const mustang = {
    color: "Midnight Blue",
    year: 1976,
    length: 120,
    width: 62,
    height: 47
}
```

You have already learned how to access object properties with dot notation and square bracket notation. So if you wanted to get the color of your Mustang, you could write any of the following code.

```js
// Dot notation
console.log(mustang.color)

// Square bracket notation
console.log(mustang["color"])

// Dynamic square bracket notation
const colorProperty = "color"
console.log(mustang[colorProperty])
```

Now you're going to learn how to work with all of the keys and values. You are tasked with taking an array of objects, and displaying all of the values in each one. For example, you work for a car dealership and the manager, Randall, wants to have a report that quickly shows the details of each car. Here's your array of cars.

```js
const allCars = [
    {
        model: "Mustang",
        color: "Midnight Blue",
        year: 1976,
        length: 120,
        width: 62,
        height: 47
    },
    {
        model: "Xterra",
        color: "Forest Green",
        year: 2011,
        length: 144,
        width: 71,
        height: 55
    },
    {
        model: "Thunderbird",
        color: "Red",
        year: 2005,
        length: 115,
        width: 58,
        height: 42
    },
    {
        model: "Suburban",
        color: "Grey",
        year: 2015,
        length: 149,
        width: 73,
        height: 58
    }
]
```

You want to produce the following report. It's not pretty, but it shows all the information the manager needs.

![](./images/carlot.png)

Since you just need the values of each object, you can use the `Object.values()` method. Here where you would start.

1. Iterate the array of cars with `forEach()`.
1. Use `Object.values()` to iterate the values of each car.

For this to work, you will need an HTML element in your `index.html` with an id of `app`.

```js
const outputElement = document.querySelector("#app")

// Iterate the array of cars. Individual objects stored in `car`.
allCars.forEach(car => {

    // Iterate all of the values of the current car
    for (const value of Object.values(car)) {
      outputElement.innerHTML += `<div>${value}</div>`
    }
})
```

At this point, you proudly show Randall the results of your hard work, and as usual, changes are immediately requested.

> **Randall:** "Ok this is good, but those numbers listed there aren't helpful unless I know what they mean. Like, is that the horsepower, engine size, and wheel base? Can you fix that?"

Of course you can! It's time for you to list all of the properties that are on each object at the top of the page so that Randall knows which properties are on each of the object.

![car properties](./images/carlot-properties.png)

In order to do that, you can use the `Object.keys()` method.

```js
/*
    Just get a reference to the first object since
    they all have the same properties
*/
const firstCar = allCars[0]

// Now iterate its keys
outputElement.innerHTML += "<h1>Properties</h1>"
for (const key of Object.keys(allCars[0])) {
  outputElement.innerHTML += `<div>${key}</div>`
}

// Iterate the array of cars. Individual objects stored in `car`.
outputElement.innerHTML += "<h1>Car List</h1>"
allCars.forEach(car => {

    // Iterate all of the values of the current car
    for (const value of Object.values(car)) {
      outputElement.innerHTML += `<div>${value}</div>`
    }
})
```

> **You:** "Here you go Randall. Now you can see all of the properties that are listed for each car. In fact, if we add more properties later, this page will automatically show those!"

You're very proud of yourself.

> **Randall:** "Um, that's cool and all, but eventually we're going to have more than 4 cars to sell and if I'm scrolling and I forget what the properties are, that means I'll have to scroll all the way back up to the top to remember. Can't you just list the properties on each car as well as the values?"

Time to refactor!

To achieve this new requirement from Randall, you can use the `Object.entries()` method. This method returns an array, with the first item being the key name, and the second item being the value.

Here's a simple example.

```js
const bug = {
    flying: true,
    legs: 6
}

for (const entry in Object.entries(bug)) {
    console.log(entry)
}

> [ 'flying', true ]
> [ 'legs', 6 ]​​​​​
```

```js
outputElement.innerHTML += "<h1>Car List</h1>"
allCars.forEach(car => {
    outputElement.innerHTML += "<hr/>"

    for (const entry of Object.entries(car)) {
    outputElement.innerHTML += `<div>${entry[0]}: ${entry[1]}</div>`
}
})
```

Now your report meets all of Randall's basic requirements.

![](./images/carlot-entries.png)


> **Lightning Exercise 1:** Create an object to represent your favorite dinner item (e.g. meatloaf, spaghetti, fried fish, gumbo). Each object should have a `name` property, but you can add any other properties that you like, such as size, weight, ethnicity, vegetarian boolean.

> **Lightning Exercise 2:** Output all of the key/value pairs into the DOM inside an `<article>` element with a class of `food`.

## Rest and Spread Operators



