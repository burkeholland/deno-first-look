---
path: "/permissions/requesting-permissions"
title: "Requesting Permissions"
order: "5B"
section: "Permissions"
description: "Burke explores the unique permissions model of Deno."
---

> Note that as of the time of this writing the permissions API is behind the "unstable" flag.

Deno has a permissions API that will allow you to request certain access from your code as a library author.

- At the `app.ts` file, define a `const` object which specifies the name of the "read" permission.

  ```typescript
  const desc = { name: "read" } as const;
  ```

- Pass the descriptor object to the "request" method on "Deno.permissions". Because the permissions API is still unstable, it does not show up in intellisense under the "Deno" object.

  ```typescript
  const desc = { name: "read" } as const;
  const status = await Deno.permissions.request(desc);
  ```

The "status" variable will contain a "state" object after the user has either granted or denied permissions. If the permission is granted, the state will be "granted". If it is denied, then "denied".

- Check the `state` object on the `status` constant to make sure permission has been granted. If so, list all the files in the current directory.

  ```typescript
  const desc = { name: "read", path: "." } as const;
  const status = await Deno.permissions.request(desc);

  console.log(status);

  if (status.state === "granted") {
    const results = await Deno.readDir(".");
    for await (const result of results) {
      console.log(result);
    }
  } else {
    console.log("This program requires 'read' permissions to execute");
  }
  ```

- Run the program from the terminal with `deno run --unstable app.ts`.

You should see a prompt in the terminal asking you if you want to grant or deny permission to "read".

    ```bash
    Check file:///home/burkeholland/dev/deno-first-look-exercises/app.ts
    ️⚠️  Deno requests read access. Grant? [g/d (g = grant, d = deny)]
    ```

- Click the "g" button to grant or "d" to deny and view the output.

> Note that if you run this program with the `--allow-read` flag, the permissions request will be ignored and the "status.state" value will be "granted".
