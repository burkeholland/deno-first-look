---
path: "/dependencies/best-practices.md"
title: "Best Practices"
order: "4D"
section: "4 - Understanding Dependencies"
description: "Burke dives into how Deno handles dependencies."
---

We've already established that Deno would like to stay as close to browser conventions as possible, and in that case, importing from a URL feels less icky. But it's sure not very pleasant to look at. You wouldn't really want a bunch of imported URL's just hanging out all over your code. That's no way to live your life.

Deno recommends that you do all of your importing in a root file called `deps.ts` and export from that single file. That way all of your deps are in the same place, and all of the URL's are centralized to that one file. Later on in this workshop, we'll be building a full website with Deno and the `deps.ts` file for that project looks like this...

```typescript
import {
  Application,
  Context,
  Router,
  send,
} from "https://deno.land/x/oak/mod.ts";
import { Handlebars } from "https://deno.land/x/handlebars/mod.ts";

export { Application, Context, Router, send, Handlebars };
```

Then our application code stays clean, and only references the `deps.ts` file.

The common pushback I hear on this idea is "I can't install the module if I'm on an airplane". That is true. But you also can't install npm modules if you are on an airplane. Nothing changes here. npm package have to be downloaded. Deno modules have to be downloaded. Period. The only difference is that Deno is using the URL as the top level construct on which to identify the module, and npm downloads it and uses a specific location - node_modules.

For those who still aren't convinced, let me leave you with this thought: Someone once suggested that we put HTML in JavaScript. I laughed at that person when I first heard that suggestion. Today, React is quite possibly the most popular JavaScript framework in the world, and we don't think twice about putting HTML in our JavaScript. In fact, it works quite well. In my humble opinion, having your markup and code in the same component file is BETTER than not. It's a better developer experience.

I'm not saying that Deno is the React of JavaScript runtimes. I'm not saying that importing from URL's is going to ultimately be the better developer experience. But I am saying that sometimes we have to allow ourselves to examine possiblities that don't look so good on the surface.

Give it a chance. I think you'll find that it's a relatively simple solution to the dependency problem.
