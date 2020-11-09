---
path: "/dependencies/remote-dependencies.md"
title: "Remote dependencies"
order: "5C"
section: "Understanding Dependencies"
description: "Burke dives into how Deno handles dependencies."
---

"Remote" dependencies are how Deno handles what you would know today as "npm" packages. These are libraries that you might consume and use that someone else has written.

Deno maintains a list of [approved third-party modules](https://deno.land/x) on their site. In fact, we'll use some of them later in this course.

Deno also has a "Standard Library" that you will probably use quite a bit. These would be modules that are considered "built-in" by Node.

For instance, Node has a "path" module that is frequently used. For instance, if you wanted to get the file name from a path, you could use the `path` module and `basename` method.

    ```javascript
    const path = require("path");

    console.log(path.basename("/files/folders/folder/file.txt"));
    ```

The Deno standard library also has a "path" module. In Deno, the exact same functionality looks like this...

    ```typescript
    import * as path from "https://deno.land/std@0.73.0/path/mod.ts";

    console.log(path.basename("/files/folders/folder/file.txt"));
    ```

Both of these do the exact same thing and return the exact same result. The big difference is that you had to include the module by URL.

- Run the second one in the `app.ts` file
- Notice the output...

  ```bash
  Download https://deno.land/std@0.73.0/path/mod.ts
  Download https://deno.land/std@0.73.0/path/_constants.ts
  Download https://deno.land/std@0.73.0/path/separator.ts
  Download https://deno.land/std@0.73.0/path/glob.ts
  Download https://deno.land/std@0.73.0/path/win32.ts
  Download https://deno.land/std@0.73.0/path/posix.ts
  Download https://deno.land/std@0.73.0/path/_interface.ts
  Download https://deno.land/std@0.73.0/path/common.ts
  Download https://deno.land/std@0.73.0/path/_util.ts
  Download https://deno.land/std@0.73.0/_util/assert.ts
  file.txt
  ```

Deno downloads all of the dependences specified in the `mod.ts` file which we imported to get the whole path module.

Let's examine again what happened here by navigating to the generated output for the project...

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

The `app.ts` file now looks like this...

    ```javascript
    import * as path from "https://deno.land/std@0.73.0/path/mod.ts";
    console.log(path.basename("/files/folders/folder/file.txt"));
    //# sourceMappingURL=data:application/json;base64,eyJ2ZXJzaW9uIjozLCJmaWxlIjo...
    ```

But we know that Deno doesn't download that file everytime we run the program. We know that the first time we try and use it, Deno downloads it to the "deps" cache.

Linux

```
~/.cache/deno/gen/file/<path-to-deno-first-look-exercises>

// on my machine, it looks like this...
~/.cache/deno/deps/https/deno.land
```

Windows

```powershell
C:\Users\burkeholland\AppData\Local\deno\deps\https\deno.land

// on my machine, it looks like this...
C:\Users\burkeholland\AppData\Local\deno\deps\https\deno.land
```

The "deps" cache now contains a bunch more files. These are the depedencies that were downloaded to your machine. This is the equivilant of an "npm install". Deno uses the metadata files to find the dependencies you need right on your own machine. It won't try and download these again unless you change the version or force an update.

Now let's take a moment and address the elephant in the room - which is that importing modules from URL's feels....completely nuts.
