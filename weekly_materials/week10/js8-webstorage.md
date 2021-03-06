# WebStorage API

## Overview
HTML pages are generally "stateless"--even if you change elements on the page based on user interaction, reloading the page sets it back to its original state. 

You can save state information about a page (and a site) by storing data on the user's computer using one of two methods: "cookies" or the WebStorage API. Cookies are name/value pairs typically used for things like session management, personalization, and user tracking. The WebStorage API introduced with HTML5 also allows us to use JavaScript to store **key:value** data in the user's browser, and retrieve that information when the user returns to the site. This API allows more data storage, and is more secure, than cookies. 

**Table of Contents**
* [WebStorage Reference](#webstorage-reference)
* [An Example](#an-example)
* [Storing Objects with WebStorage](#storing-objects-with-webstorage)
* [Important Notes](#important-notes)
* [Review Questions](#review-questions)
<hr>

## WebStorage Reference
These are useful documentation pages and examples:

- https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API
- https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API
- https://html.spec.whatwg.org/multipage/webstorage.html
- https://mdn.github.io/dom-examples/web-storage/


## An Example

- Go ahead and try out this example, whenever you `onchange` the values of the textbox or the &lt;select>, their values are written to `localStorage`. 
- If you close the window and reopen it, your changes will be preserved.  
- After you have referred to the links above, it should be pretty easy to figure out what's going on in the code.
- One thing worth mentioning is the `prefix` variable (see below). Because Web Storage uses the same set of keys for *each domain*, this means on servers like banjo that all of the students are sharing the same set of keys, so that if someone uses `highscores`as a key, another student's `highscores` key could wipe out and replace their data. One solution is to prefix your key names with something unique, like your RIT web account id. Therefore `highScores` would become `abc1234highScores` for one student, and `xyz9876highScores` for someone else, and the keys would never conflict.

### webstorage-1.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8" />
	<title>Web Storage Example</title>
</head>
<body>

<div>
What is your name -> <input type='text' id='nameField'>
</div>

<div>
What is your favorite color -> 
<select id="colorSelect">
	<option value="red">Red</option>
	<option value="green">Green</option>
	<option value="blue">Blue</option>
	<option value="magenta">Magenta</option>
	<option value="cyan">Cyan</option>
	<option value="white">White</option>
	<option value="gray">Gray</option>
</select>
</div>

</body>
<script>

/* This stuff happens "onload" */
// declare some constants
const nameField = document.querySelector("#nameField");
const colorSelect = document.querySelector("#colorSelect");
const prefix = "abc1234-";
const nameKey = prefix + "name"
const colorKey = prefix + "color";

// grab the stored data, will return null if user has never been to this page
const storedName = localStorage.getItem(nameKey);
const storedColor = localStorage.getItem(colorKey);

// if we find a previously set name value, display it
if (storedName){
	nameField.value = storedName;
}else{
	nameField.value = "Turbo";
}

// if we find a previously set color value, display it
if (storedColor){
	colorSelect.querySelector(`option[value='${storedColor}']`).selected = true;
}

/* This stuff happens later when the user does something */
// when the user changes their favorites, update localStorage
nameField.onchange = e=>{ localStorage.setItem(nameKey, e.target.value); };
colorSelect.onchange = e=>{ localStorage.setItem(colorKey, e.target.value); };


</script>
</html>
```

### And here is what it looks like:

![Web Page](web-storage-1.jpg)


## Storing Objects with Web Storage
- A major limitation of Web storage is that it doesn't allow us to store arrays and other objects directly. But there's an easy workaround - you can easily convert built-in JavaScript objects (Object, Array, Date, etc) to and from a string representation, and then save them to `localStorage`. This is known as *serialization* - https://en.wikipedia.org/wiki/Serialization

### Save an Array to localStorage with `JSON.stringify()`

```javascript
let listID = "abc1234-action-list";
let items = ["Direct Movie","Deliver Baby","Cure Cancer"];
items = JSON.stringify(items); // now it's a String
localStorage.setItem(listID, items);
```

### Retrieve an array from localStorage with `JSON.parse()`

```javascript
let listID = "abc1234-action-list";
let items = localStorage.getItem(listID); // returns a String
items = JSON.parse(items);  // now it's an Array
```

## Important Notes
- The process by which the browser works out how much space to allocate to web data storage and what to delete when that limit is reached is not simple, and differs between browsers - read about it here: https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Browser_storage_limits_and_eviction_criteria


## Review Questions
1. What is a limitation of using localStorage on a shared domain like people.rit.edu? What is a workaround that will mitigate this issue?
1. What is the difference between local and session storage?
1. If the user opens up the demo page in a different web browser on the same machine, will their stored preferences still be visible? Why or why not?
1. Define *serialization*
1. What does `JSON.stringify()` do?
1. What does `JSON.parse()` do?
1. One big issue with the applications we have written this semester is that reloading the page will wipe out all of the user's work (for example the pixel art creations in *Pixel Artist*). How could web storage be used to improve any of the other JavaScript exercises you've done?


