# Chrome Extensions!

Hi! I'm going to explore more about chrome extensions.


# Bookmarklet

It’s the above javascript code inside an  `a href`  element with  `javascript:`in front of the self-executable function.
```html
<a href="javascript:(function(){})()">bookmarklet</a>
```
If your code is complicated and long it’s often simpler to just put it in another JS file and reference it like so:

```javascript
(function () {
  var script = document.createElement('script');
  script.src = 'http://shiffman.net/a2z/js/bookmarklet.js';
  document.body.appendChild(script);
})();
```

## Content Scripts

A content script is a piece of JavaScript code (which can also include and reference CSS) that runs after the page loads. It has full access to the DOM so you can do things like alter content, styles, layout, images, anything that is on the page. The manifest.json file should reference your content script.
```javascript
{
  "manifest_version": 2,
  "name": "My Extension",
  "version": "0.1",
  "content_scripts": [
    {
      "js": ["content.js"]
    }
  ]
}
```
```javascript
"content_scripts": [
    {
      "matches": [
        "<all_urls>"
      ],
      "js": ["content.js"]
    }
  ]
```
```javascript
var elts = document.getElementsByTagName('p');
for (var i = 0; i < elts.length; i++) {
  elts[i].style['background-color'] = '#F0C';
}
```
## Background Scripts

When the user clicks the button, it triggers an “onClick” event. This code would go in  `background.js`.

```javascript
// Add a listener for the browser action
chrome.browserAction.onClicked.addListener(buttonClicked);

function buttonClicked(tab) {
  // The user clicked the button!
  // 'tab' is an object with information about the current open tab
}
```
## Messaging

A message is just a JavaScript object of your own design that you send from the background script to the content script. Here is the sending:
```javascript
  chrome.tabs.sendMessage(tab.id, msg);

```
And then in the content script you can receive the message and perform an action. Lots of data comes in with the message, but the actual object you sent is in the `request` variable.
```javascript
// Listen for messages
chrome.runtime.onMessage.addListener(receiver);

// Callback for when a message is received
function receiver(request, sender, sendResponse) {
  if (request.message === "user clicked!") {
    // Do something!
  }
}
```
