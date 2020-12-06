---
path: "/getting-to-know/first-program"
title: "Your first program"
order: "3C"
section: "3 - Getting to know Deno"
description: "Burke goes over how to write your first program with Deno and execute it with the Deno CLI."
---

> Switch to the "3-your-first-program" branch to follow along with this section.

Deno programs can be written in TypeScript or JavaScript. Either will work.

In the "app.ts" file in the "exercise" folder, write out "Hello World" to the terminal.

```typescript
console.log("Hello World");
```

Execute the file with Deno from your terminal. Make sure you are in the exercise directory where the "app.ts" file is located.

```bash
deno run app.ts
```

You should see the following output...

```bash
Check file:///home/burkeholland/dev/deno-first-look-exercises/app.ts
Hello World
```

Notice that the terminal says "Check <file-path/app.ts>". There is then a bit of delay before that "Hello World" shows up.

Run the program a second time

```bash
deno run app.ts
```

This time it doesn't say "Checking". This time it just runs and "Hello World" shows up immediately.

In order to understand what's happening here, we need to understand what Deno is doing when it "Checks" a file.

## Deno run - behind the scenes

While TypeScript is a first-class citizen in Deno, it still has to be JavaScript before V8 can run it. Deno has to convert the file to TypeScript using the TypeScript compiler/transpiler. When it does this, it creates a "build" of your file on your system. Let's investigate that.

Navigate from your terminal to the "deno" cache directory. This will be different if you are on Windows vs Linux. You can use the "deno info" command from your terminal and it will tell you exactly where your cache folder is.

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
C:\Users\burkeholland\AppData\Local\deno\gen\file\C\dev\burkeholland\deno-first-look-windows
```

Inside that folder, you will find several files...

- app.ts.bundleinfo
- app.ts.js
- app.ts.metadata

Open this folder in VS Code by executing the following command in the terminal from the same folder where these files are located.

```bash
code .
```

This is the output of your program. The "app.ts.bundleinfo" file contains your program's build info. It has paths to TypeScript definition files.

If you scroll all the way down to the bottom, you'll see an embeded ".tsconfig"

The "app.ts.js" file contains the JavaScript output of your code - minified and with sourcemaps.

The `app.ts.meta` contains information concerning the version of your file. Deno is using these files to determine what dependencies your program has, what version it is, ect. We'll look at this again later on and you'll understand more about _why_ Deno is doing all of this.

In summary, Deno is the compiler and it checks code for errors the way any strongly typed language compiler would.

## Running without checks

You can go around the TypeScript checking all-together by passing the "--no-check" flag to the run command.

Modify the app.ts to contain the following valid TypeScript code...

```typescript
const message: string = "Hello World";
console.log(message);
```

Run it with the `--no-check` flag...

```bash
deno run --no-check app.ts
```

This time the app runs without the check. There is still a slight delay as the code is transpiled, but it's faster because it's not checked for errors first.

If you run the code again, it will be even faster because nothing has changed, so it doesn't need to be transpiled again.

## Executing JavaScript

In the same way that you execute TypeScript, you can execute plain JavaScript.

In the "app.js" file, add a line to print out "Hello World".

```javascript
console.log("Hello World");
```

Run the file from your terminal with Deno.

```bash
deno run app.js
```

Notice that there is no check this time and the result is immediate. And there is nothing in the "gen" folder because there is no generation that needs to happen.

When executing JavaScript files in Node, you don't need to pass a file extension. This is why Express apps have a file called "bin/www" with no extension.

Deno doesn't support this and throws an error. You need to specify the file extension. This is something that will be important when we talk about dependencies as well.

```bash
node run app
Hello  World


deno run app
error: Cannot resolve module "file:///C:/dev/burkeholland/deno-first-look-windows/app"
```

## Executing remote scripts

Deno can also execute scripts that are remote. That means that you literally call them with a URL. This is why the Deno docs have you call their sample "Hello World" program. This feels awfully unintuitive for a JavaScript developer. We explicity do **not** execute scripts from other sources because we don't know what they are going to do. But since Deno is "secure by default" as we discussed earlier, the model changes and Deno actually expects you to include code from remote locations.

Instead of running our local "app.ts" file, execute the remote "Hello World" example that Deno provides...

```bash
deno run https://deno.land/std@0.76.0/examples/welcome.ts

Download https://deno.land/std@0.76.0/examples/welcome.ts
Check https://deno.land/std@0.76.0/examples/welcome.ts
Welcome to Deno 🦕
```

Notice that first Deno downloads the file, then it sends it through the compiler/transpiler, then executes it. Normally, we would consider this a highly dangerous thing to do, because we don't know what this file is or what it might attempt! But because Deno will not allow any program to access your files system, the net or do virually anything else without your permission, this is ok.

But where is this file downloaded to? For that, we need to go back to the cache.

## Where Deno downloads dependencies

Deno treats this file as a dependency, and downloads it to the same place it downloads all dependencies. This file isn't really a dependency that we are using in our program - it IS the program. But Deno treats it the same way it treats dependencies.

Navigate from your terminal to the "deno.land" directory in the "deno" cache. This will be different if you are on Windows vs Linux...

Linux

```
~/.cache/deno/deps/https/deno.land
```

Windows

```powershell

// on my machine, it looks like this...
C:\Users\burkeholland\AppData\Local\deno\deps\https\deno.land
```

In that folder, provided you have never worked with Deno before, you should see 2 files both with really long names. One has no extension and the other has a `.metadata.json` extension. Something like this...

```bash
d85d61a352596fd18eac16f29315093cc2e293166b66528a92e171492992f148
d85d61a352596fd18eac16f29315093cc2e293166b66528a92e171492992f148.metadata.json
```

You can open that first file - the one without the extension - in your editor, and you'll see the code in the "welcome.ts" file that you ran from "https://deno.land/std@0.76.0/examples/welcome.ts"...

```typescript
// Copyright 2018-2020 the Deno authors. All rights reserved. MIT license.
console.log("Welcome to Deno 🦕");
```

Deno downloaded that file, cached it, transpiled it to JavaScript and executed it.
