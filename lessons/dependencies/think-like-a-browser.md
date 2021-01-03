---
path: "/dependencies/think-like-a-browser.md"
title: "Thinking like a browser"
order: "4A"
section: "4 - Understanding Dependencies"
description: "Burke dives into how Deno handles dependencies."
---

You'll remember from the earlier introduction section that Deno thinks differently about dependencies. It's a core design premise. But we didn't talk about _how_ it's different.

Deno tries to stay as close to browser conventions as possible everywhere that it can. And how do you include a JavaScript file in a browser? You reference it with a script tag. If it's on a CDN, you might do it like this...

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.12/dist/vue.js"></script>
```

Deno works the same way. You require in dependencies by URL. If you are requiring a local dependency, it's just the relative path. If you are using a third-party library, you do it by the URL.

Deno follows the [ECMAScript standard](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) for `import/export` syntax.

## Caching

Caching is an important part of how Deno handles dependencies. Since it uses the URL reference as the method to inlcude them, you wouldn't want it to try and download those everytime you ran the program.

Instead, Deno downloads a dependency the first time that you run the program and until the dependency changes or you force an update, that cache is where the dependency is served from.

How Deno handles dependencies is different based on whether or not you are using a local dependency (importing a file in the current project) or a remote dependency (referencing a third-party module by URL). In the next few sections, we'll look at how Deno handles these two scenarios, and some best practices for organizing dependenices in Deno.

## Getting Dependency Info

As we move through the next sections, the `deno info` command may come in handy. By itself, the command returns the location of your Deno cache. Try it out and see what you get...

```bash
deno info
```

Here's what I get...

```bash
DENO_DIR location: "/home/burkeholland/.cache/deno"
Remote modules cache: "/home/burkeholland/.cache/deno/deps"
TypeScript compiler cache: "/home/burkeholland/.cache/deno/gen"
```

We'll be looking at these directories going forward. If you get stuck or can't find the cache, a simple `deno info` should get you there.
