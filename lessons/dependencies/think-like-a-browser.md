---
path: "/dependencies/think-like-a-browser.md"
title: "Thinking like a browser"
order: "5A"
section: "Understanding Dependencies"
description: "Burke dives into how Deno handles dependencies."
---

You'll remember from the earlier introduction section that Deno thinks differently about dependencies. It's a core design premise. But we didn't talk about _how_ it's different.

Deno tries to stay as close to browser conventions as possible everywhere that it can. And how do you include a JavaScript file in a browser? You reference it with a script tag. If it's on a CDN, you might do it like this...

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.12/dist/vue.js"></script>
```

Deno works the same way. You require in dependencies by URL. If you are requiring a local dependency, it's just the relative path. If you are using a third-party library, you do it by the URL.

Deno follows the [ECMAScript standard](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) for `import/export` syntax.
