# Testing APIs

You can test router endpoints using the `mock` method.

```typescript
import { mock, Router, macro } from '@stricjs/router';

// An example app
const app = new Router()
  // Send 'Hi'
  .get('/', macro('Hi'))
  // Yield the request body
  .post('/json', (ctx) => Response.json(ctx.data), {
    body: 'json',
  });

// Create a test client
const client = mock(app);
```

To test an endpoint:

```typescript
// This calls the fetch function and return the response as string
await client.text('/'); // 'Hi'

// This calls the fetch function and parse the response to JSON
await client.json('/json', {
  method: 'POST',
  // If the body is an object it can be automatically stringified
  body: { hi: 'there' },
}); // { "hi": "there" }
```

Other similar methods:

- `client.head`: Return the header.
- `client.ok`: Return whether the response is ok.
- `client.msg`: Return the status message.
- `client.code`: Return the status code.
- `client.stat`: Return an object contains `code` and `text` represent the status code and message.
- `client.blob`: Return the blob sent.
- `client.form`: Return the form data sent.
- `client.buf`: Return the response as an `ArrayBuffer`.

You can pass the same arguments to the above methods as `Request`
constructor arguments, except for the first argument which must be a pathname.

## Logging

You can specify the log level for the test client.

```typescript
mock(app, { logLevel: 0 });
```

There are 4 log levels available:

- `0`: Disable. This is the default value.
- `1`: Log the pathname that the client is testing.
- `2`: Log the fetch function metadata and the pathname.
- `3`: Log the response returned, the pathname and the fetch function metadata.

Log level below `0` behaves like log level `0` and log level above `3` behaves like log level `3`.

## Fetch meta

To get the fetch function metadata of the client, access the `meta` property from the client.

```typescript
client.meta;
```
