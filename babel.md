## babel 配置

1. ES6标准语法
2. ESM模块
3. DIY语法插件

### babel 主要使用范围

1. 在NodeJs中使用ESM，包括用ESM来写webpack配置文件，Jest测试
2. browser平台为了兼容旧的浏览器用到的垫片

### ESM 配置

虽说新版本的NodeJs已经可以使用ESM，但使用起来还是麻烦的很。需要添加一条参数来启动该功能，并且只能用于扩展名是`.mjs`结尾的文件：

```sh
node --experimental-modules index.mjs
```

似乎还会报很多错。

不过有个库可以方便的配置ESM，那就是`@std/esm`：

```sh
yarn add @std/esm -D
node -r @std/esm index.mjs
```

更重要的是可以让`.js`文件都适配于ESM，在package.json加一条配置：

```json
// package.json
{
  "@std/esm": "cjs"
}

```

但既然已经用了babel，我们也可以使用babel的require-hook：

```sh
yarn add @babel/register -D
node -r @babel/register
```

效果也是一样的。:)

对于ESM来说，这里主要用到一个babel插件，那就是`@babel/plugin-transform-modules-commonjs`。但令人崩溃的是，如果我们准备开发一个web app而用到了webpack，webpack需求babel把modules设置为false（好巧webpack也是一个ESM resolver，就是说webpack要自己去解决依赖问题），那么ESM的配置就相当于无效。

这里有个做法是设置一个专用的ENV flag，比如`script`。babel会先找有没有设置`BABEL_ENV`，如果没有，就去寻找`NODE_ENV`作为当前环境。所以需要重新配置一个script的env：

```json
// .babelrc
{
  "env": {
    "script": {
      "presets": [
        ["@babel/preset-env", {
          "targets": {
            "node": true
          },
          "loose": true
        }]
      ]
    }
  }
}
```


### 通用配置文件

```js
{
  "presets": [
    "@babel/preset-react",
    "@babel/preset-flow",
    ["@babel/preset-env", {
      "targets": {
        "browsers": ["last 1 versions"]
      },
      "modules": false,
      "loose": true,
      "debug": true,
      "useBuiltIns": "usage"
    }]],
  "plugins": [
    "@babel/plugin-proposal-class-properties",
    "@babel/plugin-proposal-export-default-from",
    "@babel/plugin-proposal-export-namespace-from",
    ["@babel/plugin-proposal-object-rest-spread", {
      "useBuiltIns": true
    }],
    "@babel/plugin-syntax-dynamic-import",
    "@babel/plugin-proposal-throw-expressions"
  ],
  "env": {
    "script": {
      "presets": [
        ["@babel/preset-env", {
          "targets": {
            "node": true
          },
          "loose": true
        }]
      ]
    },
    "test": {
      "plugins": ["@babel/plugin-transform-modules-commonjs"]
    }
  }
}
```


### Awesomes

- [@std/esm](https://github.com/standard-things/esm)
