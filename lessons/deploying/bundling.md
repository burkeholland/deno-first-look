---
path: "/oak/deploying"
title: "Bundling Deno Apps"
order: "8B"
section: "Deploying"
description: "Burke looks at how to deploy an application built with Deno"
---

When you run a Deno app in production, you can run it the same way that we've been running it here all along. There is no difference there. The only additional item you would want is some way to restart the service if and when it crashes. But why would it crash?

## Why Deno might crash

In principal, Deno should just crash if it encounters and unhandled error. That's what most every other programming language does when such a thing happens. In the case of Oak, errors are handled for us an exposed via the `error` method which we wired up in the last section. However, there are scenarios in which Deno can still crash.
