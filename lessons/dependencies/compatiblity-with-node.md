---
path: "/dependencies/compatibility-with-node"
title: "Compatibility With Node"
order: "4E"
section: "4 - Understanding Dependencies"
description: "Burke dives into how Deno handles dependencies."
---

## About npm compatibility

One of the first questions that might pop into your head here is that there is a problem: Node is successful due to the massive library of packages. Are those packages compatible with Deno?

Probably not.

Deno **does** have a [Node compatibility module](https://deno.land/std@0.67.0/node/README.md) in its standard library. In theory, at some point, you would be able to import this module and all Node code would "just work". However, as of the time of this writing, of the 38 built-in Node modules, only 8 are supported in the Deno compatibility module.

Furthermore, if the npm package you are using does any system calls, it will be built with `node-gyp`. Deno simply doesn't support this. It's built with Rust and has an entirely different compiler called Cargo for bindings. This means that a lot of Node modules are simply going to "not work". However - there's a lot of smart people out there and I'm sure that this is a problem that will be solved at some point.

Also, the third party list for Deno is growing rapidly already. As I typed this section I checked for my least favorite `node-gyp` requiring package - which is Sass. There is already a Sass library that you can use with Deno.
