# Optimizations
Here are some optimizations you can do to make your Stric application faster.

## Set the base URL 
You can specify the exact base URL of your app to make path parsing faster. Note that will not works correctly if your app has subdomains.

```typescript 
app.base = 'http://localhost:3000'; // You can pass an option object as well
```

## Skipping characters
If `base` is not provided and `parsePath` is `true`, Stric will search for the first `/` character starting from index `12` by default.

To change this behavior, set the `uriLen` to the value you want to search `/`.

```typescript
app.uriLen = 20;
```

## Macros
Macros inject your code directly to your fetch without caching and calling the handler like usual, which can reduce response time. 

This feature should only be used for small handlers.
```typescript
import { macro } from '@stricjs/router';

app.get('/', macro(() => new Response('Hi')));
// Or shorter (For string response)
app.get('/', macro('Hi'));
```

### Limitations
- Macros cannot access variables outside its scope. You need to store the states to use in macros.
- Macros code cannot use `await`. The fetch function is synchronous. You can return a promise, but can't use `await`.
- Macros need proper parameters name (`r`, `s` or both). Code in macros that directly use the request and 
store need to have the same name as the variables in `fetch`.
