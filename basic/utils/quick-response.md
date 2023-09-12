# Quick response
Creating response functions to return a static response with `@stricjs/utils`.

Install `@stricjs/utils` using Bun or your favorite package manager.
```bash
bun add @stricjs/utils
```

Import in your source file:
```typescript
import { response, writeHead, createHead } from '@stricjs/utils';
```

Here's an example of how to create a bad request response function.
```typescript
// Bad request
const badReq = response('Bad request', { status: 400 });
badReq(); // Return a response
```

If you want to customize the response body, use `writeHead` instead.
```typescript
const badReq = writeHead({ status: 400 });
badReq('Bad request'); // Return a response
```

If you want to customize the head as well, use `createHead`.
```typescript
const notAllowed = createHead({ status: 403 });
notAllowed(null, { 
    // Just an example :)
    headers: { 'Authorization': 'Required' } 
});
```
