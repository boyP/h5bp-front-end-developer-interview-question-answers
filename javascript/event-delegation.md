# How does event delegation work?

## What is event delegation?

Event delegation is a pattern in which the parent html element handles the event on behalf of its child element.

## Without event delegation

To understand the benefit of event delegation we must first see what the world looks like without it. 

### Exercise instructions
Open [this codepen](https://codepen.io/boyP/pen/YzVZgdQ?editors=0010) and complete these exercises.

1. Add event handlers to each of the list items so that each list item is a different color when clicked.
2. Modify the event handler to remove the menu item when clicked. Don't forget to remove the event listener of the removed element. 
3. As a thought exercise, what would the code look like if we added 50 more list items?

If you get stuck, refer to this [completed exercise](https://codepen.io/boyP/pen/oNWYXgN).

## With event delegation

So now you have experienced the world without event delegation. Let's see how things change with it. 

### Exercise instructions
Open [this codepen](https://codepen.io/boyP/pen/MWmpxzd) and complete these exercises.

1. Add event handlers to the parent so that each list item is a different color when clicked.
2. Modify the event listener to remove the menu item when clicked. Do you notice a difference between the previous solution and this one?
3. As a thought exercise, what would the code look like if we added 50 more list items?

At this point, you should understand the benefit of event delegation and how to use it in practice.

If you get stuck, refer to this [completed exercise](https://codepen.io/boyP/pen/mdmWvgP?editors=1010).

## Limitations of event delegation

1. Event delegation only works if the event bubbles up (i.e. a parent is notified of a child event). However, not all events bubble up. **Blur**, **focus**, **scroll**, **change** are some examples that do not bubble up. And for good reason. They are specific to a single element.
 
2. Event delegation can be intentionally (or unintentionally) blocked with `event.stopPropagation()`. This function stops the event from bubbling up which means the parent's handler would not fire.

## Usage with frameworks
Let's be real. Most of the time you'll be using a framework. Let's see how some frameworks deal with event delegation. 

### ReactJS
React actually implements event delegation as an opimization. This means we don't need to use event delegation because it already does it under the hood. Let's see it in action!

[Open this codepen](https://codepen.io/boyP/pen/qBmrvva)

1. Open up the chrome dev tools

2. Inspect the cheeseburger Li element

3. Click the eventListners tab

4. Expand the click event handlers. You should see 2 for `div#root` and 1 for `li#cheeseburger`

5. The cheeseburger handler is actually a dummy handler used to resolve a safari issue. See [this article](https://maddevs.io/blog/a-bit-about-event-delegation-in-react/) for more information. 

6. Remove the cheeseburger event listener and then click on the cheeseburger list item. It should still work as before. This proves that that event isn't actually responsible for the color change logic. 

7. Next remove the first `div#root` event listener then click on the cheeseburger list item. It should not work anymore. In fact, none of the menu item clicks should trigger a color change. This proves that React does event delegation by default.