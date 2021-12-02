---
title: "Hugo Community Modules"
date: "2021-11-22T23:53:44+01:00"
draft: false
description: "Hugo Community Mods"
---

Welcome! This page is all about user-friendly modules for [Hugo](https://gohugo.io/), a fast static site generator.
The [hugo-mods organization](https://github.com/hugo-mods/) is where all the repos are!

Below you can find all currently available modules which you can easily use to power up your site or theme.
Fun fact: The entire page makes use of the modules itself, showing you how they look fully integrated!

{{< term >}}
lines:
    - type: input
      data: hugo mod init github.com/username/your-repo
      wait: 500
    - data: "go: creating new go.mod: module github.com/username/your-repo"
    - type: input
      data: $EDITOR config.yaml
    - data: '[[module.imports]]'
    - data: 'path = "github.com/hugo-mods/icons"'
    - type: input
      data: hugo mod get -u
      wait: 500
    - data: "hugo: downloading modules …"
    #- type: progress
    #  data: 100
    #  wait: 200
    - data: "✓ hugo: collected modules in 3372 ms"
      wait: 0
{{</ term >}}

Made with {{< icon "heart" >}} by [kdevo](https://kdevo.github.io/).
