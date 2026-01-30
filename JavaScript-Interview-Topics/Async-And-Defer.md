# Async & Defer

As I mentioned before, JavaScript, like CSS is a **render blocking** element. This simply means the browser needs to wait for it to load and execute before it can parse the rest of the HTML document.

This hugely increases our **First Contentful Paint**. In order to fix this issue we can use two of the features which are not used by many people but are very effective.

**Normal execution**
When you use a `<script>` to load a JavaScript file, it interrupts the parsing of the document. Browser fetches the resource, executes this and then continues on paring:

# The Async attribute

The **async attribute** allows the script to be loaded concurrently with other elements on the page.

Once the script is downloaded, it is executed immediately, without waiting for other scripts or content to finish loading.

```js
<script async src="script.js">
```

This attribute can be used only on external JavaScript files. The file would be downloaded in parallel and once the download is finished, the parsing is paused for the script to be executed:

This can be great for scripts that don’t depend on other scripts, such as analytics or social media widgets.

# Example

**Best Use Case**

- Use async for scripts that are independent and can be executed as soon as they’re downloaded, like ads, tracking, or analytics.

# The Defer attribute

The defer attribute allows the script to be loaded in parallel with other resources but defers its execution until the page has finished loading (after the HTML is parsed).

```js
<script defer src="script.js">
```

This ensures that the page content is fully rendered before any JavaScript is executed, which can prevent render-blocking issues.

Like Async this file gets downloaded in parallel but the execution only happens when the whole HTML document is parsed

# Notes

At the end remember to put all of your script tags right at the end of the body to prevent more delay in parsing your HTML.

As for the browser support, fortunately these attributes are fully supported by all of the major ones.
