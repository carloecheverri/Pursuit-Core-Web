# DOM Manipulation with JavaScript

## Goals

- Use querySelector and querySelectorAll to get HTML elements
- Insert new HTML elements
- Delete HTML elements
- Replace HTML elements

## Readings

- https://www.w3schools.com/js/js_htmldom_nodes.asp
- https://www.w3schools.com/jsref/met_document_createelement.asp
- https://www.w3schools.com/jsref/met_document_queryselector.asp
- https://www.w3schools.com/jsref/met_document_queryselectorall.asp
- https://www.w3schools.com/tags/tag_div.asp

# 1. Using querySelector to get elements

## querySelector()

Last lesson, we saw that `getElementById()` was one way that we could access HTML elements.  But what if we don't have the ID of the element we are looking for, or we didn't define an ID at all?  `querySelector()` is a powerful method that can search for HTML elements by many means, not just their ID.

In the example below, we can use `querySelector()` to find the paragraph element.

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">  
  <body>
    <p>This is my website!</p>
    <script>
      let paraElement = document.querySelector("p")
      window.alert(paraElement.innerText)
    </script>
  </body>
</html>
```

By passing in "p" to the querySelector, we're asking it for a paragraph element.  But what happens if we have multiple paragraph elements?

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <body>
    <p>This is my website!</p>
    <p>And I even have two paragraphs!</p>
    <script>
      let paraElement = document.querySelector("p")
      window.alert(paraElement.innerText)
    </script>
  </body>
</html>
```

The alert is the same as before.  querySelector will find the first element that matches the conditions that we give it.  Fortunately, querySelector works with IDs as well:

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <body>
    <p id='introPara'>This is my website!</p>
    <p id='additionalPara'>And I even have two paragraphs!</p>
    <script>
      let paraElement = document.querySelector("#additionalPara")
      window.alert(paraElement.innerText)
    </script>
  </body>
</html>
```

## querySelectorAll()

We can also user querySelectors to get multiple elements all at once.  This method returns an object where the keys are numbers, and the values are the elements:

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <body>
    <p>This is my website!</p>
    <p>And I even have two paragraphs!</p>
    <script>
      let allParaElements = document.querySelectorAll("p")
      for (let element of allParaElements) {
          window.alert(element.innerText)
      }
    </script>
  </body>
</html>
```

There are a lot more things you can do with querySelectors.  A full list is [here](https://www.w3schools.com/jsref/met_document_queryselector.asp), and we'll be resisting them, especially when we get to CSS.

# 2. Creating new HTML elements

So far, when we've wanted to show new information to the user, we've done so by changing the innerText of an existing HTML element.  We can also add new HTML elements entirely to change what elements exist.  We'll use the method `createElement()` to make a new element.  The argument it takes in, is what kind of element we want to make.

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <body>
    <p>This is my website!</p>
    <script>
      let endingPara = document.createElement("P")
      endingPara.innerText = "Thanks for visiting!"
    </script>
  </body>
</html>
```

When we open this in a browser, we see the first paragraph, but not the second.  Why not?  It's because we never put this paragraph anywhere.  Even though we've created our element, we never added it to the DOM.  We can add it directly to the body by using the `body` property of the `document`.  All HTML elements also have an `appendChild` function that we can use.

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <body>
    <p>This is my website!</p>
    <script>
      let endingPara = document.createElement("P")
      endingPara.innerText = "Thanks for visiting!"
      document.body.appendChild(endingPara)
    </script>
  </body>
</html>
```

That works great to add it to the body.  But this will just add it direclty to the body. If we want to add a new element to an ordered list, for example, we'll need a different approach.  We can use `getElementById` or `querySelector` to find the element that we want directly, then add the child to it.


```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <body>
    <h1>Today's Cubs Lineup</h1>
    <ol id="lineupList">
      <li>Jason Heyward (L) RF</li>
      <li>Nicholas Castellanos (R) RF</li>
      <li>Kris Bryant (R) 3B</li>
      <li>Anthony Rizzo (L) 1B</li>
      <li>Javier Baez (R) SS</li>
      <li>Kyle Schwarber (L) LF</li>
      <li>Ian Happ (S) LF</li>
      <li>Victor Caratini (S) C</li>
    </ol>
    <script>
      let lesterListItem = document.createElement("li")
      lesterListItem.innerText = "Jon Lester (L) P"
      let listElement = document.getElementById("lineupList")
      listElement.appendChild(lesterListItem)
    </script>
  </body>
</html>
```

Great!  Now all 9 players show up on our website.  But writing out 8 `<li>` elements out manually is a bit tedious.  Let's do it all through our script instead.


```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <body>
    <h1>Today's Cubs Lineup</h1>
    <ol id="lineupList">
    </ol>
    <script>
      class Player {
        constructor(name, hand, position) {
          this.name = name
          this.hand = hand
          this.position = position
        }
        getDescription() {
          return `${this.name} (${this.hand} ${this.position})`
        }
      }

      let allPlayers = [
        new Player("Jason Heyward", "L", "RF"),
        new Player("Nicholas Castellanos", "R", "RF"),
        new Player("Kris Bryant", "R", "3B"),
        new Player("Anthony Rizzo", "L", "1B"),
        new Player("Javier Baez", "R", "SS"),
        new Player("Kyle Schwarber", "L", "LF"),
        new Player("Ian Happ", "S", "LF"),
        new Player("Victor Caratini", "S", "LF"),
        new Player("Jon Lester", "L", "P"),
      ]

      let listElement = document.getElementById("lineupList")

      for (let player of allPlayers) {
        let newPlayerListItem = document.createElement("li")
        newPlayerListItem.innerText = player.getDescription()
        listElement.appendChild(newPlayerListItem)
      }
    </script>
  </body>
</html>
```

This approach is much more flexible.  If we wanted to change something about how we displayed each player, we would only have to do that in one place instead of nine.  For example, try editing both of the above methods to not show the position of the player on the list.

Additionally, when we get data from online, it will usually look something like the `allPlayers` object we made above.

# 3. Deleting HTML elements

We can also delete HTML elements from the DOM using the `removeChild` method.  To delete an element, we need to know both the parent object, and its child.

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <body>
    <div id="div1">
      <p id="para1">This is the first paragraph</p>
      <p id="para2">This is the second paragraph</p>
    </div>
    <button onclick="deleteFirstParagraph()">Delete the first paragraph</button>
    <script type="text/javascript">
      function deleteFirstParagraph() {
        let divisionOne = document.getElementById("div1")
        let paraOne = document.getElementById("para1")
        divisionOne.removeChild(paraOne)
      }
    </script>
    </body>
</html>
```

It is a little annoying to have to get both the parent node and the child node.  Fortunately, HTML elements have a `.parentNode` property that we can use to get the parent node.

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <body>
    <div id="div1">
      <p id="para1">This is the first paragraph</p>
      <p id="para2">This is the second paragraph</p>
    </div>
    <button onclick="deleteFirstParagraph()">Delete the first paragraph</button>
    <script type="text/javascript">
      function deleteFirstParagraph() {
        let paraOne = document.getElementById("para1")
        paraOne.parentNode.removeChild(paraOne)
      }
    </script>
  </body>
</html>
```


# 4. Replacing HTML elements

We can also edit the DOM by replacing nodes using the `replaceChild()` method.  `replaceChild()` takes in two arguments, the new node that you are replacing it with and the child node to replace.

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <body>
    <div id="div1">
      <p id="para1">This is the first paragraph</p>
      <p id="para2">This is the second paragraph</p>
    </div>
    <button onclick="replaceFirstParagraph()">Replace the first paragraph</button>
    <script type="text/javascript">
      function replaceFirstParagraph() {
        let firstPara = document.querySelector("p")
        let newNode = document.createElement("p")
        newNode.innerText = "Here's a new random number: " + Math.random()
        firstPara.parentNode.replaceChild(newNode, firstPara)
      }
    </script>
  </body>
</html>
```
