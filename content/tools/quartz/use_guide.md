---
title: "Quartz"
categories: ["Tools"]
tags: []
draft: true
---

# Introduction

This document will serve as a simple reminder on how to use Quartz and also as a helper on how to config some aspects that I think are worth understanding.

# Quartz?

Quartz is a static site generator that takes your markdown and transforms it into a fully functional website, perfect to takes notes without the hassle of having to set everything up.

Under the hood Quartz uses Node.js + Typescript and in build time is able to generate all the html code. This allows the code to be hosted in Github Pages without any backend!


# First Time

Ensure you have the necessary dependencies, **Node** and **npm**.
And the setup is pretty easy, just follow the *Get Started Guide:

```zsh
# 1. Clone the Quartz repository
git clone https://github.com/jackyzha0/quartz.git
cd quartz
 
# 2. Install dependencies
npm i
 
# 3. Initialize your site (choose a template, set your base URL, import content)
npx quartz create
 
# 4. Install plugins referenced by your chosen template
npx quartz plugin install --from-config
 
# 5. Preview your site locally at :8080
npx quartz build --serve
```

# How to use it

The basic idea is simple, write content inside `/content` in Markdown style. Extensions may add extra syntax functionalities, like the default support for Github Markdown and Obsidian style.

To publish the content to Github, just run `npx quartz sync`.


