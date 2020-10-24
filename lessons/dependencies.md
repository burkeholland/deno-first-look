---
path: "/dependency-hell"
title: "Dependency Hell"
order: "2C"
description: "Burke looks at what the current state of dependencies in Node.js is, and why Deno has a fundamentally different approach."
section: "What is Deno"
---

Dependencies have beena topic of ongoing controversy for a long time. Across all platforms. People have different opinions about how dependencies should be resolved, packaged, referenced and the like. The reason for this is that dependencies often take dependencies of their own and those dependencies have dependencies. And that just explodes into a million dependencies to do something very simple.

So what's wrong with the way that Node.js handles dependencies? Well, there are a few things that...maybe could be handled differently.

## npm and Single Points of Failure

First off, npm is the central package repository for Node. Yes, technically anyone can setup a package repository and you can have private ones as well. But by and large, nearly the entire community is served by npm. Is that a bad thing? Maybe, maybe not. You may recall that last year there was some escalating issues with npm as it tried to generate enough revenue to stay afloat. Serving up packages to everyone in the world is an expensive endeavor. I don't know if you've ever looked at cloud bandwidth bills, but they are frequently the lions share of cloud computing costs. And npm is absorbing that every day. GitHub stepped in and purchased npm earlier this year, possibly preventing what could have been the collapse of essentially where all our Node packages live.

Again, is a single package repository a bad thing? Maybe not. It's incredibly convenient to have a central place to look for any code you might want to use and not have it strewn about over the internet. It's probably a driving force behind why Node's adoption was so quick as well.

But for that convenience we get a single point of failure and it is now owned by a corporation. Some people may not feel great about those two realities.

### package.json

Believe it or not, one of the things that Brian regrets is the `package.json` file. I find this to be a nitpick, but he feels like it's a lot of boilerplate and for what? He's got a point there.

Create a new folder and in that folder, execute the following command...

```javascript
npm init
```

You get asked about a dozen questions and it's not clear why you need any of this information. What's an entrypoint? Do you need one? Why do you need a version? Keywords? These things might be important if you are creating an npm package, but most of us are not doing that. The `package.json` is usually full of information that you don't really need. It's extraneous.

```json
{
  "name": "2-dependency-hell",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

Now, it should be noted that you can execute the shorthand...

```javascript
npm init -y
```

Which won't ask you any questions, but still gives you the same output.

This is not necessarily a bad thing. The `package.json` file lets you specify a lot of metadata that a lot of libraries do use. For instance, when you publish a VS Code extension, it needs some fields in that `package.json` in order to fill out the page for your extension correctly.

One last point about the `package.json` is that it contributes to the bloat of configuration files in a repo. Just look at the number of config files in the static site code for this course...

![static site dependencies](images/static-site-deps.jpg)
