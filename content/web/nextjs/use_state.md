---
title: "Use State in React"
tags: ["web", "react"]
draft: true
---

# What is it?

A way of remembering state and requesting a re-render. React components can change, but altering a simple variable does not trigger that change. `Use State` gives us both **memory** and a **trigger**

# Structure

```
const [_var, _func] = useState(_initialValue)
```
The array structure might be weird, but is simply javascript **destructuring**:
```
const state = useState(_initialValue)
const value = state[0]
const setter = state[1]
```


