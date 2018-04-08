# Query the DOM with querySelector()

## Retrieve the First Matching Component

The `document.querySelector()` method allows you to identify the first element that matches the CSS selector you specify.

```html
<article>
    <section class="article__section article__header">
        Welcome to Nashville Software School
    </section>

    <section class="article__section article__body">
        We are ecstatic that you chose us to guide you through your journey to being a software developer.
    </section>

    <section class="article__section article__footer">
        Contact us at info@nashvillesoftwareschool.com
    </section>
</article>
```

```js
/*
    This code will only return the first section component
    even though there are three components with the same 
    class
*/
const sectionEl = document.querySelector(".article__section")

/*
    This code will only return the third section component
*/
const sectionEl = document.querySelector(".article__footer")
```

## Retrieve all Matching Components

If you want to get a list of all components that match your selector, you need to use the `document.querySelectorAll()` method.

```js
/*
    This code will return all section components
*/
const sectionEl = document.querySelectorAll(".article__section")

NodeList(3)
0 : section.article__section.article__header
1 : section.article__section.article__body
2 : section.article__section.article__footer
```

```js
/*
    This code will still only return the third section component
*/
const sectionEl = document.querySelectorAll(".article__footer")

NodeList
0 : section.article__section.article__footer
```

## Practice

Here's the HTML you saw in the CSS Selectors chapter.

```html
<body>
  <article class="article">
    <section class="article__header">
      Welcome to my blog
    </section>
    <section class="article__content">
      Copper mug small batch meh plaid flexitarian. Before they
      sold out occupy chartreuse hot chicken, la croix
      fingerstache offal VHS air plant. Humblebrag blue bottle
      cred af jean shorts etsy copper mug chicharrones cronut
      art party scenester pabst chillwave. Distillery 8-bit
      pabst fashion axe, tousled cloud bread bushwick roof party
      franzen quinoa fixie.
    </section>
    <aside class="aside_box--dark dashed">
      <div class="article__header">
        Very important box header
      </div>
      Messenger bag sriracha tote bag intelligentsia air plant
      leggings.
    </aside>
    <section class="article__footer">
      Author: Steve Brownlee
    </section>
  </article>
</body>
```

1. Use JavaScript to obtain a reference to the first article header and change its text with `textContent` property to "Welcome the {insert your name here} blog"
1. Use JavaScript to obtain a reference to **all** `article__header` elements and change their `classList` property value to "`article__header important`".