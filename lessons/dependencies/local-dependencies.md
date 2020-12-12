---
path: "/dependencies/local-dependencies"
title: "Local dependencies"
order: "4B"
section: "4 - Understanding Dependencies"
description: "Burke dives into how Deno handles dependencies."
---

> Make sure you are on the [4-local-depedencies](https://github.com/burkeholland/deno-exercises/tree/4-local-depedencies) branch to follow along with this section.

First let's look at how Deno handles local dependencies. We'll do that by creating a local dependency and importing it into our main "app.ts" file.

- Add a file to the project called "utils.ts".
- Add a const called "utils".
- Add a function to that const that reverses a string.
- Export the `utils` constant as the default export.

```typescript
const utils = {
    reverse: (text: string) => {
        return text.split('').reverse().join('');
    }
}

export default utils;
```

In the "app.ts" file, require the `utils` module. Note that you will have to specify the file extension. Unlike Node, Deno does not allow you to import a module without an extension. This is the one way in which it differs from the ECMAScript spec.

```typescript
import utils from "./utils.ts";
```

From the "app.ts" file, call the `reverse` method and log out the value.

```typescript
import utils from "./utils.ts";

console.log(utils.reverse("Hello World"));
```

Execute the program and review the output...

```bash
Check file:///home/burkeholland/dev/deno-first-look-exercises/app.ts
dlroW olleH
```

Let's examine how Deno handled this dependency locally.

- Open the generated output by navigating to the Deno cache folder...

Linux

```
~/.cache/deno/gen/file/<path-to-deno-first-look-exercises>

// on my machine, it looks like this...
~/.cache/deno/gen/file/home/burkeholland/dev/deno-first-look-excercises
```

Windows

```powershell
C:\Users\burkeholland\AppData\Local\deno\gen\file\<path-to-deno-first-look-exercises>

// on my machine, it looks like this...
C:\Users\burkeholland\AppData\Local\deno\gen\file\C\dev\burkeholland\deno-first-look-exercises
```

You should see the following files present...

- app.ts.buildinfo
- app.ts.js
- app.ts.meta
- utils.ts.js
- utils.ts.meta

If you open the `app.ts.js` file, you'll see that the utils file is being included and it's output has been transpiled here. 

This should look familiar based on what we reviewed during the TypeScript section. Next we'll look at how this changes (or doesn't) when you install dependencies from other sources. Remote Dependencies.
