# CSS/readSync\(\)

This function returns one or more style properties for the given element. It is a convenient alternative to [`window.getComputedStyle`](https://developer.mozilla.org/en-US/docs/Web/API/Window/getComputedStyle). It also has special support for vendor-prefixed properties.

The suffix *Sync* differentiates this method from its *Async* counterpart - [`readAsync()`](../readasync). Unlike the *Async* counterpart, `readSync()` is a normal function that runs in the same flow with that of the calling code.

## Import

```javascript
import readSync from '@webqit/play-ui/src/css/readSync.js';
```

## Syntax
See [`cssSync() - Get Computed Properties`](../csssync#greater-than-get-computed-properties)