# File streaming

The utilities component `@stricjs/utils` provides 2 ways to serve static files.

Install `@stricjs/utils` using Bun or your favorite package manager.

```bash
bun add @stricjs/utils
```

Import your source file:

```typescript
import { file, dir, group } from '@stricjs/utils';
```

## Serve a file

This returns a request handler that streams the file.

```typescript
// Return a request handler
file('path/to/file');
```

## Serve a directory

There are two ways to serve a directory.

### Dynamic serve

Search for a path and serve it if the path does exist when handling requests.

```typescript
app.all('/public/*', dir('./public'));
```

### Static serving

Create a plugin that pre-searches the directory and add routes.

```typescript
// Search for every file in 'public' and add routes
const plugin = group('public');

app.plug(plugin);
```
