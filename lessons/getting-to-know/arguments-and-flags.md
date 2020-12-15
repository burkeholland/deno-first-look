---
path: "/getting-to-know/arguments-and-flags"
title: "CLI Tour"
order: "3E"
section: "3 - Getting to know Deno"
description: "Burke looks at how to pass arguments and flags to the Deno CLI"
---

> Switch to the [3-cli-tour](https://github.com/burkeholland/deno-exercises/tree/3-cli-tour) branch to follow along with this exercise.

For a full list of arguments you can pass to Deno, execute `deno -h`.

There are quite a few built-in features for Deno, including a formatter, a linter and a built-in test runner. For any of the built-in arguments, you can select the argument and the pass `-h` again after the argument to find out what you can do with a command, what options are available and what flags you can pass.

Inspect the `fmt` argument using the `-h` command.

```bash
deno fmt -h
```

Change the formatting in the `app.ts` file so that a default value is passed for the message if `args` doesn't exist. Make sure to use single quotes.

```typescript
const message: String = Deno.args[0] || 'World';
console.log(`Hello ${message}`);
```

Run the formatter on the `app.ts` file

```bash
deno fmt app.ts
```

Notice that the formatter changes the single-quotes to double-quotes. Formatting in Deno is done by the [dprint library](https://github.com/dprint/dprint) with the default configuration. As of the time of this writing, there is no way to pass a configuration to dprint. My personal preference is to use Prettier and let the editor worry about formatting. The same goes for linting.

## Passing in Arguments

Arguments can be passed to the Deno CLI after the path of the file to be executed.

```bash
deno run app.ts a b c
```

These arguments are referenced within the file by looking at the `Deno.args` object.

Modify the code in "app.ts" to log out all incoming arguments...

```typescript
console.log(Deno.args);
```

Run the code from the terminal. Pass in the values a, b and c as args

```bash
deno run app.ts a b c

Check file:///home/burkeholland/dev/deno-first-look-exercises/app.ts
[ "a", "b", "c" ]
```

Modify the code to echo out the value of the argument passed in and concatenate it with "Hello"

```typescript
const message: string = Deno.args[0];
console.log(`Hello ${message}`);
```

Run the program

```bash
deno run app.ts World

Check file:///home/burkeholland/dev/deno-first-look-exercises/app.ts
Hello World
```

Deno doesn't do named arguments by default, but these can be passed in as flags using the Standard Library, which we'll examine later on in this course.

## Flags

Deno has built-in flags that can be passed to the runtime. You've already used many of them. We used "--no-check" to skip TypeScript checking and the "--allow-env" and "--allow-read" permissions flags.

Flags that are passed to Deno **must** be passed **before** the name of the file to execute. If they are passed after, they will be ignored. This is important as it can cause quite a bit of confusion if your flags are working the way you think they will.

There are several flags listed from the help, but let's take a look at a few of the ones you will use the most often.

### --watch & --unstable

One of my favorite features of Deno is the built-in watcher. It's kind of like supervisor or nodemon if you've used Node before. It just watches for file changes and then re-builds your TypeScript and restarts the process.

- Execute the `app.ts` file with the built-in watcher by passing the `--watch` flag.

```bash
deno run --watch app.ts World

error: The following required arguments were not provided:
--unstable

USAGE:
    deno run <SCRIPT_ARG>... --unstable --watch

For more information try --help
```

Uh oh - what happened? The CLI is telling us that we are missing a required flag to use the `--watch` feature. That is the `--unstable flag`. The `--unstable` flag allows Deno to run with still unstable features. Watch is one of those features. You'll encounter quite a few features in Deno (which is considered stable) that are behind the "unstable" flag at the time of this writing. That's important to note since you might assume that Deno being "stable" means all of it is stable. That's not the case.

- Run the program again with the `--unstable --watch` flags...

```bash
deno run --unstable --watch app.ts World

Check file:///home/burkeholland/dev/deno-first-look-exercises/app.ts
Hello World
Watcher Process terminated! Restarting on file change...
```

Modify the code in "app.ts" so that it transforms the output to uppercase

```bash
const message: String = Deno.args[0];
console.log(`Hello ${message}`.toUpperCase());
```

Save the file and notice that the terminal updates with the new output

```bash
Watcher File change detected! Restarting!
Check file:///home/burkeholland/dev/deno-first-look-exercises/app.ts
HELLO WORLD
Watcher Process terminated! Restarting on file change...
```

## Others

There are other flags that can be passed in that deal with different aspects that are unique to Deno. These we will take a closer look at when we talk about concepts like dependencies and permissions. For now, the important things to know about the Deno CLI are...

1. Pass -h after any option to see a description, sample usage and sub-options
1. Pass in all flags **before** the file name
1. Pass in your own arguments **after** the file name
