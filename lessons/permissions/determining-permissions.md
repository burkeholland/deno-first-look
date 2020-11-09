---
path: "/permissions/determining-permissions"
title: "Determining Permissions"
order: "4D"
section: "Permissions"
description: "Burke explores the unique permissions model of Deno."
---

The "permissions" API in Deno allows you to "query" for a specific permission to see if you have it or not. This gives you a chance to ask for it if it doesn't already exist. Although, if the permissions is already granted, the code requesting the permission again will not run, and the "state" will be "granted".

The default "state" of any permissions is always "prompt".

- Modify the `app.ts` file to query for the state of the "read" permission and log it out

  ```typescript
  const desc = { name: "read", path: "." } as const;

  console.log(await Deno.permissions.query(desc));

  const status = await Deno.permissions.request(desc);

  console.log(await Deno.permissions.query(desc));

  if (status.state === "granted") {
    const results = await Deno.readDir(".");
    for await (const result of results) {
      console.log(result);
    }
  } else {
    console.log("This program requires 'read' permissions to execute");
  }
  ```

- Execute the program with `deno run --unstable app.ts`.

You should see the status object in the terminal with an initial value of "prompt" and then "granted" or "denied" based on whether or not you chose to give the program access.

    ```bash
    PermissionStatus { state: "prompt" }
    ️⚠️  Deno requests read access to ".". Grant? [g/d (g = grant, d = deny)] d
    PermissionStatus { state: "denied" }
    This program requires 'read' permissions to execute
    ```
