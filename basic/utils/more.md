# More utilities
The `@stricjs/utils` component provides faster alternative utility methods to native methods.

Install `@stricjs/utils` using Bun or your favorite package manager.
```bash
bun add @stricjs/utils
```

## Left pad
This left pad uses [Travvy](https://github.com/ThePrimeagen/leftPadDeez/blob/master/src/leftPads.js#L213) left pad algorithm.

```typescript
import { leftPad } from '@stricjs/utils';

// Prepend 2 '_' to original string;
leftPad('hi', 2, '_'); // "__hi"
```

## Send file
The `sendFile(path)` function is a shorthand for `new Response(Bun.file(path))`.
```typescript
import { sendFile } from '@stricjs/utils';

sendFile('index.ts'); // Send `index.ts` as a response
```

## Extend object
To extend an object with properties of another object, use one of these functions:
```typescript
/**
 * Extend the target object like `Object.assign`.
 */
extend(target: any, source: any): void;

/**
 * Create a compiled function to extend another object.
 * This only works with object which has string or number keys
 */
createExtend(source: any): (target: any) => void;

/**
 * Return a function to create a shallow copy of an object.
 * The object created is a `null` prototyped object.
 */
createCopy<T>(o: T): () => T;
```
