# CORS
CORS option parsing and checking origin is built-in to `@stricjs/utils`.

Install `@stricjs/utils` using Bun or your favorite package manager.
```bash
bun add @stricjs/utils
```

Import in your source file:
```typescript
import { CORS } from '@stricjs/utils';
```

Create a `CORS` object to parse and get headers: 
```typescript
const cors = new CORS();
cors.headers; // Parsed headers 
cors.check('origin'); // Return the headers matching the current origin
```

