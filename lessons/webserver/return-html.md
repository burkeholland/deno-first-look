---
path: "/webserver/reading-parameters"
title: "Reading the query string"
order: "6B"
section: "Building a Webserver"
description: "Burke looks at how to build a simple web server with Deno"
---

## Reading query parameters

An important part of any web server is the ability to handle parameters. Let's modify this app so that it returns a "Hello" to whatever `name` parameter we pass in on the query string...

```bash
http://localhost:3000?name=World
```

The `req.url` object holds the URL fragment - which is everything that comes after the address and port. You can use the browser `URLSearchParams` object to parse out the query string parameters.

> Note that a "/" gets prepended to the "?" in a query string by the browser and URLSearchParams can't handle this. So we'll strip off the first character with substring().

- In the for loop, use the `URLSearchParams` object to parse the `req.url` and get the name.

  ```typescript
  for await (const req of server) {
    const searchParameters = new URLSearchParams(req.url.substring(1));
    const name = searchParameters.get("name");

    req.respond({
      body: "<h1>Hello World</h1>",
    });
  }
  ```

- Check to the see if the `name` parameter was passed in. If so, return it in the HTML. If not, throw a 500 server error.

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

- Run the program...

  ```bash
  deno run --allow-net=0.0.0.0:3000 app.ts
  ```

- Pass in the name parameter on the URL...

  ```bash
  http://localhost:3000?name=World
  ```

The browser should return whatever name you pass in. If not, you should get a 500 error with a messae saying that a "name" parameter is required.

This all works, but we don't normally serve HTML this way. Normally we serve up actual HTML files. Let's take a look at how to do that.
