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

����ESM��˵��������Ҫ�õ�һ��babel������Ǿ���`@babel/plugin-transform-modules-commonjs`�������˱������ǣ��������׼������һ��web app���õ���webpack��webpack����babel��modules����Ϊfalse������webpackҲ��һ��ESM resolver������˵webpackҪ�Լ�ȥ����������⣩����ôESM�����þ��൱����Ч��

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
