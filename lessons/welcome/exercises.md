---
path: "/exercises"
title: "How to use the exercises"
order: "1B"
section: "1 - Welcome"
description: "Burke covers the agenda for the Deno First Look workshop, talks about himself a little bit more than you would probably like, explains how to submit issues with this course and then pontificates on whether or not Deno is something that has a future and if we should be investing valuable time into learning it."
---

The steps that I'll go through in this workshop are also available as a repo with exercises where you can follow along.

The repo is called Deno Exercises. All you need to do to use it is to clone it.

## Using the exercise project

The Deno Exercise project has branches for all of the different sections in this workshop. For instance, if the section we're on is "Oak Templates" in chapter 7, the branch you need to be on to follow along is "7-oak-templates". This convention is used for every section, although not every section has an exercise.

Some sections build on each other. This becomes especially true when we get to the Oak section. If you get lost at any point, these sections contain a "solutions" folder that should have the working code.

## The Deno dev container

Deno Exercises also contain a ".devcontainer" definition. What this does it preconfigures an environment where Deno is already installed and fully functional. You won't need to install anything, it will "just work".

In order to use this Dev Container, you need to have the Remote-Containers extension for VS Code, as well as Docker Desktop installed. Note that if you are on Windows, it's highly recommended that you also have WSL2 enabled as the backing subsystem for Docker.
