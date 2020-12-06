---
path: "/getting-to-know/tooling"
title: "Tooling"
order: "3B"
section: "3 - Getting to know Deno"
description: "Burke covers how to install Deno, and takes a look at the Deno CLI, REPL and what options you need to be aware of."
---

> Switch to the "3-tooling" branch to follow along with this section

Tooling is available for Deno in the form of the Deno VS Code extension. The primary value of the extension is that it provides intellisense for Deno and a debugger.

![The Deno extension](../images/deno-extension.jpg)

The extension is installed from the VS Code Extension gallery either through VS Code, or via the [extension gallery web site](https://marketplace.visualstudio.com/items?itemName=denoland.vscode-deno).

By default, the extension won't appear to do anything. This is because it is disabled by default. You'll need to enable the Deno extension for your Deno code by adding the following line to your **Workspace** settings. Workspace is bolded there because that is what Deno recommends that you do, and at the time of this writing, enabling it in your user settings appears to have no affect.

In VS Code, open the Command Palette and select "Preferences: Open Workspace Settings (JSON)". When you do that, a new folder will be added to your project called ".vscode". Inside there is a "settings.json" file. These settings ONLY apply to the current project and are not global. The file should be empty. Add the following line to enable the Deno extension.

```json
"deno.enable": true
```

Once you change this setting, reload the editor by selecting "Reload Window" from the Command Palette in VS Code. I have been unable to get the extension to work without doing this after first install. There may also be occasions where the extension stops working. In this case, a "Reload Window" usually solves the problem.

To test that the extension is working, open the "app.ts" file in the "exercise" folder. Type the "Deno" global variable and press the ".". This should trigger VS Code's intellisense, which will show you all of the methods available off of the global "Deno" object.

![Deno intellisense](../images/deno-intellisense.jpg)

Great! You've got Deno installed and you've got the tooling that you need. It's time to write our first program using Deno.
