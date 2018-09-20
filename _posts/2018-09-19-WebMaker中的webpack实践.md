---
layout: post
title: WebMaker中的webpack实践
subtitle: 
date: 2018-09-19 11:12:23
categories: 技术
postPatterns: circuitBoard
tags: 
---

- WebMaker目录结构
    
    - WebMaker
        - framework
            - src
                - core
                - components
                - assets
                - styles
                - layout
                - pages
                - utils
                - i18n
                - icons
                - App.vue
                - index.js
                - package.json

            - builder
            - bin

        - builder
        - project
        - api-mocks

- webpack核心概念
    - entry
    - output
    - loader
    - plugins

- 将webpack.config.js中的公共配置抽离出来形成base.config.js

- 在base.config.js中入口七点(entry pointer)不设置，在后续的webpack项目配置中才具体设值。
