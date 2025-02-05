# Routing

Stric provides a heavily optimized router for handling requests.

To get started, install the component with Bun or your preferred package manager.

```bash
bun add @stricjs/router
```

Import and use the router like this:

```typescript
import { Router } from '@stricjs/router';

const app = new Router();
```

Or if you want to use chaining like other frameworks:

```typescript
import { Router } from '@stricjs/router';

export default new Router()
  // Contain chaining here
  .get('/' /* Handler here */);
```

To set the options you can pass an option object directly to the constructor or manually set it. For example:

```typescript
new Router({ port: 8080 });
// or
app.port = 8080;
```

## Define routes

You can define route handlers for a pathname using the syntax `method(path, handler)`. For example:

```typescript
app.get('/', () => new Response('Hi'));
```

The router API supports chaining. That means you can do:

```typescript
app
  .get('/', () => new Response('Hi'))
  .post('/json', (ctx) => ctx.json().then(Response.json));
```

To add a handler for all methods, use:

```typescript
app.all('/', () => new Response('Hi'));
```

## Handlers

Route handlers have 2 parametes, `ctx` for current request content and `server` for
accessing the current running server instance.

```typescript
(ctx, server) => {};
```

A handler can return anything, including an object and primitives that are not instances of `Response`.

The response then will be wrapped by a wrapper or return directly if a wrapper was not specified.

## Parametric routes

You can use parametric routes to retrieve data from URLs.

To access the parsed data use the `params` property of the request context. Note that the type is inferred automatically so you don't need to.

```typescript
app.get('/id/:id', (ctx) => new Response(ctx.params.id));
```

## Wildcard routes

Similar to parametric routes but you can get the rest of the path.

```typescript
app.get('/*', (ctx) => new Response(ctx.params['*']));
```

Wildcard routes are executed if normal routes and parametric routes didn't match the request path and method.

## Body parsing

Stric has a set of built-in body parser you can use.

```typescript
app.post(
  '/user/new',
  (ctx) => {
    // Use the parsed body here (it has type hint)
    ctx.data;
  },
  { body: 'json' }
);

// Available parsers:
{
  body: 'text';
} // Parse to Text
{
  body: 'json';
} // Parse to JSON
{
  body: 'blob';
} // Parse to Blob
{
  body: 'form';
} // Parse to FormData
{
  body: 'buffer';
} // Parse to ArrayBuffer
{
  body: 'none';
} // Skip parsing body. This is the default value
```

To handle when body parsing failed, call `use(400)` to use the default `400` handler or handle using your own handler.

```typescript
// Use the default handler (return only status and no message)
app.use(400);

// Register your own handler
app.use(400, (ctx) => {
  /* Handle error and return a response */
});
```

## Special handlers

Stric provides a way to handle `404`.

```typescript
// Default handler
app.use(404);

// Custom handler
app.use(404, (ctx) => {
  /* Return a response here */
});
```

For error handling, you can register a `500` handler.

```typescript
// Default handler
app.use(500);

// Custom handler
app.use(500, (err, ctx, server) => {
  // Handle error and return a response
});
```

## Response wrappers

Response wrappers provide a way to wrap a specific group of subroute responses.

```typescript
// Use the default wrapper
app.get('/', () => 'Hi').wrap('/', (res) => new Response(res));
```

In the example above, accessing the `/` will return a response with text `Hi`.

A wrapper is a function that accepts up to 3 parameters.
The first parameter is the response the route returns, and other parameters are the same as handler parameters.

```typescript
// Return a response
(res, ctx, server) => {};

// This is the default wrapper, which is manually specified in the example above
(res) => new Response(res);
```

If a wrapper is not specified as the second parameter of `wrap`, the default wrapper will be used.

You can also specify a wrapper for a single route by doing:

```typescript
// Use the default wrapper if set to true or default
app.get('/', () => 'Hi', { wrap: true });
```

To disable wrapping for a specific route:

```typescript
app.get('/', () => new Response('Hi'), { wrap: false });
```

Note that this wrapper will be prioritized over parent wrappers.

There are built-in response wrappers to make this more convenient.

- `default`: This wraps the return value of the route with a `Response` object.
- `json`: This serializes the return value using `JSON.stringify` and wraps it with a `Response` object.
- `send`: This sends metadata such as `ctx.status`, `ctx.statusText` and `ctx.head` along with the response (You can set it while handling).
- `sendj`: This works like `send` but wraps the return value like the `json` wrapper.

You can specify these built-in wrappers with:

```typescript
app.wrap('/', 'send');
// Or in specific handler
app.get(
  '/',
  (ctx) => {
    ctx.status = 418;
    return `I'm a teapot`;
  },
  { wrap: 'send' }
);
```

## Guarding routes

Guard routes work like wildcard but these routes are invoked first to check whether a specific subroute should be handled.

Guards have the same parameters as a normal route handlers.

```typescript
// Return null to tell the router to use the 404 handler (or reject handler if set)
app.guard(
  '/admin',
  (ctx) => ctx.headers.get('Authorization') === 'admin' || null
);

// Custom reject handler
// If this is not set the app will use the default 404 handler
app.reject('/admin', () => new Response('Forbidden'));
```

## Plugins

You can use plugins to modify the app. For example:

```typescript
const plugin = (app) =>
  app
    .all('/json', () => new Response('Invalid method'))
    .post('/json', (ctx) => ctx.data, { body: 'text', wrap: true });

app.plug(plugin);
```

## Groups

Router groups are plugins designed specifically for registering routes of a specific route.

```typescript
const group = new Group('/json')
  .all('/', () => new Response('Invalid method'), { wrap: false })
  .get('/get', () => {
    hi: 'there';
  })
  .post('/', (ctx) => ctx.data, { body: 'json' })
  .wrap('/', 'json');

app.plug(group);
```

## Aliases

You can alias handlers from an already-existed path to another path. This basically sets a specific method handler for that path.

```typescript
// Register a `GET` handler for `/`
app.get('/', macro('Hi'));

// This will not be overritten after the alias
app.post('/api/new', () => {});

// This copies the `GET` handler to the new path
app.alias('/api/new', '/');
```

## WebSocket

You can split routes for WebSocket handlers.

```typescript
app.ws({
  message(ws) {
    ws.send('Hi');
  },
});
```

WebSocket routes cannot collide with other route handlers of the router, but you can still register
route handlers with the same pathname but not `GET` method.

## Fetch meta

To get the fetch meta, use the `meta` getter.

```typescript
app.meta;
```

The fetch meta includes the parameters to put in the scope of the fetch function,
the values associated with these params and the fetch function body.

This method should be used for advanced use cases only.

## Start the server

There are two ways to start the server.

```typescript
// This gets the handler and runs it normally
export default app;

/*
This runs the garbage collector (by default) and then
starts the server and returns the server instance
*/
app.listen();
```
