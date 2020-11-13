---
path: "/webserver/handling-routes"
title: "Handling Routes"
order: "6D"
section: "Building a Webserver"
description: "Burke looks at how to build a simple web server with Deno"
---

We can use the "req.url" object to determine the path and read the proper file.

- Modify the `for` loop by adding a `switch` statement to determine the route and serve the proper file.

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

At this point, we're building out a full web application and our simple server probably isn't going to scale. We're going to need to handle templates, a better routing solution, HTTP verbs - there is a lot to implement here. Instead of building our own web server from scratch, let's take a look at the Oak web framework for Deno.
