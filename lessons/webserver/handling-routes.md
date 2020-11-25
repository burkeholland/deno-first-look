---
path: "/webserver/handling-routes"
title: "Handling Routes"
order: "6D"
section: "Building a Webserver"
description: "Burke looks at how to build a simple web server with Deno"
---

Just like we have to implement our own query string parsing, we have to implement our own routing solution as well. A simple and naitve way is to just match the `req.url` object to determine the path and read a file.

Modify the `for` loop by adding a `switch` statement to determine the route and serve the proper file.

```typescript
for await (const req of server) {
  let fileToServe = "";
  switch (req.url) {
    case "/":
      fileToServe = "index.html";
      break;
    case "/about":
      fileToServe = "about.html";
      break;
    default:
      fileToServe = "404.html";
  }

  const html = await serveFile(req, fileToServe);
  req.respond(html);
}
```

This works, but certainly doesn't scale. We would need to fully parse out route parameters, handle wildcards and much, much more. A proper web framework usually takes care of all of these things for you. If you've used Node before, you are familiar with frameworks like Express. ASP.NET has MVC.

Deno has a web framework as well. It's called Oak, and it will most likely be your go-to for any sort of real-world web solution you might be building. In the next section we'll take a look at Oak and see how to use it to build a full application with routing, templating, error handling and more.
