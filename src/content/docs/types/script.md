---
title: Script
description: |
  The most flexible kind of val, a way to run any TypeScript and JavaScript
sidebar:
  order: 2
---

Script vals are free form JavaScript, TypeScript, or JSX. They are useful
for testing, saving utility functions, and one-off operations.

## Scripts For Running One-off Operations

Some good use cases for one-off utility scripts might include:
 - Running [SQLite Migrations](/std/sqlite/migrations).
 - Importing and analyzing data from third party sources like
   [Airtable](/integrations/airtable) or [Google
   Sheets](/integrations/google-sheets/).
 - Updating data in [SQLite](/std/sqlite/sqlite) or [Blob storage](/std/blob)
   that might be referenced by other vals.

## Importing Script Vals

[Imported your script vals](/reference/import) into other vals can open up lots
of possibilities for sharing data, code, and types between vals.

### Shared Data

A script val can store shared data. Let's start with a val called `sharedVal`
that exports an integer value:
```ts val
export value = 2
```

Elsewhere you can import this value and use it.

```ts val
let { value } = await import("https://esm.town/v/maxm/sharedVal");

console.log(value) // => 2
```

### Helper Functions

Additionally you can share helper functions and other code. Here we extend the
initial val to include a helper function with a typed argument.

```ts val
export value = 2

export function isOdd(v: number) {
  return v % 2 !== 0;
}
```

We can import the helper function and use it.
```ts val
let { value, isOdd } = await import("https://esm.town/v/maxm/sharedVal");

console.log(isOdd(57)) // => true
```

If we pass the wrong type of value to `isOdd` we'll get a helpful type error.

```ts val
isOdd("57") // => Error: Argument of type 'string' is not assignable to parameter of type 'number'.
```

### Importing Types

For more complicated usage of TypeScript types you can also import and export
custom types. Let's add another helper function and an exported type to our
shared val.

```ts val
export function printFooBar(fb: FooBar) {
  console.log(`foo: ${fb.foo} bar: ${fb.bar}`);
}

export type FooBar = { foo: string; bar: number };
```

Elsewhere, we can import that type, and the helper function.
```ts val
import type { FooBar } from "https://esm.town/v/maxm/sharedVal";
import { printFooBar } from "https://esm.town/v/maxm/sharedVal";
let fb: FooBar = { foo: "baz", bar: 2 };
printFooBar(fb); // => foo: baz bar: 2
```


## Using `std/set` to Update Shared Script Data

The `std/set` package can edit the content of a val on the fly. You can use this
functionality to share and update data values.

:::note

The `set` package will update the val [using the val.town
API](https://www.val.town/v/std/set?v=14), and updates can overwrite each other
if they occur simultaneously. If you would like more robust data handling you
should consider using [SQLite](/std/sqlite) or [blob](/std/blob).

:::


Let's say we have a shared array of colors in our script val that we've called
`sharedColors`:
```ts val
export let sharedColors = ["orange", "blue", "green"]
```

We can import it and use the `set` package to add a new color to the list:

```ts val
import { set } from "https://esm.town/v/std/set";
let { sharedColors } = await import("https://esm.town/v/maxm/sharedColors");
set("sharedColors", [...sharedColors, "red"]);
```

If we return to our `sharedColors` and view the source it will show our updated
array of colors:

```ts val
// set at Tue Dec 19 2023 20:09:00 GMT+0000 (Coordinated Universal Time)
export let sharedColors = ["orange", "blue", "green", "red"];
```
