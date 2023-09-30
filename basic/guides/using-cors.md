# Use CORS the router

You can use CORS to check resource origin by registering guards.

```typescript
import { CORS } from '@stricjs/utils';
import { Router } from '@stricjs/router';

const cors = new CORS({
  allowOrigins: ['localhost:3000', 'localhost:4000'],
});

export default new Router()
  .plug(cors.guard('/restricted'))
  .all('/restricted/main', (ctx) => new Response('Hi', { headers: ctx.head }));
```

If no origin or one single origin is provided, parse it as static headers or
use it with other utils to create a response wrapper.

```typescript
import { CORS, writeHead } from '@stricjs/utils';
import { Router } from '@stricjs/router';

const cors = new CORS(),
  // Send a response body only
  send = writeHead({ headers: cors.headers });

export default new Router().get('/', () => 'Hi').wrap('/', send);
```
