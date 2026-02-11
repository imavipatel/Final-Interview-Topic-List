# Q1. How css works in react native how layout get decides

# HOW "CSS" WORKS IN REACT NATIVE — plain language

# STYLE MERGING — how to override safely

- There is no cascade. Merge styles explicitly with arrays:

```jsx
<View style={[baseStyle, isError && errorStyle]} />
```

- Later entries in the array override earlier ones.

# 7. PERFORMANCE TIPS (keep layouts fast)

- Use StyleSheet.create for static styles to get small optimization.
- Avoid creating style objects inline in render when possible:

```jsx
// bad: style={{ margin: someValue }}
// better: memoize or use StyleSheet
```

- Reduce deep nesting of Views; each extra node costs layout work.
- For list-heavy UIs, use FlatList with getItemLayout when possible.
- For animations, prefer native drivers (reanimated or Animated with
  useNativeDriver) so layout/animation work is off the JS thread.
