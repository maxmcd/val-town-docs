---
title: Exports
description: |
  Vals can export anything they want, but if you intend to use vals as
  web handlers, email handlers, or on a schedule, they'll need at least
  one export.
---

Vals can export anything they want, but if you intend to use vals as
web handlers, email handlers, or on a schedule, they'll need at least
one export. For example, if you've written an email handler but forgotten
to add the `export` keyword, you'll encounter an error reminding you to
export something.

```tsx val
/**
 * Note the 'export' keyword which tells JavaScript to export
 * this method and make it available to other files.
 */
export function handleEmail(email: Email) {
  console.log("ok!");
}
```
