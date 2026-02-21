# 1. HOW "CSS" WORKS IN REACT NATIVE

- React Native does NOT use browser CSS files. Instead you use JavaScript
  objects to define styles, usually with StyleSheet.create().
- There is no CSS cascade or selectors; styles are combined explicitly:
  style={[base, override]}.
- Layout is done by the Yoga engine (a Flexbox-based layout system).
- Values are numbers representing density-independent pixels (dp). Use
  strings like '50%' for percentages.
- Style props map directly to native view properties (UIView / Android View).

# 2. HOW LAYOUT IS DECIDED (YOGA flow) — simple steps

- 1. React builds a tree of components (View, Text, Image).
- 2. Each node has a style object (width, flex, padding, etc).
- 3. Yoga measures and runs a Flexbox pass to decide sizes and positions.
- 4. Yoga outputs exact frames (x, y, width, height) for each native view.
- 5. Native UI is updated with those frames.
     Re-layout happens on prop/state changes, dimension/orientation change,
     or when parent/child styles change.

# STYLE MERGING — how to override safely

- There is no cascade. Merge styles explicitly with arrays:

```jsx
<View style={[baseStyle, isError && errorStyle]} />
```

- Later entries in the array override earlier ones.

# PERFORMANCE TIPS (keep layouts fast)

- Use StyleSheet.create for static styles to get small optimization.
  - Avoid creating style objects inline in render when possible:
    // bad: style={{ margin: someValue }}
    // better: memoize or use StyleSheet
  - Reduce deep nesting of Views; each extra node costs layout work.
  - For list-heavy UIs, use FlatList with getItemLayout when possible.
  - For animations, prefer native drivers (reanimated or Animated with
    useNativeDriver) so layout/animation work is off the JS thread.

# ABSOLUTE POSITIONING VS FLEXBOX — when to use which

- Use absolute positioning for overlays, floating buttons, badges, etc.
- For responsive, flow-based layouts prefer Flexbox.
- Absolute removes the element from normal layout flow — use carefully.

# PLATFORM DIFFERENCES — watch outs

- Shadows:
  iOS: shadowColor, shadowOffset, shadowOpacity, shadowRadius
  Android: elevation
  - Text / font metrics may vary across platforms; test on both iOS & Android.
  - Some style props behave slightly differently on each platform.

# 10. COMMON INTERVIEW QUESTIONS & SHORT ANSWERS

**Q: How does RN calculate layout?**
A: Yoga (Flexbox) measures the tree and returns frames (x,y,width,height).

**Q: Default flexDirection?**
A: 'column' (vertical stacking)

**Q: flex:1 vs width: '100%'?**
A: flex:1 fills remaining space along the main axis; width:'100%' sets only width.

**Q: Units for sizes?**
A: Numbers → density-independent pixels (dp). Percentages use strings.

**Q: What triggers re-layout?**
A: Style changes, size changes, prop/state changes, orientation changes.

**Q: How to debug?**
A: Use the RN Inspector (Dev Menu), add borders, and use onLayout to log sizes.
