# File streaming
The utilities component `@stricjs/utils` provides 2 ways to serve static files.

Install `@stricjs/utils` using Bun or your favorite package manager.
```bash
bun add @stricjs/utils
```

Import in your source file:
```typescript
import { stream, group } from '@stricjs/utils';
```

## Serve a file
This returns a request handler that streams the file.
```typescript
// Return a request handler
stream('path/to/file');
```

## Serve a directory
There are two ways to serve a directory.

### Dynamic serve 
Search for path and serve if path does exists when handling requests.

```typescript
// Parse path, concat with the directory passed in and start checking
stream('public');
```

### Static serving
Create a plugin that pre-search the directory and add routes.

```typescript
// Search for every files in 'public' and add routes 
const plugin = group('public');

app.plug(plugin);
```

