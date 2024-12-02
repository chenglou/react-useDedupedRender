# (Stale) useDedupedRender

**Do not use this.** Only here for archiving purposes.

A React hook for deduping renders. Was supposed to be used so:

```jsx
function MyComponent() {
  const render = useDedupedRender(fn)
  function fn() {
    // ...
    setWhateverState(...)
  }

  useEffect(() => {
    function handleClick() {
      render(fn)
    }
    function handleKeyDown() {
      render(fn)
    }
    // ... register event handlers
  }, [])
}
```

This is so that we can gather all the events from the current frame, and acting on them.

Implementation-wise, the `render` returned from `useDedupedRender` calls `requestAnimationFrame`, which ultimately calls the `fn` passed to `useDedupedRender`.

Unfortunately, `setState` inside `requestAnimationFrame` inside a React event handler is queued _next_ frame, not the current one. Which sucks. So don't use this.

See alternative solution at https://x.com/_chenglou/status/1862262785801167228
