---
title: "JavaScript Events - Bubbling, Capture and Delegation"
date: "2023-10-22"
---

## Bubbling

Event bubbling is simply the way the browser treats events targeted at nested elements.  
[- MDN web docs](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_bubbling)

Essentially, any event that is triggered on a nested element will also be triggered on any element that is a parent. Not just the immediate parent, but all parents.

```html
<div class="parent">
  <div class="child">
    <button class="inner-child">Click Me</button>
  </div>
</div>
```

So if a click event is triggered on the button, which is the most nested element, the click event will also be triggered on the div with the class of "child" and then on the div with the class of "parent" in that order.  
The way in which the event goes from the inner most element to the outer most element is how we get the term bubbling. The event bubbles up much like bubbles would float upwards from the bottom of a container filled with water.

If we ever need to stop this behaviour, we would have to call the **stopPropagation** method on the event object.  
Taking the above code snippet.

```js
var button = document.querySelector("button");

button.addEventListener("click", (event) => {
  event.stopPropagation();
  // do whatever
});
```

This would register the click event with the button and the button only. This click event does not bubble up to the parent divs.

## Capture

There's not much to say about this except that it operates in the reverse order. So instead of events of the most nested elements moving outwards to the parent elements, the events move from the parent to the child element. This is a behaviour that you would have to opt into, however, as the default behaviour is that of event bubbling.

```js
var parentDiv = document.querySelector(".parent");

parentDiv.addEventListener(
  "click",
  (event) => {
    // do whatever
  },
  { capture: true }
);
```

The above snippet would cause the click event to first register on the parent div and move inwards to the child div. We'd then add a similar click event listener to the child div to get it to then move inwards to the button element.

## Delegation

Event bubbling isn't all bad. As with many other things it has its benefits. If you ever find yourself registering the same event listener to multiple sibling elements, you could instead add the logic to an event listener registered on their parent element. When the event is triggered on the child, it would then bubble up to the parent and the block of code executed.  
In this case we'd make use of the target property on the event object if we want to control something specific to the element clicked.

```html
<div class="parent">
  <button class="inner-child">Click Me</button>
  <button class="inner-child">Click Me</button>
</div>
```

Here we have two buttons that are siblings. If we somehow need them both to respond to the same event in the same way, we'd add the event listener to the parent div.

```js
var div = document.querySelector("div");

div.addEventListener("click", (event) => {
  // do something
});
```
