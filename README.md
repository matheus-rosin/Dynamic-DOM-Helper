# Dynamic-DOM-Helper
Just a simple help to create elements dynamically

*To the lovers of vanilla Javascript!*

Let's suppose you want to insert new content into your document dynamically. You would do this:
```javascript
let anchor = document.createElement('a')
anchor.href = '#link'
anchor.textContent = 'My Link'
document.querySelector('#some').appendChild(anchor)
```
With the help, you can write like this:
```javascript
let anchor = HTML('a', {
    a: { 'href': '#link' },
    c: 'My Link'
})
document.querySelector('#some').appendChild(anchor)
```
At first, it doesn't appear to have any advantage. So let's look more closely:
### the caller: `HTML`.
This is the function itself. You should call it with at least a parameter, that is a simple string with the name of the element you want to create. So you can execute, for example:
```javascript
HTML('br')
HTML('section')
HTML('video')
// &c.
```
You can also include a second parameter, which should be an object that can contain four keys:
### `a`, `e`, `c` and `m`.
The `a` key stands for **attributes**. Thus you can easily set your attributes like this:
```javascript
HTML('input', {
    a: { 'type': 'range', 'min': '0', 'max': '9', 'step': '1', 'name': 'myRangeInput' }
})
// This will render <input type="range" min="0" max="9" step="1" name="myRangeInput">
```
The `e` keys stands for **events**. It would be like this:
```javascript
HTML('button', {
    e: {
        'click': function() { /* the whole stuff */ },
        'focus': function() { /* the whole stuff */ },
        'blur': function() { /* the whole stuff */ }
    }
})
```
The events are attached using `addEventListener`. `useCapture` is `true` by default. In case you need set it to `false`, the solution (at least for the time being) is to use an array, like this:
```javascript
HTML('nav', {
    e: {
        'click': [function() { /* the whole stuff */ }, false]
    }
})
```
So now we pass to the `c` key, that stands for **children** or **content**; you choose.
If you want a singular child, you can declare it directly. For example:
```javascript
HTML('p', {
    c: 'My paragraph'
})
// This will render <p>My paragraph</p>
HTML('ul', {
    c: HTML('li', {
        c: 'My list item'
    })
})
// This will render <ul><li>My list item</li></ul>
```
As you can perceive, it is possible either declare an element or a string, which must be interpreted as a text node.
And if you will insert more than one node, you can do this by passing an array with all of them, such as:
```javascript
HTML('audio', {
    c: [
        HTML('source'),
        HTML('source'),
        HTML('source'),
        'Your browser is from the Stone Age'
    ]
})
```
And lastly, there is the `m` key, that means **methods**. This is intended to help make things organized. Imagine:
```javascript
let menu = HTML('aside', {
    m: {
        show: function() {
            this.classList.add('show')
            // some class with opacity = 1 and visibility = visible, for example
        },
        hide: function() {
            this.classList.remove('show')
            // return to opacity = 1 and visibility = hidden, for example
        }
    }
})
HTML('button', {
    c: 'Show menu',
    e: {
        click: function() {
            return menu.show()
        }
    }
})
HTML('button', {
    c: 'Hide menu',
    e: {
        click: function() {
            return menu.hide()
        }
    }
})
```
Nice, right? To have an function linked to the object itself instead of call an external one. And you can indeed use this to record simple values:
```javascript
HTML('table', {
    m: {
        info: 'Table with customers info &c.'
    }
})
```
There is still a third parameter possible. It's for
###  `custom elements`.
(My intention is not tell if it is good or not, useful or not; it is, rather, give support...)
As you should know, there are two types of custom elements: the completely new ones and the extended ones. So if your custom element is a new one (those that you make from `HTMLElement` API), you can use the function normally:
```javascript
HTML('product-box')
```
And if your custom element is an extended one (those which you make from some built-in element, e.g. from `HTMLAnchorElement` API), you must use this way:
```javascript
// Let's suppose that you are extending an anchor element
HTML('a', {
    // ...
}, 'internal-link')
// This will render <a is="internal-link"></a>
```
And we're done! That's all for now. Enjoy it with :coffee:.
