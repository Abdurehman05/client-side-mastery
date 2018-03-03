# Objects

Your job as a web application developer will often be to take a process that is done by an organization and build an application that does the same thing.

For example, a small company may use a colossal Excel spreadsheet to track all of their business expenses, and they email it around to each other, and eventually the accounting department gets it. They eventually realize that this process is rife with opportunities for human error, and the file becomes so large that it can't be emailed any more.

They hire you to build an application to track expenses.

Let's examine some of the things that you will be representing in code.

1. An employee who needs to track expenses
1. The expense itself (date, business, dollar amount, purpose, etc.)
1. An employee who needs to verify that it's a valid expense
1. An expense report

These things are objects we need to represent in code. Each object has properties that define it.

* An employee has a first name, last name, a role, and an account number
* The expense has a date, location, dollar amount, and purpose
* The expense report has a date, and a collection of expenses

So objects are things in the physical world that have properties that we want to represent in the application. A simple representation of an expense resource, in code, would look like this.

```js
const expense = {
    dateCreated: "01/01/2017",
    location: "Bob's Burgers",
    dollarAmount: "14.34",
    purpose: "Lunch with very important client",
    code: "1001A"
}
```

Here's a representation of a person who is an employee as a JavaScript object.

```js
const employee = {
    firstName: "Michael",
    lastName: "Tambornino",
    role: "Sales Rep",
    accountNumber: "1-8349058340"
}
```

Here's a representation of an expense report resource. It's a collection of expenses, with a timestamp of when the report was generated.

```js
const expenseReport = {
    dateCreated: "02/01/2017",
    expenses: [
        {
            amount: 14.34,
            dateCreated: "01/01/2017",
            vendor: "Bob's Burgers",
            code: "1001A"
        },
        {
            amount: 51.03,
            dateCreated: "01/03/2017",
            vendor: "Sunoco",
            code: "2213D"
        },
        {
            amount: 7.22,
            dateCreated: "01/07/2017",
            vendor: "Taco Bell",
            code: "1001A"
        }
    ]
}
```

## Object.create()

You can also use `Object.create()` to initialize an object. Let's rewrite the `expense` object with this method.

```js
const expense = Object.create({}, {
    dateCreated: {
        enumerable: true,
        writable: false,
        value: "01/01/2017"
    },
    location: {
        enumerable: true,
        writable: false,
        value: "Bob's Burgers"
    },
    dollarAmount: {
        enumerable: true,
        writable: false,
        value: 14.34
    },
    purpose: {
        enumerable: true,
        writable: false,
        value: "Lunch with very important client"
    },
    code: {
        enumerable: false,
        writable: true,
        value: "1001A"
    }
})
```

You might be thinking this right now.

> "Um, this seems like way too much code to write when I can just do it the other way. Why would I ever do this?"

You use `Object.create()` when you want to have fine-grained control over the state and behavior of an object. Using simple object create with `{}`, you don't have much control. You'll learn more about this as your continue through the course.

For now, using simple object creation is fine.

> **Googling Tip**:
>
> [Writable and enumerable properties](http://bfy.tw/Gu9L)
>

## Object State

Ok, I mentioned "object state" above, and you might not understand what I mean by that. Stated as simply as possible, when you hear the words _object state_, it means that you want to know what the values of an object's properties are _**at this moment in time**_. That means an object's state can change, i.e. a property's value can be reassigned.

```js
const employee = {
    firstName: "Michael",
    lastName: "Tambornino",
    role: "Sales Rep",
    accountNumber: "1-8349058340"
}
```

Those property value define the state of this employee that we're representing in code.

```js
employee.role = "Sales Manager"
```

I just changed the state of the JavaScript object that is representing Michael Tambornino.

## Videos to Watch

1. [What's an Object in JavaScript?](https://www.youtube.com/watch?v=8iXoWC9XcU8)
1. [Objects and Prototypes In-depth 02 - Objects Basics](https://www.youtube.com/watch?v=yHkPHteghSc&index=2&list=PLqq-6Pq4lTTaflXUL0v3TSm86nodn0c_u)
1. [Objects and Prototypes In-depth 03 - Creating Objects](https://www.youtube.com/watch?v=25wlojU2mHg&index=3&list=PLqq-6Pq4lTTaflXUL0v3TSm86nodn0c_u)
1. [Object Keys](https://www.youtube.com/watch?v=cEVnlKgySsg)
1. [Javascript - How to Access Javascript Object Property Names Using A `for in` LOOP](https://www.youtube.com/watch?v=fDBuAnu0fvo)
1. [Modular Javascript - Prototypal Pattern vs Classical OOP in JS](https://www.youtube.com/watch?v=doXpW5AD60Q)
1. [Object.create - Object Creation in JavaScript P6 - FunFunFunction #57](https://www.youtube.com/watch?v=CDFN1VatiJA)

## Practice

You have volunteered your time to a local political candidate, Elizabeth Sanger, who wants to become a US representative in Congress for your district. Unfortunately, the team discovered that you're a software developer, so they have begged you to build an application that lets them track volunteers, and store information about Elizabeth and her campaign.

Your job is to define the different objects and resources, their structure, and the corresponding properties for each, to represent the following information about Elizabeth's campaign.

1. Her congressional district (_you can use yours here_)
1. Her platform statements for the following issues.
    1. Taxes
    1. Jobs
    1. Infrastructure
    1. Health care
    1. Crime and enforcement
1. URL for donation form
1. Calendar of events
1. Volunteer information
    1. Name
    1. Address
    1. Email
    1. Phone number
    1. Availability
    1. What activities each one is willing to do (e.g. answering phone calls, taking polls, etc.)
1. Biography
1. Image gallery
    1. Head shot
    1. Picture of family
    1. Picture of constituents
1. Mission statement
1. URL for registering to vote

## Challenge

After you have defined all the objects for representing the data, write a corresponding function for each one whose purpose is to change the state of the object. Then invoke each function, and pass a reference to the correct object to it and modify a property.

Example:

```js
function changeBookState (anyBookObject, thePropertyToChange) {
    ...
}

const jest = {
    title: "The Infinite Jest",
    author: "David Foster Wallace",
    publishDate: "1 February 1996"
}

const lamb = {
    title: "Lamb: The Gospel According to Biff",
    author: "Christopher Moore",
    publishDate: "25 May 2004"
}
```

## Advanced Challenge

Use `document.createElement()` to build an `<article>` element representing Elizabeth. The `id` property of the element should be her congressional district, and its content should be her platform data. Once the element has been created, insert it into the DOM.
