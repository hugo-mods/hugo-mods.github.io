---
title: "Introducing an icon module that scales"
date: "2021-11-28T10:46:04+01:00"
description: "hugo-mods/icons - Introducing an icon module that scales."
draft: false
---

{{< lazyimg img="**logo*" alt="logo of hugo-mods/icons" >}}

There was a time when [crafting an icon font](https://fontello.com/) was iconic. Now the time has come to replace them with [something better](https://css-tricks.com/icon-fonts-vs-svg/) &mdash; something that can be easily processed, embedded and customized. Icons as they were meant to be so to say. With Hugo and without any additional tooling.

## Let's use SVG icons!

When I developed my first Hugo theme more than four years ago, I was searching for a reliable option to embed SVG icons as an alternative to icon fonts, but only for the icons that the users of the theme actually needed.

I developed a partial that helped me with my task to inline SVG icons by reading-in the referenced SVG icon file, but the approach came with some disadvantages:

1. **Sharing without caring**: For quite a long time, Hugo partials (or shortcodes) were for shared in a "copy-paste & forget" blog-post fashion. I feel like every Hugo poweruser shared a shortcode once in a while which were useful, but readers needed to copy from a code block and therefore could not retrieve any upstream improvements. 
A mechanism to reliably modularize stuff for Hugo was needed! 
Luckily, this changed with [Hugo 0.56](https://github.com/gohugoio/hugo/releases/tag/v0.56.0) where Hugo modules were introduced, which made it possibly to ship parts that can be layered on top of your site via Hugo's virtual FS principles.
2. **Missing browser support**: Back then in 2017, SVG icon browser support (and their more advanced features) was not good. This forced me to fallback to an icon font approach where I needed to polyfill multiple older browsers. Things always become more messy when you have to provide all kinds of backward-compatibility. 
It's not only holding you back from some good features, but especially from a good performance, and I'm a big fan of [performant websites](https://github.com/kdevo/osprey-delight). 
It's 2021 now and a lot of things have changed in the browser landscape, be it positive or [negative](https://drewdevault.com/2020/03/18/Reckless-limitless-scope.html). One of the positive things is the great browser [support for SVG icons](https://caniuse.com/svg).
3. **Website size**: The relatively simple approach of inlining SVG icons makes it possible to only embed the icons that are wanted. However, embedding one and the same icon *n* times leads to an increase of *n* times the size of the SVG icon. The same holds true for information such as SVG's `<title>` or `<desc>` tags which help accessibility. 

Luckily, the last problem could be solved when I got to know the [SVG symbol approach](https://css-tricks.com/svg-symbol-good-choice-icons/). The approach makes it possible to define a set of SVG icons ID-tagged once and invisibly somewhere on the page and then reference them across the page by using SVG's `<use>` element. 
This [DRY](https://en.wikipedia.org/wiki/Don't_repeat_yourself) approach ensures that your page does not grow linearly per SVG icon repetition while still making individual styling possible ([among other neat advantages](https://css-tricks.com/svg-symbol-good-choice-icons/#why-symbol-is-better-for-icons)).

Hence, I only needed to wrap up my collection of icon-related partials to package a nice and new Hugo module!

## Introducing the `icons` mod

I developed the module with the goal to make it extra user-friendly and scalable, the latter just like SVGs themselves. Here is a summary of the most important features:

- High performance with wide browser support through SVG symbol usage
- Currently supports Font Awesome, but aims to be icon-family agnostic in future
- User-friendly partial API with support for [various usage patterns](https://github.com/hugo-mods/icons#usage), including custom icon name support
- Carefully implemented and structured [partials](https://github.com/hugo-mods/icons/blob/main/layouts/partials), fully utilizing Hugo's capabilities (read more below)
- Smart and comfortable icon search function
- Auto-enabled accessibility by extracting icon's metadata

[Go check it out!](https://github.com/hugo-mods/icons)
Set it up as a module on your Hugo site or theme within a few minutes, or continue reading for some development insights.

## Implementation
### A Composition of Partials

I implemented `icons` via multiple [partials](https://gohugo.io/templates/partials/) which form the building blocks of the module. If you have a programming background, you can compare each partial with a function that has predefined input parameters and returns an output value. Just like in [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming), I divided the partials into the documented *public* API (e.g. `icon` partial) and *private* partials (e.g. `icon-search` and `icon-resolve`).

{{< lazyimg img="**architecture*" alt="architecture of hugo-mods/icons" >}}

Furthermore, I opted for an architecture that makes it possible to search icons and retrieve metadata and to resolve icons independently from icon-family. While currently only Font Awesome is supported, the plan is to add more icon families in the future!

As you can see in above figure, the user does not necessarily need to know the full icon name or where it comes from. The module automatically resolves the first found “rocket” icon even if the name is not exact.
To stabilize for future updates, the module outputs a warning in such a case as an advice to better use the full icon name (or ID) when building the site:

{{< lazyimg img="**search*" alt="search function shown in terminal" >}}

This greatly helps with quick prototyping. It also supports non-obvious queries such as “NASA”, where it will output the same rocket icon — by making use of Font Awesome’s metadata in the background.

Finally, if you want to see the module in action from a usage perspective, check out my Hugo theme [Osprey Delight](https://github.com/kdevo/osprey-delight), which uses the “Data Pattern”. This usage pattern makes it possible to specify custom icon names e.g. in [data/icons.yaml](https://github.com/kdevo/osprey-delight/blob/dev/data/icons.yaml) for a reliable way of referencing icons.