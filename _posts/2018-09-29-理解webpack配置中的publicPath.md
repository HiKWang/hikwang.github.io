---
layout: post
title: 理解webpack配置中的publicPath
subtitle: 
date: 2018-09-29 10:18:58
categories: 技术
postPatterns: circuitBoard
tags: 
---

# 草稿
> 当使用webpack命令进行打包上生产时，它确实是在静态资源路径前面加上publicPath的值。 但是当我们使用webpack-dev-server 进行开发时，它却不是在静态文件的路径上加publicPath的值，相反，它指的是webpack-dev-server 在进行打包时生成的静态文件所在的位置。

## 搭建项目结构

    |-package.json     
    |-webpack.config.js
    |-css
        |--style.css
    |-js
        |--index.js
    |-images
        |--small.jpg
        |--big.jpg
    |-dist
    |- index.html
    
1. `npm init`生成`package.json`文件
2. `npm install webpack webpack-cli webpack-dev-server css-loader style-loader url-loader file-loader --save-dev`安装依赖包
3. 配置`package.json`的`scripts`

```
# package.json

"script": {
    "build": "webpack",
    "dev": "webpack-dev-server"
},

```

4. 手动创建`webpack.config.js`

```javascript
const path = require('path');
const webpack = require('webpack');
//const htmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: path.join(__dirname, './js/index.js'),
    output: {
        path: path.join(__dirname, './dist'),
        publicPath: "/dist/",
        filename: "bundle.js"
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ["style-loader", "css-loader"]
            },
            {
                test: /\.(png|jpg)$/,
                loader: "url-loader",
                options: {
                    limit: 10000,
                    name: '[name].[hash:8].[ext]'
                }
            }
        ]
    },
    /*
    plugins: [
        new htmlWebpackPlugin()
    ]
    */
}
```

5. 手动创建`index.html`
