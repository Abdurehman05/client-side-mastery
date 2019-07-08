# Populating React Component State from an API

At this point, you have a static application. Now it's time to implement realistic data. You will request it from your JSON API database.


## Populate the API

You are going to use `json-server` to create an API for your kennel application.

In the root directory of your project, create an `api` directory that is a sibling to `src`. Create a file in that directory named `kennel.json`.

```sh
mkdir api
touch api/kennel.json
```

Add animal data to the JSON file.

```json
{
    "animals": [
        { "id": 1, "name": "Doodles", "breed": "Poodle"},
        { "id": 2, "name": "Decker", "breed": "German Sheperd" },
        { "id": 3, "name": "Esme", "breed": "Pitbull" }
    ]
}
```

Open a new terminal window, and start your API with the following command.

```sh
json-server -p 5002 -w kennel.json
```


## Thinking in React and Querying Data from the API

Currently, this is the flow of code in your application:
1. `index.js` is the first file to load and it renders the `<Router>` with `Kennel.js`
2. `Kennel.js` renders the `<NavBar>` and `<ApplicationViews>`
3. `<NavBar>` contains links to other views
4. `<ApplicationViews>` renders routes based on the URL.
5. When viewing the Animals section, AnimalCard.js  will load up and invoke `render()`.
6. Our page displays.


## Querying the Data from your API

As we have done before, let's create a module for database calls.

## Setup

1. Create a `src/modules` directory
1. In that directory, create a file named `AnimalManager.js`

## Single Responsibility Principle

Keeping the single responsibility principle in mind, you are going to create a JavaScript module that contains **all** of the API calls. This provides flexibility for your application.

Other components, _in the future_, may need the ability to make their own API calls. You're going to eliminate the possibily of duplicate code by making a module whose sole responsibility is to interact with the API.

> AnimalManager.js

```js
const remoteURL = "http://localhost:5002"

export default {
  get(id) {
    return fetch(`${remoteURL}/animals/${id}`).then(e => e.json())
  },
  getAll() {
    return fetch(`${remoteURL}/animals`).then(e => e.json())
  }
}
```

Our `AnimalCard` does a great job at rendering a single animal. So, let's make a new file that will initiate the AnimalManager call, hold on to the returned data, and then render the **`<AnimalCard />`** component for each animal.

```sh
touch src/components/animal/AnimalList
```

The following code snippets should look familier.
Get all of the animals from the API:

```js
    AnimalManager.getAll(animalResults =>{
        console.log("animals", animalResults);
    })
```

Render each one with an **`<AnimalCard>`** component.

```js
    allAnimals.map(animal => <AnimalCard />)
```
For each animal in allAnimals, return an **`<AnimalCard>`** component.


`render()` is a built in method of React. Another built in method is `componentDidMount()`. Once a component is loaded and after render is called, `componentDidMount()` is invoked.


Let's build the **`<AnimalList >`** component.

> AnimalList.js

```js
    import React, { Component } from 'react'
    //import the components we will need
    import AnimalCard from './AnimalCard'
    import AnimalManager from '../../modules/AnimalManager'

    class AnimalList extends Component {
        //define what this component needs to render
        state = {
            animals: [],
        }

    componentDidMount(){
        console.log("ANIMAL LIST: ComponentDidMount");
        //getAll from AnimalManager and hang on to that data; put it in state
        AnimalManager.getAll()
        .then((animals) => {
            this.setState({
                animals: animals
            })
        })
    }

    render(){
        console.log("ANIMAL LIST: Render");

        return(
            <div className="container-cards">
                {this.state.animals.map(animal => <AnimalCard />)}
            </div>
        )
    }
}

export default AnimalList


```

Modify `ApplicationViews.js` to load the **`<AnimalList />`** instead of **`<AnimalCard />`**


Run the code. We have 3 cards displaying (and an error message. We will fix the error and display the correct data in the next chapter.). Take a look at the console. This is the order of the code running:

```
AnimalList: Render
AnimalList: ComponentDidMount
AnimalList: Render
```

In React, retrieving state from a remote API works in, what seems like, a counterintuitive way. React must first render the component to the DOM with any existing data (held in state), then you will request the data, setState, and then the component will invoke render again.

One of the lifecycle methods available to every React component is [componentDidMount](https://reactjs.org/docs/react-component.html#the-component-lifecycle). Straight from their docs (emphasis mine):

> `componentDidMount()` is invoked immediately after a component is mounted. Initialization that requires DOM nodes should go here. _If you need to load data from a remote endpoint, this is a good place to instantiate the network request._

The `componentDidMount()` lifecysle hook runs after the component output has been rendered to the DOM, so if your component needs API data, this is the place to do it.


## Component LifeCycle

![component lifecycle hooks](https://reactjs.org/docs/react-component.html)
![lifecycle hooks diagram](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

## Resources

* [React App Requests to JSON API](https://www.youtube.com/watch?v=vwWPM7za3Pk&list=PLhScwEnhQ-bmroyHFduwgOZ1KrdDvk_44) video series
* [React for Everyone](https://www.youtube.com/playlist?list=PLLnpHn493BHFfs3Uj5tvx17mXk4B4ws4p) video series

## Practice - NSS Kennels API

1. Create modules to query the database for employees, locations, and owners from your API.
2. Create list components to handle calling the database modules.
3. Display a static *designed* card for each section. We will get to displaying the correct data next.


> **Pro tip:** Remember to use your network tab in the Chrome Developer Tools to watch your network requests and preview the responses.

![](./images/eB9CCcrUHy.gif)


* Owners should have the `id`, `phoneNumber`, and `name` properties.
* Animals can have multiple owners.

