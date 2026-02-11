# 📘 CSS FLEXBOX – ALL PROPERTIES (Beginner Friendly)

**🟢 What is Flexbox?**

- Flexbox = Layout system to align items easily
- One dimensional layout (row OR column)
- Perfect for responsive UI

**Two parts:**

- 1️⃣ Flex Container (parent)
- 2️⃣ Flex Items (children)

- 🟢 Enable Flexbox

```css
.container {
  display: flex;
}
```

**🟢 Important Axes (VERY IMPORTANT)**

- Main Axis → depends on flex-direction
- Cross Axis → perpendicular to main axis

**flex-direction: row**

- Main Axis → left → right
- Cross Axis → top → bottom

**flex-direction: column**

- Main Axis → top → bottom
- Cross Axis → left → right

**🟢 FLEX CONTAINER PROPERTIES (Parent)**

- 1️⃣ flex-direction
- Defines direction of items

```css
.container {
flex-direction: row; // default
flex-direction: row-reverse;
flex-direction: column;
flex-direction: column-reverse;
}
```

**2️⃣ flex-wrap**

- Controls wrapping of items

```css
.container {
flex-wrap: nowrap; // default
flex-wrap: wrap;
flex-wrap: wrap-reverse;
}
```

**3️⃣ flex-flow (shorthand)**

- flex-flow = flex-direction + flex-wrap

```css
.container {
  flex-flow: row wrap;
}
```

**- 4️⃣ justify-content (MAIN AXIS)**

- Aligns items along MAIN axis

```css
.container {
justify-content: flex-start; // default
justify-content: flex-end;
justify-content: center;
justify-content: space-between;
justify-content: space-around;
justify-content: space-evenly;
}
```

**5️⃣ align-items (CROSS AXIS)**

- Aligns items on CROSS axis

```css
.container {
align-items: stretch; // default
align-items: flex-start;
align-items: flex-end;
align-items: center;
align-items: baseline;
}
```

**6️⃣ align-content (MULTI-LINE ONLY)**

- Works ONLY when:
- flex-wrap: wrap
- multiple rows exist

```css
.container {
  align-content: flex-start;
  align-content: flex-end;
  align-content: center;
  align-content: space-between;
  align-content: space-around;
  align-content: stretch;
}
```

# 🟢 FLEX ITEM PROPERTIES (Children)

**- 7️⃣ order**

- Controls item order (default = 0)

```css
.item1 {
  order: 2;
}

.item2 {
  order: 1;
}
```

**8️⃣ flex-grow**

- Controls how much item grows

```css
.item {
  flex-grow: 1;
}
```

- 0 → no grow
- 1 → grow equally

**9️⃣ flex-shrink**

- Controls shrinking when space is small

```css
.item {
flex-shrink: 1; // default
}
```

**🔟 flex-basis**

- Initial size before grow/shrink

```cs

.item {
flex-basis: 200px;
}
```

**1️⃣1️⃣ flex (shorthand)**

- flex = grow shrink basis

```css
.item {
  flex: 1 1 200px;
}
```

**Most common usage:**

```css
.item {
flex: 1; // grow = 1, shrink = 1, basis = 0
}
```

**1️⃣2️⃣ align-self**

- Overrides align-items for single item

```css
.item {
  align-self: center;
}
```

**🟢 VISUAL CHEAT SHEET (Mental Model)**

- justify-content → MAIN axis alignment
- align-items → CROSS axis alignment
- align-content → MULTI-ROW alignment

**🟢 COMMON INTERVIEW CONFUSIONS**

- **align-items vs align-content**

**- align-items:**

- - Aligns items inside ONE row

**- align-content:**

- - Aligns rows themselves
- - Needs flex-wrap

- justify-content vs align-items

- justify-content → MAIN axis
- align-items → CROSS axis

**🟢 REAL-WORLD EXAMPLES**

- Center an element

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

- Responsive cards

```css
.container {
  display: flex;
  flex-wrap: wrap;
}

.card {
  flex: 1 1 300px;
}
```

**🟢 INTERVIEW ONE-LINERS**

- Flexbox is a one-dimensional layout system
- that aligns items along main and cross axis.

- justify-content works on main axis,
- align-items works on cross axis.

**🟢 FINAL SUMMARY**

**- Parent:**

- display
- flex-direction
- flex-wrap
- justify-content
- align-items
- align-content

**- Child:**

- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self
