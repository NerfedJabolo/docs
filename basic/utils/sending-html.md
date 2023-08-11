# Sending HTML
For convenient, `@stricjs/utils` provides the `html` method to send HTML response.

Install `@stricjs/utils` using Bun or your favorite package manager.
```bash
bun add @stricjs/utils
```

Import in your source file:
```typescript
import { html, createHTML } from '@stricjs/utils';
```

To send an HTML string, simply wrap it with `html()`.
```typescript
app.get('/', () => html(`<p>Hi</p>`));
```

If you want to customize the header to be sent, create your own HTML function.
```typescript
const html = createHTML({ status: 404 }); 
html(`<p>Not found</p>`); // Return a response
// Or even static HTML 
const notFound = createHTML({ status: 404 }, `<p>Not found</p>`);
notFound(); // Return a response
```
