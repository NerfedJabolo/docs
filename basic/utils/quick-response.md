# Quick response
Creating response functions to return a static response with `@stricjs/utils`.

Install `@stricjs/utils` using Bun or your favorite package manager.
```bash
bun add @stricjs/utils
```

Import in your source file:
```typescript
import { response } from '@stricjs/utils';
```

Here's an example of how to create a bad request response function.
```typescript
// Bad request
const badReq = response('Bad request', { status: 400 });
badReq(); // Return a response
```


