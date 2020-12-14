---
path: "/webserver/reading-parameters"
title: "Reading the query string"
order: "6B"
section: "6 - Building a Webserver"
description: "Burke looks at how to build a simple web server with Deno"
---

> Make sure you are on the "6-reading-the-query-string" branch to follow along with this section.

## Reading query parameters

An important part of any web server is the ability to handle parameters. These could be query string parameters or parameters passed on the body. Let's look at how to do both of those things with our simple web server.

Let's modify this app so that it returns a "Hello" when we pass in a `name` parameter on the query string. Like this...

```bash
http://localhost:3000?name=World
```

How do we get that "name" parameter? Well, we need to parse the query string to do that. The Deno web server is just that, and it doesn't come with an easy way to just get the query string parameters. But we can get them from the request.

The `req.url` object holds the URL fragment - which is everything that comes after the address and port. You can use the browser [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) object to parse out the query string parameters.

> Note that a "/" gets prepended to the "?" in a query string by the browser and URLSearchParams can't handle this. So we'll strip off the first character with substring().

In the for loop, use the `URLSearchParams` object to parse the `req.url` and get the name.

```typescript
for await (const req of server) {
  const searchParameters = new URLSearchParams(req.url.substring(1));
  const name = searchParameters.get("name");

  req.respond({
    body: "<h1>Hello World</h1>",
  });
}
```

Check to the see if the `name` parameter was passed in. If so, return it in the HTML. If not, throw a 500 server error.

```typescript
for await (const req of server) {
  const searchParameters = new URLSearchParams(req.url.substring(1));
  const name = searchParameters.get("name");

  if (name) {
    req.respond({
      body: `<h1>Hello, ${name}</h1>`,
    });
  } else {
    req.respond({
      status: 500,
      body: "The name parameter was not found on the request",
    });
  }
}
```

Run the program...

```bash
deno run --allow-net=0.0.0.0:3000 app.ts
```

Pass in the name parameter on the URL...

```bash
http://localhost:3000?name=World
```

The browser should return whatever name you pass in. If not, you should get a 500 error with a message saying that a "name" parameter is required.

## Route Parameters

But what about route parameters? Query strings are simple enough, but how can we parse out a route parameter such as "/name"? We can do that, but we'll need more than what Deno offers with its standard library. And, unfortunately, we'll need more than what we can from it's current third-party library offerings as well.

But what about npm?

Let's do a quick check. Yep - looks like there is a package called [route-parser](https://www.npmjs.com/package/route-parser) ready to go that will parse route parameters for us no problem. This is why Node is so great. Every problem is one package install away. Or maybe that's the problem with Node. I'll let you decide.

In any event, we're going to get a chance to see how we can use Node modules in our program.

### Using Node Dependencies

We can use _some_ Node modules in Deno. If they have strong Node runtime dependencies, they probably won't work. But this one says it's isomorphic. That means that it works in Node and in the browser. If it works in the browser, there is a high probability it will work in Deno.

But how do we import a Node module into Deno? What is the URL for a Node module.

There are a few services out there that will allow you to import a package from npm via URL. One of them is called "unpkg". Unfortunately, this won't work for us in Deno and that's because of the fact that Deno only supports ES modules. A lot of Node modules predate that syntax and offer CommonJS `module.exports` syntax. Let's look at this "route-parser" code what it's doing. If you visit the "lib" folder in the repo and look at the ["route.js"](https://github.com/rcs/route-parser/blob/master/lib/route.js) file. The last line says...

```javascript
module.exports = Route;
```

That's CommonJS. It's not going to work with Deno. So what do we do? Well, believe it or not there is a service called "jspm" which will take a module by URL and give you an ES Module compatible import/export. All we have to do is run this module through that service and it will let us import the module...

```typescript
import routeParser from "https://dev.jspm.io/route-parser@0.0.5";
```

But we've got TypeScript here. Which means we need type definitions to use this library. How do we import those?

First, the types have to exist. Fortunately for us, searching for "route parser types" in npm will reveal that the "DefinitelyTyped" library has typings for "route parser".

We need this file served up raw. In that case, the "unpkg" service will work just fine...

```typescript
import routeParser from "https://dev.jspm.io/route-parser@0.0.5";
import RouteParser from "https://unpkg.com/@types/route-parser@0.1.3/index.d.ts";
```

Now we just need to tell TypeScript that `routeParser` is type of `RouterParser`...

```typescript
import routeParser from "https://dev.jspm.io/route-parser@0.0.5";
import RouteParser from "https://unpkg.com/@types/route-parser@0.1.3/index.d.ts";

const Route = routeParser as typeof RouteParser;
```

And now we can use it just like the "route-parser" docs show. Let's return the name if we find it and if not, return a static "404.html" page.

```typescript
import { serve } from "https://deno.land/std/http/server.ts";

import routeParser from "https://dev.jspm.io/route-parser@0.0.5";
import RouteParser from "https://unpkg.com/@types/route-parser@0.1.3/index.d.ts";

const Route = routeParser as typeof RouteParser;

const PORT = 3000;
const HOSTNAME = "0.0.0.0";

const server = serve({ hostname: HOSTNAME, port: PORT });

console.log(`Server is now running on: http://${HOSTNAME}:${PORT}`);

const route = new Route("/:name");

for await (const req of server) {
  const match: any = route.match(req.url);
  if (match.name) {
    req.respond({ body: `Hello, ${match.name}` });
  } else {
    req.response({ body: "Please pass a name route." });
  }
}
```

As you can see - it's a little hacky. It's not the best solution, but it _does_ work. It also shows you what it's like to import type definitions for a file that isn't TypeScript.
