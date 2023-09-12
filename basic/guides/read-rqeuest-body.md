# Read request body with `@stricjs/router`
Add a third argument to your route handler for Stric to parse the body.
```typescript
import { Router } from '@stricjs/router';

export default new Router()
    .post('/yield', ctx => new Response(ctx.data), {
        body: 'text'
    });
```

This comes with type hint for each parser.
