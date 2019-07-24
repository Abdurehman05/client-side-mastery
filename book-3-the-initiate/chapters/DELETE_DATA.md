# Deleting Raw Data in an API

You have implemented the POST HTTP verb in order to create a new object in your API. Next is learning to use the DELETE verb to remove an object from your API.

## Learning Objectives

By the end of this chapter, you will have been shown the following techniques.

1. How to add a button in the DOM whose purpose it to initiate a DELETE operation for a particular resource.
1. How to dynamically add the unique identifier for a resource to the `id` attribute of the button responsible for deleting it.
1. How to listen for the click event on that button.
1. Determine which resource should be deleted by using the `split()` method on a string.
1. Perform a DELETE operation with the JavaScript Fetch API.
1. Ensure that the DELETE operation is complete, and then display all resources again.

## Delete Button

By placing the unique key of the resource you are creating in the `id` attribute of the `<button>` _(see below)_, you can use the JavaScript `split()` method to extract it when the button is clicked.

### Get Id on Button Click

```js
const recipeList = document.querySelector("#recipeList")

/*
    Note that the event listener is on the list element,
    not on the individual cards. Use the power of event
    bubbling to your advantage.
*/
recipeList.addEventListener("click", event => {
    const recipeToDelete = event.target.id.split("--")[1]
})
```

### Add Id to Button Element

```js
const createRecipeCard = recipe => `
    <section class="recipe--${recipe.id}">
        <header class="recipe__title">
            ${recipe.title}
        </header>
        <div class="recipe__instructions">
            ${recipe.instructions}
        </div>
        <button id="deleteRecipe--1">
            Delete Recipe
        </button>
    </section>
`
```


## Delete Syntax

Here is how you perform a deletion operation using the `fetch` syntax in JavaScript. You need to always include the `method` and the `Content-Type` header. Also note that the unique key of the resource to be deleted is a route parameter at the end of the URL.

```js
recipeList.addEventListener("click", event => {
    /*
        Only trigger the delete operation if the button
        was clicked. Remember that the side-effect of
        putting the "click" handler on the parent container
        is that if you click on _anything_ inside that
        element, the event will bubble up.
    */
    if (event.target.id.startsWith("deleteRecipe--")) {
        const recipeToDelete = event.target.id.split("--")[1]

        fetch(`http://localhost:8088/resource/${recipeToDelete}`, {
            method: "DELETE",
            headers: {
                "Content-Type": "application/json"
            }
        })
        .then(res => res.json())
    }
})
```

## Delete and Get All Option

Once you delete a resource in your API, you need to rebuild your list of HTML representations because the state of the API has changed. Remember that your HTML is simply a representation of your APIs current state, with affordances to change its state.

This means that you need to request all of the data again and go through the same process that executed when the page first loaded.