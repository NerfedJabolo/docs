# Tips to write performant application
Stric has one of the fastest core router, but performance problems can still happen (as the way people write code).

## Cache static headers
Headers are, most of the time static. So caching them is important.
Consider this example:
```typescript
app.get('/', () => new Response(null, {
    headers: { 'X-Powered-By': 'StricJS' }
}));
```

This will create an object on **every** request.
Instead use this:
```typescript
const options = {
    headers: { 'X-Powered-By': 'StricJS' }
};

app.get('/', () => new Response(null, options));
```

Or shorter with `@stricjs/utils`:
```typescript
import { writeHead } from '@stricjs/utils';

const send = writeHead({
    headers: { 'X-Powered-By': 'StricJS' }
});

app.get('/', () => send(null));
```

## Async functions usage
Async functions should only be used when needed. 
For example, this:
```typescript
app.get('/', async () => new Response());
```

Will perform worse than:
```typescript
app.get('/', () => new Response());
```

## Reduce variable creation
Even if it makes the code cleaner, reduce the variable creation as much as possible.
For example, this:
```typescript
app.get('/', () => {
    const date = Date.now();
    return new Response(String(date));
});
```

Should be rewritten to:
```typescript
app.get('/', () => new Response(
    String(Date.now())
));
```

In fact, destructuring the context object does create variables.

The context object is normally pass to handlers as a reference to 
the original object, which does not allocate memory.

Stric is faster than the alternatives, but if you don't write your application 
in a performant way it can still perform as worse (simply this happens with the others).
