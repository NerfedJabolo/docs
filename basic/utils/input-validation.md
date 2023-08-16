# Input validation
Stric utilities include a `guard` namespace to create guard functions.

Install `@stricjs/utils` using Bun or your favorite package manager.
```bash
bun add @stricjs/utils
```

Import in your source file:
```typescript
import { guard } from '@stricjs/utils';
```

Create a guard function and validate data:
```typescript
const check = guard({
    name: 'str',
    age: 'num',
    // Put a question mark to make a property optional
    address: '?str'
});

check({ name: 'Reve', age: 15 }); // Yield the object when matches
check({ name: 'Reve' }); // Return null when not matches
```

All built-in types are:
- `str`: `string` 
- `num`: `number`
- `bool`: `boolean`
- `nil`: `null`
- `undef`: `undefined`

As of right now arrays are not supported.
