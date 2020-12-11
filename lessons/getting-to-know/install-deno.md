---
path: "/getting-to-know/install"
title: "Installing Deno"
order: "3A"
section: "3 - Getting to know Deno"
description: "Burke covers how to install Deno, and takes a look at the Deno CLI, REPL and what options you need to be aware of."
---

Deno can be installed via cURL, Homebrew, PowerShell, Chocolatey - you name it. [Check the Deno docs](https://deno.land/#installation) for official instructions. For the sake of brevity, we'll use the cURL/PowerShell commands for this class.

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

That outputs "Hello World" because we did the console.log, and then it prints "undefined" because that's the return value of the console.log method. Every method in JavaScript returns a result - even if you don't explicitly do it.

Like the Node.js REPL, the Deno REPL will automatically add new lines when it sees that you are trying to write a multiline program. Try creating a new function which takes in a parameter and then outputs that with the world "Hello" in front. Then return the input parameter as the output.

```bash
>function hello(who) {
console.log(`Hello ${who}`);
return who;
}
```

When you add the closing brace, Deno REPL automatically exits out of edit mode. Note that Node.js does this same thing.

Unlike Node.js, the Deno does not support editor mode. In Node.js, you can have the REPL go into editor mode by specifying the ".editor" option in the REPL.

```bash
$> node
Welcome to Node.js v12.19.0.
Type ".help" for more information.
> .editor
// Entering editor mode (^D to finish, ^C to cancel)
```

Deno will just ignore that ".editor" command. However, we don't spend a lot of time coding in REPL's so - probably not a feature you're going to miss.

## What is Deno written in?

By the way, any guesses on what language the native bindings for Deno are written in? Both Deno and Node use V8 as the JavaScript runtime, but the bindings that allow you to reach out of V8 and onto the native operating system are different.

For Deno, it says right at the top, but it's more fun to look at source code, so let's do that instead.

If you look at the ["core" folder](https://github.com/denoland/deno/tree/master/core) in the Deno repo, you'll see that all the files have a `.rs` extension. That file extension is for the language Rust. Rust is a language that was designed for highly safe concurrent systems. What they mean by that is that it is memory safe. It's kind of like C++, but it doesn't allow things like null pointers. And it was written by the folks at Mozilla, with some contributions from Brendan Eich - whose name might ring a bell because he's the guy who created JavaScript. So we haven't fallen too far from the apple tree here. Node.js native bindings are written in a combination of C and C++.

You might remember I mentioned earlier that when Ryan created Node.js, he was mostly interested in I/O. That's important to note because as of right now, Node.js is faster in terms of HTTP server performance than Deno is, although according to [Deno's benchmarks](https://deno.land/benchmarks), the difference isn't stark.

![Benchmark for Deno vs Node HTTP Performance](../images/deno-vs-node-benchmarks.jpg)

So, not a huge gap to close for Deno. I've often heard this complaint about Deno when comparing it to Node.js, and I'm just not sure the difference is great enough to care and certainly not a gap too big for Deno to close. And besides, most of us are not working with systems that are going to experience the kind of load that is going to necessitate agonizing over a char like the one above.
