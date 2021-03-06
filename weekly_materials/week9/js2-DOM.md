# Introduction to JavaScript 2: Working with the Web DOM

In this exercise we are going to look at how to use JavaScript to alter HTML elements on the page. To do this we will need to utilize the DOM (Document Object Model). The DOM API defines methods and properties need to access and manipulate a web page.

When developers discuss database records and operations, there's a concept known as **C.R.U.D.** - "Create", "Read", "Update" and "Delete". Every database needs to be able to let the developer perform these operations in code.

Similarly, if we are going to create web applications, we will have to do the same things to our web pages: **create** new elements, select (**read**) existing elements, **update** elements, and **delete** elements.

## Selecting and Modifying Elements on the Page
If you want to modify an HTML element, you first need to:

1. Get a *reference* to the element
1. Change the value of a *property* of the element

To get a **reference to a single element** we usually use `document.querySelector(selector)` where `selector` is a valid CSS selector. For example `document.querySelector("p")` would select the first paragraph on a web page, while `document.querySelector("#table")` would select the element on the page of `id="table"`.

To get a **reference to multiple elements** we usually use `document.querySelectorAll(selector)`. For example, `document.querySelectorAll("p")` would return an array (actually a `DomNodeList`) of all of the paragraph tags on a page.

HTML elements are instances of the class `HTMLElement` and have `innerHTML` and other properties we can set. A full list of element properties and methods can be found at https://developer.mozilla.org/en-US/docs/Web/API/Element and  https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement

Create a new HTML file called dom-1.html, save it, and open it in Chrome. It is attempting to change the text of the &lt;h1> on the page to "My *UFO* Page":

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="utf-8" />
   <title>DOM-1</title>

   <style>
      body{border:1px solid gray;}
   </style>

   <script>
     // first, get a reference to the first <h1> on the page
     let h1 = document.querySelector("h1");

     // second, change its HTML
     h1.innerHTML = "My <em>UFO</em> Page";	
	</script>
</head>
<body>
	<h1>My Page</h1>
	<h2>Info</h2>
	<h2>Pictures</h2>
	<h2>Info</h2>
	<p>Blah Blah Blah</p>
	<p>Blah Blah Blah</p>
	<footer>Copyright &copy; 20XX</footer>
</body>
</html>
```
Load it up in Chrome. 

## It  didn't work!
When you load this into Chrome, the &lt;h1> text doesn't change. What went wrong?

### Check the JavaScript console
Bring up the console in the Inspector to get an idea why.

![The JavaScript Console](dom-2.jpg)

The error is: `Uncaught TypeError: Cannot set property 'innerHTML' of null`. 
The JavaScript engine is complaining about our second line of code.

### Check the JavaScript debugger
So what has a value of `null`? It turns out that our variable `h1` does - which we can see if we select the **Sources** tab, then select **dom-1.html** on the left, check the **Pause on caught exceptions** checkbox, and reload the page.

![The JavaScript Console](dom-3.png)

**(Fun fact - if you click on the line numbers next to the code listing, you can add and remove breakpoints that will interrupt code execution whenever the debugger is open. You will then be able to inspect the values of variables, and step through your code)**

The syntax for our `h1` selector is correct, but the value is `null` - so what gives?

## Order of Operations
The "null" problem we had above is happening because the JavaScript code is in the head of the document, so it's running *before* the web page has loaded. 

Since the h1 element hasn't yet been loaded into the DOM, our statement `let h1 = document.querySelector("h1");` returns a value of `null`, and  when we try to set the `.innerHTML` of `null` we get an error.

**How to fix this? Simply run the code *after* the page loads.** There are many ways to accomplish this, for now  the easiest way is to move the &lt;script> element to just before the closing &lt;body> tag, like this: 

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8" />
	<title>DOM-1</title>
	 <style>
      body{border:1px solid gray;}
   </style>
</head>
<body>
	<h1>My Page</h1>
	<h2>Info</h2>
	<h2>Pictures</h2>
	<h2>Info</h2>
	<p>Blah Blah Blah</p>
	<p>Blah Blah Blah</p>
	<footer>Copyright &copy; 20XX</footer>
	<script>
		// first, get a reference to the first <h1> on the page
		let h1 = document.querySelector("h1");
		
		// second, change its HTML
		h1.innerHTML = "My <em>UFO</em> Page";	
	</script>
</body>
</html>
```

**Load the page into a browser, you should now see the changes we made to the heading!:**

![Web Page](dom-4.jpg)

## Experimenting With CSS Selectors
The power of `document.querySelector()` and `document.querySelectorAll()` is that they accept all CSS selectors, including those in the CSS3 standard.

https://www.w3.org/TR/css3-selectors/#selectors

Let's try a few of these out. Create a new file called dom-2.html, and put the following code in it: 

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8" />
	<title>DOM-2</title>
	<style>
		body{border:1px solid gray;}
	</style>
</head>
<body>
<h1>My Page</h1>
<h2>Info</h2>
<h2>Pictures</h2>
<h2>More Info</h2>
<p>Blah Blah Blah</p>
<p>Blah Blah Blah</p>
<p id="lastParagraph">Blah Blah Blah</p>
<footer>Copyright &copy; 20XX</footer>
<script>
	// first, get a reference to the first <h1> on the page
	let h1 = document.querySelector("h1");
	
	// second, change its HTML
	h1.innerHTML = "My <em>UFO</em> Page";	
	
	// 3 - get a reference to the second paragraph and change its HTML in 1 line of code
	document.querySelector("p:nth-of-type(2)").innerHTML = "I am 2nd paragraph!";
	
	// 4 - get a reference to the third paragraph using an id selector and change its HTML in 1 line of code
	document.querySelector("#lastParagraph").innerHTML = "I am the last paragraph";
	
	// 5 - get a reference to the <footer>
	let footer = document.querySelector("footer");
	
	// 6 - we can set CSS values through the .style property
	footer.style.color = "green";
	footer.style.fontFamily = "monospace";
	footer.style.fontSize = "2em";
	footer.style.border = "2px solid pink";
	footer.style.paddingTop = "10px";
	footer.style.paddingBottom = "10px";
	footer.style.margin = "5px";
</script>
</body>
</html>
```

Load the page in Chrome to see the results:

![Web Page](dom-5.jpg)

### A. Explanations
There was quite a bit going in that last example. Let's break it down:

In the third instruction, we used a **pseudo selector** of `p:nth-of-type(2)` to select the 2nd paragraph.

In the fourth instruction, we used an **id selector** to reference to the element with the id of "lastParagraph"

In the fifth instruction, we used a **type selector** to get a reference to the footer element

In the sixth instruction we changed the CSS on the &lt;footer> element by accessing its `.style` property. Note that in JavaScript, to use the CSS properties that have dashes in their name (like `font-family`) we need to make alterations. We have to drop the dash in the property name, and "camel case" the second word--so the CSS `font-family` property becomes `style.fontFamily`. You can see in the example above that we also had to do this for `font-size`, `padding-top` and `padding-bottom`.

## Selecting Multiple Elements with document.querySelectorAll()
When we use `document.querySelector()`, we get only the first element matching the request. If we want all of the elements that match, we use `document.querySelectorAll()`, which returns an array of results that match the given selector.

Create a new file called dom-3.html, and add the following code: 
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="utf-8" />
    <title>DOM-3</title>
    <style>
        body {
            border: 1px solid gray;
        }
    </style>
</head>

<body>
    <h1>My Page</h1>
    <h2>Info</h2>
    <h2>Pictures</h2>
    <h2>More Info</h2>
    <p>Blah
        <strong>Blah</strong> Blah</p>
    <p>Blah
        <strong>Blah</strong> Blah</p>
    <p id="lastParagraph">Blah
        <strong>Blah</strong> Blah</p>
    <footer>Copyright &copy;
        <strong>20XX</strong>
    </footer>
    <script>

        // 1 - get a reference to ALL of the <h2> elements on the page and add a number to the beginning of them
        // also, do some CSS transforms on them.
        let allHeadings = document.querySelectorAll("h2");

        // 2 - loop through array
        for (let i = 0; i < allHeadings.length; i++) {
            let h2 = allHeadings[i];
            let currentText = h2.innerHTML;
            h2.innerHTML = "#" + (i + 1) + " - " + currentText;

            h2.style.transform = "translateX(" + (i * 120) + "px) translateY(" + (i * 100) + "px) rotate(" +
                (i * 20) + "deg)";
            // or write the above as an ES6 template string
            //h2.style.transform = `translateX(${i*120}px) translateY(${i*100}px) rotate(${i*20}deg)`;

            h2.style.display = "inline-block";
            // make sure the h2 isn't too wide, or it will rotate off the screen
        }

        // 3 - use a *descendent selector* to target the <strong> tags inside of paragraphs 
        // but not that <strong> tag in the footer
        let myList = document.querySelectorAll("p strong");
        for (let element of myList) {
            // we  set CSS values through the .style property
            element.style.color = "red";
            element.style.fontVariant = "small-caps";
            element.style.fontFamily = "monospace";
            element.style.fontSize = "1.5em";
            element.style.border = "2px solid pink";
            element.style.paddingTop = "5px";
            element.style.paddingBottom = "5px";
        }

    </script>
</body>

</html>
</body>
</html>
```

**Load the page into a browser to see the changes our JavaScript brought about:**

![Web Page](dom-6.jpg)

### Explanations
In **instruction 1** we used `querySelectorAll("h2")` to get all of the &lt;h2> elements in an array (actually a DomNodeList). 

In **instruction 2** we looped through the array using the classic `for` loop that we know and love. We set the CSS `transform` property and saw some interesting effects.

In **instruction 3** we used a *descendant* selector to target &lt;b> tags that are inside of paragraphs, and the ES6 `for/of` to loop over the array. 

## Page Source vs Web Inspector
It is important to understand the difference in Chrome between what we see when we "View Source" in Chrome, and what we see when we activate the Web Inspector.

### "View Source" in Chrome
If we view the HTML source of our page in Chrome (right-click and choose View Page Source), we will not see any of the changes our JavaScript made. Also, there will be no indication that our new styles were applied:

![Web Page](dom-7.jpg)

### Web Inspector in Chrome
But if we utilize the Web Inspector, we WILL see all of those changes reflected in the DOM tree under the **Elements** tab:

![Web Page](dom-8.jpg)

## Important Notes
1. In this document we have been using `document.querySelector()` and `document.querySelectorAll()` to select elements on the page. Out on the web you will also see the older methods `document.getElementsByTagName()` and `document.getElementById()` in use - we recommend that you NOT use these methods as they are much less flexible and powerful than the `querySelector()` and `querySelectorAll()` methods.
2. Using the JavaScript debugger and setting breakpoints was briefly touched upon above. Being able to utilize the JavaScript debugger is an essential skill, and we will be demoing this in class. If you need to see it demoed again then please ask! Here is a helpful article on using the Chrome web tools: https://developers.google.com/web/tools/chrome-devtools/javascript/breakpoints
3. What happens when `document.querySelectorAll()` finds no matching elements on the page, what does it *return*? Answer: an empty array.
4. What happens when we try to loop though an empty array with a `for` or `for...of` loop? Answer: Nothing. The looping never happens if the array is empty.
5. When `document.querySelector()` finds no matching elements on the page, what does it *return*? Answer: `null`.
6. What happens if we try to call a method on `null`? Answer: We get a runtime error! How can we avoid this? Read on!

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8" />
	<title>Writing safer code</title>
</head>
<body>

<script>
let element = document.querySelector("h4"); // will be null because page has no h4 elements
//element.innerHTML = "Found an h4 tag"; // WILL CAUSE AN ERROR

// Safer way is to check for null first
if (element != null){
   element.innerHTML = "Found an h4 tag";
}else{
  console.log("No h4 found");
}

// Shorten up your if statements!
// JavaScript has a number of "falsy" values (false, 0, undefined, null , "", '') 
// that evaluate to false in boolean contexts.
// In a boolean context, anything that is not false, is true.

// So we can replace the above with:
if (element){ // element will be considered true if it is not one of the falsy values above
   element.innerHTML = "Found an h4 tag";
}else{
  console.log("No h4 found");
}
</script>
</body>
</html>
```

## Review Questions
1. What does "CRUD" stand for?
1. What happens when we try to use JavaScript DOM methods to access the contents of a page before it has loaded?
1. What is the name of the DOM method that will return the first element that matches the given selector?
1. What is the name of the DOM method that will return **all** elements that match the given selector?
1. Which property is used to get and set the text and HTML contents of an HTML element?
1. Which property is used to get and set the CSS styles of an HTML element?
1. Write a line of JavaScript that sets the `background-position` style property of an element to the value of `"top"`.
1. Give 2 ways to loop through an array.
1. Compare and contrast "View Source" versus the capabilities of the Web Inspector. Which method gives the developer a "live" view of the current HTML and CSS of a page?
1. How can we add breakpoints to our code in the debugger, and inspect the values of variables?
1. What does the `debugger;` statement do? (We did not talk about this one at all, so google it!)

## Deliverable
Make a copy of **dom-3.html** and name it **javascript2.html**. Delete all of the existing JavaScript code, and add JavaScript that does the following (search the web for documentation if you don't know how to do these). Make sure that you DO NOT modify the HTML source of the page in ANY way (by adding `class` or `id` attributes to the paragraphs, for example.

1. Change the `.innerHTML` of the first &lt;h1> to "My UFO Page"
1. Change the `.innerHTML` of the first &lt;h2> to "My UFO Info"
1. Change the `.innerHTML` of the 2nd &lt;h2> to "My UFO Pictures"
1. Change the `.innerHTML` of the 3rd &lt;h2> to an empty string - `""`
1. Select the &lt;body> element and make 2 style changes:
  - The `font-family` shall be "sans-serif"
  - The font `color` shall be "reddish" (specify a red shade in hexadecimal) - 
  **Power tip:** `document.body` is a handy shortcut property for getting a reference to the &lt;body> element.
6. Select the first paragraph and make some changes:
  - The inner HTML will contain the text "Report your UFO sightings here:" and have a working link to http://www.nuforc.org  - **Power tip:** In JavaScript strings, single-quotes `'` can be nested inside of double-quotes `"`
  - There will be `.style` changes:
    - the font `color` is "green"
    - the `font-weight` is "bold"
    - the `font-size` is "2em"
    - the `text-transform` is "uppercase"
    - the `text-shadow` is "3px 2px #A44"
7. Change the `.innerHTML` of the 2nd paragraph to an empty string - `""`
8. Change the `.innerHTML` of the 3rd paragraph to instead show an image of a UFO that is out on the web (use an &lt;img> tag)
9. Change the `.innerHTML` of the &lt;footer> copyright notice to show the current year and your name

**Here is a completed example:**

![Web Page](dom-9.jpg)

When you're done, upload the file to banjo.rit.edu, and link to it from your main 230 page. It should be completed no later than 11:59pm on Friday, October 27. 


  
