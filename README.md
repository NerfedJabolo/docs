# Stric

A framework for building high-performance, scalable web applications.

## Features

- **Performant**: Optimize the handler by storing and composing `@medley/router` tree structure into a single handler.
- **Scalable**: Ensures high performance even under heavy workloads.
- **Simple**: Components are lightweight and simple to use.
- **Extensible**: Comes with a plugin system, dependencies injection and optional optimizations.

Stric is one of the fastest JS frameworks. See the [benchmark](//github.com/bunsvr/benchmark) for more details.

[Benchmark compact result](//gist.githubusercontent.com/aquapi/ec3bcae3c0f6ca84309c908d0f51fcc7/raw/d283ef40a6c61ad16237a9139d228243cc74e88e/compact.txt ':include :type=code')

## Quick Start
You need to have [Bun](//bun.sh) installed to use Stric.

Create a new project and install the router.
```bash
# Create an empty project
bun init

# Install the router
bun add @stricjs/router
```

After installing, you can start writing a simple server at `index.ts`.
```typescript
import { Router } from '@stricjs/router';

// This will respond 'Hi' on every request to path '/'
export default new Router().get('/', () => new Response('Hi'));
```

Start the server using `bun index.ts`. Now you can go to [localhost:3000](http://localhost:3000) and you should see `Hi` as a response.

Just like that, you have created your first app with Stric 🎉.
