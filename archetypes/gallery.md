---
title: ""
# subtitle: ""
date: "{{ .Date }}"
image: "gallery/{{ .File.BaseFileName }}.png"
alt: "hugo-mods/{{ .File.BaseFileName }}"
# color: "#fff"
#hoverColor: "#fff"
github: 
    repo: "hugo-mods/{{ .File.BaseFileName }}"
    showInfo: true
    showButtons: false
buttons:
  - i18n: get
    icon: get
    url: "https://github.com/hugo-mods/{{ .File.BaseFileName }}"
  - i18n: example 
    icon: example
    url: "https://hugo-mods-{{ .File.BaseFileName }}.netlify.app/"
draft: false
---