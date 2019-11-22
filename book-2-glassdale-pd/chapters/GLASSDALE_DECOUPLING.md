# A Discussion about Loose Coupling

The idea of Coupling in complex software is an important factor. Coupling is defined most generally as how many components know about the existance of each other. The higher that number, the higher your overall coupling is in an application. You want that number to be as low as possible.

Consider the following example. You have two modules in your system responsible for rendering HTML components.

## Tightly Coupled Components

```html
<body>
    <main id="appContainer" class="container">

        <section class="friends"></section>

        <section class="messages"></section>

    </main>

    <script type="module" src="./scripts/main.js"></script>
</body>
```

### List of all of a user's friends

```js
// FriendList.js
import { useFriends } from './FriendProvider.js'

const contentTarget = document.querySelector(".friends")

const FriendList = () => {
    const appStateFriends = useFriends()

    const render = friendCollection => {
        contentTarget.innerHTML = `
            ${
                friendCollection.map(friend => {
                    return `
                        <input class="friend" type="checkbox" value="${friend.name}">
                        ${friend.name}
                    `
                })
            }
        `
    }

    render(appStateFriends)
}
```

### List of messages from only the selected friend in the list

```js
// MessageList.js
import { getMesssageByFriend } from "./MessageProvider.js"

const eventTarget = document.querySelector(".friends")
const contentTarget = document.querySelector(".messages")

const MessageList = () => {
    const appStateFriends = useFriends()

    // Listen for when a friend is selected
    eventTarget.addEventListener("change", changeEvent => {
        // Make sure it's the change event of the friend checkbox
        if (changeEvent.target.classList.contains("friend")) {

            // Get messages for friend and render the list of messages
            const friendName = changeEvent.target.value
            const messages = getMessagesByFriend(friendName)
            render(messages)
        }
    })

    const render = messageCollection => {
        contentTarget.innerHTML = `
            ${
                messageCollection.map(friend => {
                    return `
                        <input class="friend"
                               type="checkbox"
                               value="${friend.name}">
                        ${friend.name}
                    `
                })
            }
        `
    }
}
```

Unfortunately, these two components are now tighly coupled. What couples them?

1. **`MessageList`** knows of the existence of the DOM element with `friends` class name.
1. It also knows about DOM elements with the `friend` class name and that they emit the "change" event.
1. It knows that those DOM elements have a `.value` property.

Ok... so what? Why is this bad?

Imagine that the **`FriendList`** component goes through a major refactor.

* What if it is changed to no longer render to the DOM element with the class of `friends`, but now to one with a class of `friendList`.
* What if the input elements get a new class name of `friend--user`?
* What if the friend elements are changed to a control that emits a "click" event instead of a "change" event?

Any of those change in the **`FriendList`** component will then immediately break the functionality of the **`MessageList`** component. That means a refactor in one component forces a refactor of another component.

Professional developers want to avoid that. This is one of the underlying principles to the Single Responsibility Principle. Any change to a component's functionality should only require changes to that component, and no others.

## Event Based Programming

One common way around this problem is to have the components talk to each other. Think of the components as a bunch of old friends gathering at a party. They can talk to each other and let everyone at the party know what happens to them.

Now, when one component talks, not every other component has to listen. When you are at a party, there are a few key people, or a few key topics you want to catch up with. So you choose to listen to only the things that are _important_ to you, and you choose to **not** listen to people/subjects that are not important to you.

You can do the same thing in your application. Components can broadcast custom messages, and other components that care about certain kinds of messages can listen for them. In JavaScript, you can use something called a Custom Event.

In your application, your components will use a common system for talking to each other. That will be the only type of coupling between components: a shared language. Components will be self-contained and will control everything that happens to them and simply dispatch messages in the agreed-upon format to an agreed-upon location.

In this application, the agreed-upon location will be the top-most DOM element.

```html
<main id="appContainer" class="container">
```

That is going to be the event hub because it's the element in which all the other ones will be contained.

## Loosely Coupled Components

### Friend Was Selected

The first step in this process is to have the friend list have control over what happens with itself, and also control what information is wants to share with other components.

```js
// FriendList.js
import { useFriends } from './FriendProvider.js'

/*
    CHANGE: The event target is now the main container, or the Event Hub
*/
const eventTarget = document.querySelector(".appContainer")
const contentTarget = document.querySelector(".friends")

const FriendList = () => {
    const appStateFriends = useFriends()

    /*
        CHANGE: The event listener for when the user selects a
                friend now exists in the FriendList component.
                This makes more sense.
    */
    eventTarget.addEventListener("change", changeEvent => {
        if (changeEvent.target.classList.contains("friend")) {
            const selectedFriend = changeEvent.target.value

            /*
                NEW: This component is now going to "talk" by
                     sending a message to the Event Hub. Which
                     component would want to "listen" to a message
                     like this?
            */
            const message = new CustomEvent("friendSelected")

            /*
                Any component that thinks this message is important
                would also care which friend was chosen. You can pass
                that information along in the event like this.
            */
           message.detail.selectedFriend = selectedFriend
        }
    })

    const render = friendCollection => {
        contentTarget.innerHTML = `
            ${
                friendCollection.map(friend => {
                    return `
                        <input class="friend" type="checkbox" value="${friend.name}">
                        ${friend.name}
                    `
                })
            }
        `
    }

    render(appStateFriends)
}
```

### Components That Listen

Now the message list component can listen for any messages that are sent to the Event Hub that it cares about.

```js
// MessageList.js
import { getMesssageByFriend } from "./MessageProvider.js"

const eventTarget = document.querySelector(".appContainer")
const contentTarget = document.querySelector(".messages")

const MessageList = () => {
    const appStateFriends = useFriends()

    /*
        CHANGE: The message list component is listening for a very
                specific event that it cares about. It can then extract
                the data in the payload and use it however it wants.
    */
    eventTarget.addEventListener("friendSelected", event => {
            const friendName = event.detail.selectedFriend
            const messages = getMessagesByFriend(friendName)
            render(messages)
        }
    })

    const render = messageCollection => {
        contentTarget.innerHTML = `
            ${
                messageCollection.map(friend => {
                    return `
                        <input class="friend" type="checkbox" value="${friend.name}">
                        ${friend.name}
                    `
                })
            }
        `
    }
}
```
