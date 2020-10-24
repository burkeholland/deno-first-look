---
path: "/install"
title: "Installing Deno"
order: "3A"
section: "Installing Deno"
description: "Burke covers how to install Deno, and takes a look at the Deno CLI, REPL and what options you need to be aware of."
---

Deno can be installed via cURL, Homebrew, PowerShell, Chocolatey - you name it. [Check the Deno docs](https://deno.land/#installation) for official instructions. For the sake of brevity, we'll use the cURL/Powershell commands for this class.

#### cURL

```bash
curl -fsSL https://deno.land/x/install/install.sh | sh
```

#### PowerShell

```powershell
iwr https://deno.land/x/install/install.ps1 -useb | iex
```

That's it. You can verify that deno is installed by opening a terminal and just running Deno. This opens the Deno REPL.

```bash
$> deno
```

> REPL stands for "Run Eval Print Loop"

Go ahead and write some JavaScript there - maybe a nice `console.log`. That seems like the write thing to do...

```bash
Deno 1.4.6
exit using ctrl+d or close()
> console.log("Hello World");
Hello World
undefined
>
```

That outputs "Hello World" because we did the console.log, and then it prints "undefined" because that's the return value of the console.log method. Every method in JavaScript returns a result - even if you don't explicity do it.

Like the Node.js REPL, the Deno REPL will automatically add new lines when it sees that you are trying to write a multline program. Try creating a new function which takes in a parameter and then outputs that with the world "Hello" in front. Then return the input parameter as the output.

````bash
>function hello(who) {
console.log(`Hello ${who}`);
return who;
}
```bash

When you add the closing brace, Deno REPL automatically exits out of edit mode. Note that Node.js does this same thing.

By the way, any guesses on what language the core runtime of Deno is written in? It says right at the top, but it's more fun to look at source code, so let's do that instead.

If you look at the ["core" folder](https://github.com/denoland/deno/tree/master/core) in the Deno repo, you'll see that all the files have a `.rs` extension. That file extension is for the language Rust. Rust is a language that was designed for highly safe concurrent systems. What they mean by that is that it is memory safe. It's kind of like C++, but it doesn't allow things like null pointers. And it was written by the folks at Mozilla, with some contributions from Brendan Eich - whose name might ring a bell because he's the guy who created JavaScript. So we haven't fallen too far from the Apple tree here. Node.js uses V8 as it's runtime, which is written in C++, and it's native bindings are written in C.
````
