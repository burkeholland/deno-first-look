---
path: "/permissions/permission-specificity"
title: "Permission Specificity"
order: "4C"
section: "Permissions"
description: "Burke explores the unique permissions model of Deno."
---

## Permission Specificity

Permission in Deno come with a specificity qualifer that allows you to specify only the access your program needs. In the example above, we requested "read" access. That allows us to read any file or directory that the executing user can. It would be better if we restricted this access to juse the directory that is going to be read.

- Modify the permission descriptor to specify the path that read access will be granted to.

  ```typescript
  const desc = { name: "read", path: "." } as const;
  ```

- Run the program again with `deno run --unstable app.ts`. Notice that this time, the prompt tells you what directory the program is requesting access to read.

  ```bash
  Check file:///home/burkeholland/dev/deno-first-look-exercises/app.ts
  ️⚠️ Deno requests read access to ".". Grant? [g/d (g = grant, d = deny)]
  ```

It is better to ask for only the permissions your app needs. This leads to the question of what happens when your app needs access to multiple different locations?
