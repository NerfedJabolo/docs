# Use CORS with `@stricjs/router`
You can use CORS to check resource origin by registering guards.
```typescript
import { CORS } from '@stricjs/utils';
import { Router } from '@stricjs/router';

const cors = new CORS({
    allowOrigins: ['localhost:3000', 'localhost:4000']
});

export default new Router()
    .plug(cors.guard('/restricted'))
    .all('/restricted/main', ctx => 
        new Response('Hi', { headers: ctx.head })
    );
```

If no origin or one single origin is provided, parse it as static headers.
```
import { CORS, writeHead, createHead } from '@stricjs/utils'; 
import { Router } from '@stricjs/router';

const cors = new CORS(),
    // Send additional response options
    sendWith = createHead(cors.headers),
    // Send a response body only
    send = writeHead(cors.headers);

export default new Router()
    .get('/', () => send('Hi'))
    .get('/teapot', () => sendWith('Hi', {
        status: 418
        headers: { 'X-Current-Date': new Date().toString() }
    }))
```
