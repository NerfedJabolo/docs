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
    // Contine chaining here
    .get('/', /* Handler here */);
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
app.get('/', () => new Response('Hi'))
    .post('/json', ctx => ctx.json().then(Response.json));
```

To add a handler for all methods, use:
```typescript
app.all('/', () => new Response('Hi'));
```

This handler will be executed if the current method does not match any of the path handlers with specific methods.

## Parametric routes 
You can parametric routes to retrieve data from URLs.

To access the parsed data use `params` property of the request context. Note that the type is infered automatically so you don't need to.
```typescript
app.get('/id/:id', ctx => new Response(ctx.params.id));
```

## Wildcard routes 
Similar to parametric routes but you can get the rest of the path.
```typescript
app.get('/*', ctx => new Response(ctx.params['*']));
```

Wildcard routes are executed if normal routes and parametric routes didn't match the request path and method.

## Body parsing
Stric has a set of built-in body parser you can use.
```typescript
app.post('/user/new', ctx => {
    // Use the parsed body here (it has type hint)
    ctx.data;
}, { body: 'json' });
    
// Available parsers:
{ body: 'text' } // Parse to Text
{ body: 'json' } // Parse to JSON
{ body: 'blob' } // Parse to Blob
{ body: 'form' } // Parse to FormData
{ body: 'buffer' } // Parse to ArrayBuffer
{ body: 'none' } // Skip parsing body. This is the default value
```

To handle when body parsing failed, call `use(400)` to use the default `400` handler or handle using your own handler.
```typescript
// Use the default handler (return only status and no message)
app.use(400);

// Register your own handler 
app.use(400, ctx => { /* Handle error and return a response */ });
```

## Special handlers
Stric provides a way to handle `404`.
```typescript
// Default handler 
app.use(404);

// Custom handler 
app.use(404, ctx => { /* Return a response here */ });
```

For error handling, you can register a `500` handler.
```typescript
// Default handler 
app.use(500);

// Custom handler 
app.use(500, /* Handle error using Bun default handler */);
```

## Guarding routes 
Guard routes work like wildcard but these routes are invoked first to check whether a specific sub-route should be handled.

```typescript
// Return null to tell the router to use the 404 handler (or reject handler if set)
app.guard('/admin', ctx => 
    ctx.headers.get('Authorization') === 'admin' || null
);

// Custom reject handler 
// If this is not set the app will use the default 404 handler
app.reject('/admin', () => new Response('Forbidden'));
```

## Plugins
You can use plugins to modify the app. For example:
```typescript
const plugin = app => app
    .all('/json', () => new Response('Invalid method'))
    .post('/json', ctx => Response.json(ctx.data), { 
        body: 'text' 
    });

app.plug(plugin);
```

## Groups
Router groups are plugins designed specifically for registering routes of a specific route.
```typescript
const group = new Group('/json')
    .all('/', () => new Response('Invalid method'))
    .get('/get', () => Response.json({ hi: 'there' }))
    .post('/', ctx => Response.json(ctx.data), {
        body: 'text'
    });

app.plug(group);
```

## Aliases
You can alias handlers from an already-existed path to another path. This basically set specific method handler for that path.
```typescript
// Register a `GET` handler for `/`
app.get('/', macro('Hi'));

// This will not be overritten after the alias
app.post('/api/new', () => {});

// This copies the `GET` handler to the new path
app.alias('/api/new', '/');
```

## Store 
Stric has a store for storing utilities and states to use outside of the scope.

```typescript
app.store('check', str => str === 'admin')
    .guard('/', (ctx, store) => store.check(
        ctx.headers.get('Authorization')
    ));
```

## WebSocket
You can split routes for WebSocket handlers.

```typescript
app.ws({
    message(ws) {
        ws.send('Hi');
    } 
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
