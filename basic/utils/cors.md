# CORS

CORS option parsing and checking origin is built-in to `@stricjs/utils`.

Install `@stricjs/utils` using Bun or your favorite package manager.

```bash
bun add @stricjs/utils
```

Import your source file:

```typescript
import { CORS } from '@stricjs/utils';
```

Create a `CORS` object to parse and get headers:

```typescript
const cors = new CORS({
  allowOrigins: '*', // Value can be a string or string[]
  allowMethods: ['GET', 'POST'], // Also this can be a string
  allowCredentials: true, // This should be set to a boolean
});

cors.headers; // Get the parsed headers
cors.copy(); // Clone the parsed headers
cors.check('origin'); // Return the headers matching the current origin

const check = cors.create(); // Return a function to assign CORS header to `req.head`

// Validate origin and assign headers
check(req);

// Return a plugin to check CORS headers (override the guard of that path)
cors.guard('/');
```

The options name are the same as other `cors` libraries you used before.
