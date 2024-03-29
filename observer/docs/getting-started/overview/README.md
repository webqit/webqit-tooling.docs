---
desc: Take a one-minute overview of the Observer API.
_index: first
---
# Overview

Take a one-minute rundown of the Observer API.

## Observe

Observe operations on any object or array...

```js
let obj = {};
```

...using the [`Observer.observe()`](../../api/reactions/observe) method.

```js
Observer.observe(obj, mutations => {
    mutations.forEach(mutation => {
        console.log(mutation.type, mutation.name, mutation.path, mutation.value, mutation.oldValue);
    });
});
```

Now changes will be delivered *synchronously* - as they happen.

## Mutate

Programmatically make *reactive* changes using the *Reflect-like* [set of operators](../../api/actions)...

```js
// A single set operation
Observer.set(obj, 'prop1', 'value1');
```
```js
// A batch set operation
Observer.set(obj, {
    prop2: 'value2',
    prop3: 'value3',
});
```

...or switch to using *object accessors* - using the [`Observer.accessorize()`](../../api/actors/accessorize) method...

```js
// Accessorize all (existing) properties
Observer.accessorize(obj);
// Accessorize specific properties (existing or new)
Observer.accessorize(obj, ['prop1', 'prop5', 'prop9']);
```
```js
// Make reactive operations
obj.prop1 = 'value1';
obj.prop5 = 'value5';
obj.prop9 = 'value9';
```

...or even go with a *reactive Proxy* of your object, and imply properties on the fly.

```js
// Obtain a reactive Proxy
let _obj = Observer.proxy(obj);
```
```js
// Make reactive operations
_obj.prop1 = 'value1';
_obj.prop4 = 'value4';
_obj.prop8 = 'value8';
```

And no problem if you inadvertently cascade the approaches. No bizzare behaviours.

```js
// Accessorized properties are already reactive
Observer.accessorize(obj, ['prop1', 'prop6', 'prop10']);

// But no problem if you inadvertently proxy an accessorized object
let _obj = Observer.proxy(obj);

// And yet no problem if you inadvertently made a programmatic call over an already reactive Proxy
Observer.set(_obj, 'prop1', 'value1');
```

## Intercept

How about some level of indirection - the ability to hook into operators like `Observer.set()` and  `Observer.deleteProperty()` to repurpose their operation? That's all possible using the [`Observer.intercept()`](../../api/reactions/intercept) method!

Below, we catch any attempt to set an HTTP URL and force it to an HTTPS URL.

```js
Observer.intercept(obj, 'set', (action, previous, next) => {
    if (action.name === 'url' && action.value.startsWith('http:')) {
        return next(action.value.replace('http:', 'https:'));
    }
    return next();
});
```

Now, only the first of the following will fly as-is.

```js
Observer.set(obj, 'url', 'https://webqit.io');
```
```js
Observer.set(obj, 'url', 'http://webqit.io');
```

## Pass Some Detail to Observers

Operators, like `Observer.set()`, can pass arbitrary value to observers via a `params.detail` property.

```js
// A set operation with detail
Observer.set(obj, {
    prop2: 'value2',
    prop3: 'value3',
}, { detail: 'Certain detail' });
```

Observers will recieve this value in a `mutation.detail` property.

```js
// An observer with detail
Observer.observe(obj, 'prop1', mutation => {
    console.log('An operation has been made with detail:' + mutation.detail);
});
```

## Negotiate with Observers

Observers can access and *act* on a special object called the *Event Object*.

```js
// An observer and the response object
Observer.observe(obj, 'prop1', (mutation, event) => {
    if (1) {
        event.preventDefault(); // Or return false
    } else if (2) {
        event.stopPropagation(); // Or return false
    } else if (3) {
        event.waitUntil(new Promise); // Or return new Promise
    }
});
```

Operators can access and honour the event's state.

```js
// A set operation that returns the eventTypeReturn
let event = Observer.set(obj, {
    prop2: 'value2',
    prop3: 'value3',
}, { eventTypeReturn: true });
```

```js
if (event.defaultPrevented) {
    // event.preventDefault() was called
} else if (event.propagationStopped) {
    // event.stopPropagation() was called
} else if (event.promises) {
    // event.waitUntil() was called
    event.promises.then(() => {

    });
}
```

*Learn more about negotiation [here](../../api/core/Event#negotiating-with-operators)*

## Clean Up Anytime

Need to undo certain bindings? There are the methods for that!

+ [`Observer.unobserve()`](../../api/reactions/unobserve)
+ [`Observer.unintercept()`](../../api/reactions/unintercept)
+ [`Observer.unproxy()`](../../api/actors/unproxy)
+ [`Observer.unaccessorize()`](../../api/actors/unaccessorize)

## The End?

Certainly not! But this rundown should be a good start. Next:

+ Visit the [download](../download) page to obtain the Observer API.
+ Explore the [API Reference](../../api).