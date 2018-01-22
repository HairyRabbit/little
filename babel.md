## babel ����

1. ES6��׼�﷨
2. ESMģ��
3. DIY�﷨���

### babel ��Ҫʹ�÷�Χ

1. ��NodeJs��ʹ��ESM��������ESM��дwebpack�����ļ���Jest����
2. browserƽ̨Ϊ�˼��ݾɵ�������õ��ĵ�Ƭ

### ESM ����

��˵�°汾��NodeJs�Ѿ�����ʹ��ESM����ʹ�����������鷳�ĺܡ���Ҫ���һ�������������ù��ܣ�����ֻ��������չ����`.mjs`��β���ļ���

```sh
node --experimental-modules index.mjs
```

�ƺ����ᱨ�ܶ��

�����и�����Է��������ESM���Ǿ���`@std/esm`��

```sh
yarn add @std/esm -D
node -r @std/esm index.mjs
```

����Ҫ���ǿ�����`.js`�ļ���������ESM����package.json��һ�����ã�

```json
// package.json
{
  "@std/esm": "cjs"
}

```

����Ȼ�Ѿ�����babel������Ҳ����ʹ��babel��require-hook��

```sh
yarn add @babel/register -D
node -r @babel/register
```

Ч��Ҳ��һ���ġ�:)

����ESM��������Ҫ�õ���һ��babel������Ǿ���`@babel/plugin-transform-modules-commonjs`�������˱������ǣ��������׼������һ��web app���õ���webpack��webpack����babel��modules����Ϊfalse������webpackҲ��һ��ESM resolver������˵webpackҪ�Լ�ȥ����������⣬rollupͬ������ôESM�����þ��൱����Ч�ˡ�

�����и�����������һ��ר�õ�ENV flag������`script`��babel��������û������`BABEL_ENV`�����û�У���ȥѰ��`NODE_ENV`��Ϊ��ǰ������������Ҫ��������һ��script��env��

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

### babel ʵ�ò��

#### @babel/plugin-proposal-object-rest-spread

��ã�û��֮һ�����ڱ���������﷨��

```js
{
   ...foo,
   bar: 42
}
```

������һ��ѡ����`useBuiltins`�������Ϊtrue������ֱ����Object.assign�����棺

```js
// output
Object.assign(foo, { bar: 42 })
```

�����˵������target֧��Object.assign�Ļ��������ѡ����Ϊtrue�ͱȽϺ��ʣ���Ȼ��֧�ֵĻ���������һ��helper������`_extends(foo, { bar: 42 })`


#### @babel/plugin-proposal-class-properties

���Ҳ�Ƚϳ���һЩ���������ڿ���react app��ʱ�����ڱ���������﷨��

```js
class Foo {
  bar = 42
  baz = () => {}
}
```

ͬ������һ��loose mode��

```js
// { loose: true }
var Foo = function Foo() {
  this.bar = 42
  this.baz = function baz() {}
}

// { loose: false }
var Foo = function Foo() {
  Object.defineProperty(this, "bar", {
    configurable: true,
    enumerable: true,
    writable: true,
    value: 42
  })
  Object.defineProperty(this, "baz", {
    configurable: true,
    enumerable: true,
    writable: true,
    value: function baz() {}
  })
}
```

> loose mode�̳���preset-env

#### @babel/plugin-syntax-dynamic-import

��webpack��˵���������ͺ���Ҫ�ˣ�������import()��webpack���汻�����и���롣

### ͨ�������ļ�

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
- [loose-mode](http://2ality.com/2015/12/babel6-loose-mode.html)
