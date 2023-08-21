# More utilities
The `@stricjs/utils` component provides faster alternative utility methods to native methods.

Install `@stricjs/utils` using Bun or your favorite package manager.
```bash
bun add @stricjs/utils
```

## Decode URI 
The `decodeURIComponent` is a method from [fast-decode-uri-component](https://github.com/delvedor/fast-decode-uri-component) with JSC optimizations.

```typescript
import { decodeURIComponent } from '@stricjs/utils';

decodeURIComponent('Hi%20there'); // "Hi there"
```

## Left pad
This left pad uses [Travvy](https://github.com/ThePrimeagen/leftPadDeez/blob/master/src/leftPads.js#L213) left pad algorithm.

```typescript
import { leftPad } from '@stricjs/utils';

// Prepend 2 '_' to original string;
leftPad('hi', 2, '_'); // "__hi"
```
