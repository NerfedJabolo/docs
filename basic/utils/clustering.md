# Clustering
Cluster can be used to run multiple instances that can distribute workloads among their application threads. 

When process isolation is not needed, use [`Worker`](//bun.sh/docs/api/workers) instead, which allows running multiple application threads within a single instance.

Install `@stricjs/cluster` using Bun or your favorite package manager.
```bash
bun add @stricjs/cluster
```

This example below starts a server with 4 threads on [localhost:3000](http://localhost:3000).
```typescript
import { Router, macro } from '@stricjs/router';
import { spawn, worker } from '@stricjs/cluster';

// Check whether this thread is a worker 
if (worker) {
    const app = new Router().get('/', macro('Hi'));
    Bun.serve(app);
}
// If this is the main thread spawn 4 instances
else spawn(4);

```
