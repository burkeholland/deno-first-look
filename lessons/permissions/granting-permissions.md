---
path: "/permissions/granting-permissions"
title: "Granting Permissions"
order: "5A"
section: "5 - Permissions"
description: "Burke explores the unique permissions model of Deno."
---

> Make sure you are on the [5-granting-permissions](https://github.com/burkeholland/deno-exercises/tree/5-granting-permissions) branch to follow along with this section.

One of the core tenants of Deno is "secure by default". As we discussed in the section of the same name, this means that by default, Deno can't do very much without your permission. It can't read your file system, it can't make HTTP calls - it's completely sandboxed. 

In the "app.ts" file, attempt to read all of the files in the current directory. This can be done with the "Deno" object "read" method.

```typescript
const results = await Deno.readDir("../");

for await (const result of results) {
  console.log(result);
}
```

Execute "app.ts" from the terminal

```bash
deno run app.ts
```

This should throw an error saying that you don't have read access.

```bash
error: Uncaught (in promise) PermissionDenied: read access to ".", run again with the --allow-read flag
at processResponse (core.js:226:13)
at Object.jsonOpAsync (core.js:244:12)
at async Object.[Symbol.asyncIterator] (deno:cli/rt/30_fs.js:125:16)
at async file:///home/burkeholland/dev/deno-first-look-exercises/app.ts:3:18
```

It says to run again with the "--allow-read" flag. Let's do that.

```bash
deno run --allow-read app.ts
```

This time Deno reads the contents of the current directory and writes them out to the terminal.

```bash
{ name: "README.md", isFile: true, isDirectory: false, isSymlink: false }
{ name: ".git", isFile: false, isDirectory: true, isSymlink: false }
{ name: "app.ts", isFile: true, isDirectory: false, isSymlink: false }
{ name: ".vscode", isFile: false, isDirectory: true, isSymlink: false }
{ name: "utils.ts", isFile: true, isDirectory: false, isSymlink: false }
```

Note that by default, the `--allow-read` flag lets the program read anything that the executing account can read. It's possible to pass a path to the `--allow-read` flag that restricts the access to a certain directory.

Run the program again with the following command

```bash
deno run --allow-read=. app.ts
```

The program now fails. This is because we didn't give it enough access. We said it could read the current directory "exercise", but the program is trying to read the parent directory.

Try it again passing in the parent path...


```bash
deno run --allow-read=../ app.ts
```

The program should run successfully.

Passing flags like this is how you explicitly give code elevated access. But it is not the only way. The other way to get elevated access in Deno is to request it from the code itself. This is more akin to the way that apps on your phone will ask for access to your location, photos, notifications, etc.
