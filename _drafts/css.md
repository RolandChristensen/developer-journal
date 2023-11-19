Quick notes on CSS

# Themes
The `:root` selector represents the `<html>` element.

Add the following to the top of your css file.

```css
:root {
  --green: #00FF00;
  --white: #FFFFFF;
  --black: #000000;
}
```

You can use the variables above in other CSS rules.

At the bottom of your css file add the following.

```css
.light-theme {
  --bg: var(--green);
  --fontColor: var(--black);
}
.dark-theme {
  --bg: var(--black);
  --fontColor: var(--green);
}
```

The `bg` and `fontColor` defines two new variables for background and font color using the `var()` method calling back to the `:root` definitions of variables.

Add the following to your css file.

```css
body {
  background: var(--bg);
  color: var(--fontColor);
  font-family: helvetica;
}
```

In your HTML you wll add a class attribute `class=light-theme` or `class=dark-theme` and we will then add JavaScript to change it on the fly.

In the HTML put the following in at the bottom of your body tag. Putting it at the end of the body tag enables the entire page to load before the script is loaded.

```html
<script src="app.js"></script>
```

Adding *fault tolerance* or *graceful degradation* lets your code detect and plan for when a feature isn't supported or available.

```html
<noscript>You need to enable JavaScript to view the full site.</noscript>
```

Set strict mode to get more useful errors when you make mistakes. JavaScript lets a lot of things slide and that can make it very hard to debug.

Place the following at the top of your JavaScript file.

```javascript
'use strict';
```

Add event handler in the JavaScript

```javascript
const switcher = document.querySelector('.btn');

switcher.addEventListener('click', function() {
    document.body.classList.toggle('light-theme');
    document.body.classList.toggle('dark-theme');

    const className = document.body.className;
    if(className == "light-theme") {
        this.textContent = "Dark";
    } else {
        this.textContent = "Light";
    }
});
```
