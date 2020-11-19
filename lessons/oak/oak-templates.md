---
path: "/oak/oak-templates"
title: "Templates"
order: "7C"
section: "Oak web framework"
description: "Burke introduces the Oak web framework for Deno"
---

> Make sure you are on the "oak-templates" branch for this section in the "exercise" folder. The completed project can be found in the "solution" folder.

A key benefit of web servers is being able to dynamically compose templates. In order to do that with Oak, we need to add a templating engine. In this section, we'll add the templating engine and change the router to render the template instead of just text.

## Finding a template engine

Deno has a surprising amount of third-part libraries to choose from given how new it is. If we [search Deno's third party registry](https://deno.land/x?query=template) for template libraries, we see there are quite a few to choose from. This includes some well known libraries like Mustache and Handlebars. Given that I usually opt for Handlebars with Express, we'll be using that one for this Oak project.

![](../images/deno-templates.jpg)

> Note that there are "view engines" for Deno. We won't be using any of those options in this workshop because I felt like they inject too much complexity into the conversation. Libraries like the "view_engine" project do provide some enhanced functionality, such as caching.

If you click on the "handlebars" option, you'll see the README which shows the URL that you'll need to import.

![](../images/handlebars-readme.jpg)

## Import Handlebars into the Oak project

- Add an import for the Handlebars library to the `deps.ts` file. Make sure you export "Handlebars"

  ```typescript
  import { Application, Router } from "https://deno.land/x/oak/mod.ts";
  import { Handlebars } from "https://deno.land/x/handlebars/mod.ts";

  export { Application, Router, Handlebars };
  ```

> I find it interesting that while Deno aims to avoid centralizing dependencies, it has effectively done so by creating this repository at deno.land/x. We may very well see npm again by another name.

To use the Handlebars library, we need to initialize an instance and pass in some configuration. To do this, we'll create a shared object that the routers can import and then use to return template rendered views.

- Create a folder in the root of the project called "shared"
- Create a file inside called "hbs.ts"
- Import Handlebars from the "deps.ts" file

  ```typescript
  import { Handlebars } from "../deps.ts";
  ```

- Create a new instance of the Handlebars class and pass in the following configuration object. Export the new instance.

  ```typescript
  import { Handlebars } from "../deps.ts";

  const handlebars = new Handlebars({
    baseDir: "views",
    extname: ".hbs",
    layoutsDir: "layouts/",
    partialsDir: "",
    defaultLayout: "main",
    helpers: undefined,
    compilerOptions: undefined,
  });

  export default handlebars;
  ```

These options are all the defaults with the exception of the empty string for the `partialsDir` option. This is because we won't be using partial views for this course and if we don't override the default value (`partials/`), a "file not found" error will be thrown by the Handlebars library.

Before we can actually use these templates, we need to create them.

- Add a folder to the exercise root project called "views"
- Add a "layouts" directory in the "views" directory
- Create a "main.hbs" file in the "views/layouts" directory
- Add the following HTML to the "main.hbs" file

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Oak</title>
    </head>
    <body>
      {{{ body }}}
    </body>
  </html>
  ```

  This file serves as the main layout template. The `{{{ body }}}` is where we'll render different views depending on which URL is requested.

- Create an "index.hbs" file in the "views" folder.
- Add the following markup

  ```html
  <h1>{{ title }}</h1>
  <p>Welcome to {{ title }}</p>
  ```

  This fragment will be rendered where the `{{{ body }}}` tag is in the "main.hbs" file.

- Import "shared/hbs.ts" in the "indexRoutes.ts" file.

  ```typescript
  import { Router } from "../deps.ts";
  import hbs from "../shared/hbs.ts";
  ```

-- Call the `renderView` method on the `hbs` object to render the view as the body response.

    ```typescript
    import { Router } from "../deps.ts";
    import hbs from "../shared/hbs.ts";

    export function use(path: string, router: Router) {

      router.get(`${path}`, async (ctx) => {
        ctx.response.body = await hbs.renderView("index", { title: "Oak" });
      });

    }
    ```

**Bonus** - Create a "users" template and handle the view rendering in the "userRoutes.ts" file.
