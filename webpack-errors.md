## webpack �������

- ModuleBuildError
- ModuleNotFoundError
- EntryModuleNotFoundError

### ModuleBuildError

```js
interface ModuleBuildError {
  name: 'ModuleBuildError'
  message: string
  details: string
  module: Module
  error: Error
}
```

�������󣬳���������loader transform�Ĺ����С��������Ҳͨ�����������������ģ�

1. runloaders��ʱ��loader�����Ĵ��󡣾ٸ����ӣ������﷨����ʱ��babel�ͻ��׳��쳣����babel-loaderͬ��Ҳ���׳��쳣�����ִ��������ModuleBuildError��Ӧ�������ձ�����֮һ��

2. loader���ص�source����һ��string��buffer��webpack��Ҫ��loader���������ģ�������صĲ�����һ��string����Ҳ��û�취���ˣ���Ҳ���׳�һ��ModuleBuildError�������������Ҳ�ǹ̶��ģ��Ǿ���`new Error("Final loader didn't return a Buffer or String")`

�׳�������ļ���NormalModule.js��

�������������ǵ�һ�֣���ֻ�ܸ���loader�����������崦���ˣ��������������Ҳ����һЩ��ʾ���������`Promise.reject().catch(err => throw new Error('foo'))`������throw expr���õ�babel�Ĳ�������û�����õĻ���babelҲ����ʾ˵�Ƿ����ǰ�װ���������ִ�������Ӧ���Զ���װ������һ��ô��

�ڶ���Ҳûʲô�ð취��������˵���������õ�loader����������������������������ͨ���ͷ�����С�ڵģ��������Լ�д��loader��ǽ���İ취����**�Զ��򿪱༭������λ��return����callback�ĵط���debug**

�����и�options���Թص�����ջ��Ϣ���Ǿ��ǽ�`stats.errorDetails`��Ϊ`false`


### ModuleNotFoundError

```js
interface ModuleNotFoundError {
  name: 'ModuleNotFoundError'
  message: string
  details: string
  missing: string
  module: Module
  origin: Module
  dependencies: Dependencies
  error: Error
}
```

��������û�ҵ�ģ�顣�ӽӿ��п��Կ���missing������һ�����õ����ԣ�����ʶ��webpack������Ѱ�����ģ�顣����һ��ģ��foo���������ģ����ļ���`path/to/src/index.js`����missing���ܾ��ǣ�

```js
[
  'path/to/src/node_modules',
  'path/to/node_modules',
  'path/node_modules',
  'path/to/node_modules/foo',
  'path/to/node_modules/foo.js',
  'path/to/node_modules/foo.json'
]
```

��ſ��Կ����������Щ��ַ��ŷ�Ϊ�����֣�**node_modules��λ��**��**�ļ���չ��**��������һЩ���ÿ���Ӱ������Ľ�����ļ���չ��������`resolve.extensions`���޶���

```js
{
  resolve: {
    extensions: ['.js']
  }
}
```

��ΪĬ���ǰ���json�ļ��ģ��������Ϳ��Թص�json����������֮�������ļ��Ļ���Ҳ����Ҫ��json��չ�����ء�

node_modules�����������ǣ�������ǰ����ģ�û�ҵ���һ�������ϵݹ�ȥ�ҡ������ļ��㼶�ر�����ϼ���ǡ����node_modules��ʱ���ǡ�����������������Ҳ��������һ������·��������webpackֻ��Ѱĳ���ļ����µ�node_modules��

```js
{
  resolve: {
    modules: path.resolve('path/to/node_modules')
  }
}
```

������������֮���missing��ֻ�У�

```js
[
  'path/to/node_modules',
  'path/to/node_modules/foo',
  'path/to/node_modules/foo.js'
]
```

�����˵û�ҵ��ļ�����ͨ��Ҳ�ǻ�������������������������������react��jquery��Щ���Լ�д���ļ���ʧ��д����·����

���ڵ�һ�����������������ȷʵ����ֱ�ӵİ취���ǰ�װ�£���������`AutoInstallPlugin()`������һ������Ļ��ͱȽϼ����ˣ����Ը���������ɸѡ��Ŀ�����ļ������Ƶ��ļ���


### EntryModuleNotFoundError

�����������򵥵�һ������˼��û���ҵ����ģ�飬���������ͷ���������entry��������д��·���⣬������������ͻ�����������

```js
{
  entry: 'index.js'
}
```

ǰ���ModuleNotFoundҲ�ᵽ�ˣ������һ��module��ȥnode_modules�����ң����ǡ�ð�װ��һ������`index.js`�Ŀ⣬�ǡ������ĳɣ�

```js
{
  entry: ./'index.js'
}
```

�Ƽ��������ǽ�entry�����һ������·����

```js
{
  entry: path.resolve('index.js')
}
```


### Awesomes

- [ModuleBuildError](https://github.com/webpack/webpack/blob/master/lib/ModuleBuildError.js)
- [ModuleNotFoundError](https://github.com/webpack/webpack/blob/master/lib/ModuleNotFoundError.js)
