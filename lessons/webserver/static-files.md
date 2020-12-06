---
path: "/webserver/static-files"
title: "Servering up static files"
order: "6C"
section: "6 - Building a Webserver"
description: "Burke looks at how to build a simple web server with Deno"
---

> Make sure you are on the "6-serving-up-static-files" branch to follow along with this exercise.

Normally, we serve up HTML files with a web server. These are also called "static files", because the server doesn't change them, it just serves them. They are static.

One way to serve a static file would be to read the file with the Deno object. There is an "index.html" file already in the project. Let's use the `Deno` object to read that file off of disk and serve it up.

Modify the `for` loop to use the `Deno.readFile` method to read the "index.html" file.

```typescript
for await (const req of server) {
  const html = await Deno.readFile("index.html");

  req.respond({
    body: html,
  });
}
```

Run the app. Since we're reading the disk now, we'll have to pass in the `--allow-read` flag as well.

```bash
deno run --allow-net=0.0.0.0:3000 --allow-read=. app.ts
```

This is one way to serve a static file. But Deno provides a "file server" module as part of its Standard Library

## Serving static content with Deno Standard Library

Import the "file-server" module from the Deno standard library.

```typescript
import { serveFile } from "https://deno.land/std@0.77.0/http/file_server.ts";
```

Modify the `for` loop to use the `serveFile` method to serve up the "index.html" file.

```typescript
for await (const req of server) {
  const html = await serveFile(req, "index.html");

  req.respond(html);
}
```

Run the program with the "--allow-net" and "--allow-read" flags

```bash
deno run --allow-net=0.0.0.0:3000 --allow-read=. app.ts
```

> Note that you can pass `-A` which stands for "allow all" so you don't have to pass multiple permissions flags. Handy for development."

Now we're serving up static files. But a proper web server needs to handle routes.
