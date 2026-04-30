---
title: "Use Effect in React"
tags: ["web", "react"]
draft: true
---


# Use Effect - what is it?

thing of react that allows to control state and react to changes


# Structure

```ts
useEffect(() => {
    // side effects

    return () => {
        // cleanup
    };

}, [dependencies]);
```

As you can see **useEffect** as 3 main parts:

1. **Side effects**: affect body with respect to dependent variable values
2. **Cleanup**(optional): to cleanup/redo changes that are non idempotent
3. **Dependencies**: the actual variables used to control the component

# When do the functions run?

## the useEffect

How and when useEffect runs depends naturally on its dependencies, running **always** on the first render after React updates the scene.

### Case 1 - with dependencies

```ts
useEffect(() => {
    console.log("The effector ran because the number of clicks changed")
}, [clicks]);
```
In this scenario, it runs **everytime** any dependency changes

### Case 2 - with empty dependencies

```ts
useEffect(() => {
    console.log("One time thing")
}, []);
```
Using an empty dependencies makes it run only once

### Case 3 - without dependencies

```ts
useEffect(() => {
    console.log("Runs every render")
});
```
With this approach the effector runs every render even if nothing changed. Only usefull for very specific scenarios.


## Cleanup function ( return )

The optional defined return statement runs in 2 specific situations:

1. **before** the effect runs again ( not in the beggining )
2. When the component unmounts

# Doubts

- when to use without dependencies
- what exactly is **useState** ( its always a pair value/function )
- How do renders work?
- 