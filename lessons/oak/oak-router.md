---
path: "/oak/oak-router"
title: "The Oak Router"
order: "7C"
section: "7 - Oak web framework"
description: "Burke introduces the Oak web framework for Deno"
---

> Make sure you switch to the "7-oak-router" branch to follow along with this section.

Oak contains prebuild Router middleware that will allow you to handle different routes, parameters and queries. It's basically doing a lot of string parsing and pattern matching that you would otherwise have to do yourself.

Include the "Router" object in the Oak import.

```typescript
import { Application, Router } from "https://deno.land/x/oak/mod.ts";
```

Create a new "Router" instance below the "Application" instance.

```typescript
const app = new Application();
const router = new Router();
```

Create a handler for a "GET" request to "/" - or the root route.

```typescript
import { Application, Router } from "https://deno.land/x/oak/mod.ts";

const app = new Application();
const router = new Router();

router.get("/", (ctx) => {
  ctx.response.body = "Welcome to Oak";
});
```

Tell the `app` object to use the `router` as middleware.

```typescript
import { Application, Router } from "https://deno.land/x/oak/mod.ts";

const app = new Application();
const router = new Router();

router.get("/", (ctx) => {
  ctx.response.body = "Welcome to Oak";
});

app.use(router.routes());

console.log(`Now listening on http://0.0.0.0:3000`);
await app.listen("0.0.0.0:3000");
```

Run the app with `deno run --allow-net app.ts`

You should see the same thing you had before - a simple page saying "Hello Oak". Note, though, that this route will only respond to the root route, and only if the request is a GET. Before, the application would respond to any request at all.

## Adding additional routes

Add an additional "users" route that responds to the "/users" route. It should come before the `app.use(router.routes())` line.

```typescript
router.get("/", (ctx) => {
  ctx.response.body = "Welcome to Oak";
});

router.get("/users", (ctx) => {
  ctx.response.body = `Welcome User`;
});
```

Add a third route which also listens to the "/users" route, but listens for an additional "/name" param and echoes the value back out.

```typescript
router.get("/", (ctx) => {
  ctx.response.body = "Welcome to Oak";
});

router.get("/users", (ctx) => {
  ctx.response.body = `Welcome User`;
});

router.get("/users/:name", (ctx) => {
  ctx.response.body = `Welcome ${ctx.params.name}`;
});
```

Run the application again and test the different routes. Try passing in "localhost:3000/users/YOUR_NAME". It should be echoed back out to you.

## Organizing routes like Express

The Express generator puts all the routes in files that correspond to their base route. For instance, the "/" routes are contained in the "routes/index.js" file. The "/user" routes are all contained in the "routes/users.js" file. In this section, you'll refactor your code to move the routes into their own folder.

### Creating a deps.ts file

In order to do that, we're going to need to access our dependencies from more than one file. Instead of importing URL's all over the application, let's move all our imports into a single "deps.ts" file.

- Cut the Oak import line from the top of the "app.ts" file.
- Create a file in the root called "deps.ts"
- Paste in the Oak import
- Export the `Application` and `Router` objects.

```typescript
import { Application, Router } from "https://deno.land/x/oak/mod.ts";
export { Application, Router };
```

In the "app.ts" file, import the `Application` and `Router` objects from the "deps.ts" file

```typescript
import { Application, Router } from "./deps.ts";
```

### Creating routes files

In order to specify routes in the route files, we need a reference to the router object that was initialized in the "app.ts" file. Deno supports having multiple routers. The easiest way to implement this is to make each route file create and export a new router object.

- Create a folder called "routes"
- Create a file called "indexRouter.ts" in the "routes" folder.
- Import the "Routes" object from the "deps.ts" file

```typescript
import { Router } from "deps.ts";
```

Create a new instance of the router object

```typescript
import { Router } from "deps.ts";
```

Create a function called "use" which will accept a path and a route. We'll use that path and route to configure all of the index routes.

```typescript
import { Context, Router } from "../deps.ts";

export function use(path: string, router: Router) {}
```

Move the index route to this new "indexRouter.ts" file

```typescript
import { Context, Router } from "../deps.ts";
import hbs from "../shared/hbs.ts";

export function use(path: string, router: Router) {
  router.get(`${path}`, async (ctx: Context) => {
    ctx.response.body = "Welcome to Oak";
  });
}
```

We use the "path" variable and append all routes to that object. That's so that we can specify the path in the "app.ts" file for all our routers. As we add more and more routers, it's going to be important to know what router is handling what path by just by looking at the "app.ts" file.

Import the new file as `indexRouter` at the top of the "app.ts" file

```typescript
import { Application, Router } from "./deps.ts";
import * as indexRouter from "./routes/indexRouter.ts";
```

Tell the router to the "/" path and the existing "router" object.

```typescript
indexRouter.use("/path", router);
```

Run the app again to make sure the index route still works.

Repeat the same thing for the "user" routes with a file called "routes/userRouter.ts". Your final "app.ts" should look like this...

```typescript
import { Application, Router } from "./deps.ts";
import * as indexRouter from "./routes/indexRouter.ts";
import * as userRouter from "./routes/userRouter.ts";

const app = new Application();
const router = new Router();

indexRouter.use("/", router);
usersRouter.use("/users", router);

console.log(`Now listening on http://0.0.0.0:3000`);
await app.listen("0.0.0.0:3000");
```

Now that we've got a proper routing structure in place, let's turn our attention to the next big problem to solve - and that's how to use templates to return HTML instead of just returning text.
