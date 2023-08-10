# Query parsing
Query parsing is included in the `@stricjs/utils` module.

Install `@stricjs/utils` using Bun or your favorite package manager.
```bash
bun add @stricjs/utils
```

Import in your source file:
```typescript
import { qs, query } from '@stricjs/utils';
```

## General query parsing 
For getting all the key values, you should use the general query parser.
```typescript
app.get('/search', ctx => {
    // Get the id
    const parsed = query(ctx.url.substring(ctx.query + 1));
    parsed.id;
});
```

## Key parsing
If you only need to get a certain key value you can create a key parser.
```typescript
const getID = qs.searchKey('id');

app.get('/search', ctx => {
    const id = getID(ctx);
});
```
