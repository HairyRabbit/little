## webpack 错误相关

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

构建错误，常常发生于loader transform的过程中。这个错误也通常是由两种情况引起的：

1. runloaders的时候，loader给出的错误。举个例子，比如语法出错时，babel就会抛出异常，而babel-loader同样也会抛出异常，这种错误就属于ModuleBuildError。应该是最普遍的情况之一。

2. loader返回的source不是一个string或buffer。webpack是要将loader链接起来的，如果返回的并不是一个string，那也就没办法链了，这也会抛出一个ModuleBuildError。这个错误描述也是固定的，那就是`new Error("Final loader didn't return a Buffer or String")`

抛出错误的文件在NormalModule.js。

解决方案？如果是第一种，那只能根据loader的类型来具体处理了，或许编译器本身也给了一些提示。比如代码`Promise.reject().catch(err => throw new Error('foo'))`，编译throw expr会用到babel的插件，如果没有配置的话，babel也会提示说是否忘记安装。对于这种错误或许就应该自动安装并配置一下么？

第二种也没什么好办法，不过话说回来，常用的loader并不会出现这个情况，所以这个错误通常就发生在小众的，或者是自己写的loader里，那解决的办法就是**自动打开编辑器并定位到return或是callback的地方来debug**

这里有个options可以关掉错误栈信息，那就是将`stats.errorDetails`设为`false`


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

简单理解就是没找到模块。从接口中可以看到missing，这是一个有用的属性，他标识了webpack从哪里寻找这个模块。比如一个模块foo，引入这个模块的文件是`path/to/src/index.js`，那missing可能就是：

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

大概可以看出上面的这些地址大概分为两部分，**node_modules的位置**和**文件扩展名**。所以有一些配置可以影响这里的结果。文件扩展名可以用`resolve.extensions`来限定：

```js
{
  resolve: {
    extensions: ['.js']
  }
}
```

因为默认是包含json文件的，这样做就可以关掉json的搜索，那之后引入文件的话，也得需要加json扩展名了呢。

node_modules的搜索规则是，搜索当前下面的，没找到就一层层的向上递归去找。所以文件层级特别深，而上级又恰好有node_modules的时候，那…哈哈哈。这里我们也可以配置一个绝对路径来告诉webpack只搜寻某个文件夹下的node_modules：

```js
{
  resolve: {
    modules: path.resolve('path/to/node_modules')
  }
}
```

上面两个配置之后的missing就只有：

```js
[
  'path/to/node_modules',
  'path/to/node_modules/foo',
  'path/to/node_modules/foo.js'
]
```

如果是说没找到文件，那通常也是会出现两种情况，第三方依赖包，比如react，jquery这些；自己写的文件但失误写错了路径。

对于第一种情况，第三方依赖确实，最直接的办法就是安装呗，可以试试`AutoInstallPlugin()`；后面一种手误的话就比较棘手了，可以根据正则来筛选项目下面文件名相似的文件。


### EntryModuleNotFoundError

这个错误是最简单的一个，意思是没有找到入口模块，大多数情况就发生在配置entry处。除了写错路径外，下面的做法，就会出现这个错误：

```js
{
  entry: 'index.js'
}
```

前面的ModuleNotFound也提到了，这会变成一个module而去node_modules里面找，如果恰好安装了一个叫做`index.js`的库，那……。改成：

```js
{
  entry: ./'index.js'
}
```

推荐的做法是将entry处理成一个绝对路径：

```js
{
  entry: path.resolve('index.js')
}
```


### Awesomes

- [ModuleBuildError](https://github.com/webpack/webpack/blob/master/lib/ModuleBuildError.js)
- [ModuleNotFoundError](https://github.com/webpack/webpack/blob/master/lib/ModuleNotFoundError.js)
