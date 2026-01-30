# What is CSS?

CSS stands for Cascading Style Sheets. Think of it as the stylist of your website. While HTML creates the structure of a webpage (like text, buttons, and images), CSS is what makes it look good.

Without CSS, websites would look like a boring list of black text on a white background. With CSS, you can make them colorful, interactive, and visually stunning.

If you are a business guy, let me tell it to you in another way. No matter how good your product is, no one is gonna buy it, unless your packaging is good. That is sales 101. (Yeah, superficial humans, but we just gotta live with that, don’t we?)

So, CSS is that nice-packaging for those functional HTML codes.

# Purpose of CSS=>

**Separation of Concerns**
CSS separates design from HTML structure, making code clean and easy to manage.

**Visual Styling**
CSS controls how elements look — colors, fonts, spacing, backgrounds, and layout.

**Responsive Design**
CSS helps websites adjust automatically for mobile, tablet, and desktop screens.

**Efficiency**
One external CSS file can style the entire website, saving time and keeping design consistent.

# How CSS works?

CSS is a rule-based language. Each rule consists of a selector (which points to the HTML element you want to style) and a declaration block (which contains one or more declarations defining the style properties and their values).

```css
/* Example of a CSS rule */
h1 {
  color: blue;
  font-size: 24px;
}
```

**In this example:**

- h1 is the selector.
- color and font-size are properties.
- blue and 24px are the corresponding values.

The **"cascading"** nature refers to how the browser resolves conflicts between multiple style rules, prioritizing styles based on their importance, specificity, and the order in which they are applied.

# Types of CSS Implementation

**CSS can be added to HTML in three ways:**

# External CSS (Recommended ✅)

- CSS is written in a separate .css file
- Linked to HTML using the <link> tag
- One file can style multiple pages

```cs
<link rel="stylesheet" href="styles.css">
```

**Why use it?**

- Best for large projects
- Easy to maintain
- Reusable
- Clean code

# Internal CSS

- CSS is written inside a <style> tag
- Placed in the <head> of an HTML file
- Applies to only one page
- Good for page-specific styling

```html
<style>
  p {
    color: blue;
  }
</style>
```

# Inline CSS (Not Recommended ❌)

- CSS is written directly inside an HTML tag
- Uses the style attribute
- Applies to only one element

- Bad for maintenance, OK for quick changes

```html
<p style="color:red;">Hello</p>
```

**Interview Ans**

- External CSS is best for large websites, internal CSS is used for single pages, and inline CSS is used for small, quick styling but is not recommended.

# How is CSS applied to HTML?

As explained in How browsers load websites, when you navigate to a web page, the browser first receives the HTML document containing the web page content and converts it to a DOM tree.

After that, any CSS rules found in the web page (either inserted directly in the HTML, or in referenced external .css files) are sorted into different "buckets", based on the different elements they will be applied to (as specified by their selectors). The CSS rules are then applied to the DOM tree, resulting in a render tree, which is then painted to the browser window.

Let's look at an example. First of all, we'll define an HTML snippet that the CSS could be applied to:

```html
<h1>CSS is great</h1>

<p>You can style text.</p>

<p>And create layouts and special effects.</p>
```

Now, our CSS, repeated from the previous section:

```css
h1 {
  color: red;
  font-size: 2.5em;
}

p {
  color: aqua;
  padding: 5px;
  background: midnightblue;
}
```

This CSS:

- Selects all <h1> elements on the page, coloring their text red and making them bigger than their default size. Since there is only one <h1> in our example HTML, only that element will get the styling.
- Selects all <p> elements on the page, giving them a custom text and background color and some spacing around the text. There are two <p> elements in our example HTML, and they both get the styling.
  When the CSS is applied to the HTML, the rendered output is as follows:
