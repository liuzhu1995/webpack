### learning webpack

#### 直接通过Webpack命令进行打包  
webpack --entry=./index.js --output-filename=bundle.js --mode=development  
entry 资源打包的入口  
output-filename 输出的资源名  
mode 打包模式 Webpack 为开发者提供了三种模式  
development  production  none  

#### 使用 npm scripts
在 package.json 中添加脚本命令 通过 npm run build 执行打包 通常在一个项目中，源代码会放在 /src 目录下，输出资源放在 /dist 中  Webpack 的默认源代码入口就是 src/index.js 输出资源默认是 /dist
```
{
  "scripts": {
    "build": "webpack --output-filename=bundle.js --mode=development"
  }
}
```

#### 使用配置文件 webpack.config.js
如果在 package.js 中添加脚本命令，当项目需要越来越多的配置，就需要王命令中添加更多参数，后期难以维护，因此可以将这些参数改为对象的形式，专门放在一个配置文件中，Webpack每次打包读取该文件配置，Webpack 的默认文件配置是 webpack.config.js
```
modules.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.js"
  },
  mode: "development",
}
```

#### 使用 webpack-dev-server 插件
提供 HTTP 服务而不是使用本地文件预览  
监听文件的变化并自动刷新网页，做到实时预览    
webpack-dev-server 只是将打包结果放在内存中，并不会写入实际的文件中，每次收到请求时只是将内存中的结果返回给浏览器  
yarn add webpack-dev-server  
weboack.config.js
```
module.exports = {
  devServer: {
    publicPath: "/dist",
  }
}
```
package.js
```
{
  "scripts": {
    "dev": "webpack-dev-server"
  }
}
```

#### 配置资源入口(Webpack 通过 context、entry这两个配置项来共同决定入口文件的路径)
当使用 Webpack 对资源处理时，需要指定一个或多个入口，告诉 Webpack 具体从源码目录下哪个文件开始打包（如果把工程中的各个模块的依赖关系当作一棵树，入口就是这颗树依赖的树根） 
这些存在依赖关系的模块会在打包时被封装成一个chunk(被抽象和包装过后的一些模块)，根据配置不同，一个工程打包时可能产生一个或多个chunk。有这个chunk得到的打包产物我们称为bundle。  
在工程中可以定义一个或多个入口，每一个如旧都会产生一个结果资源，在一些特殊情况下，一个入口也可能产生多个chunk并生成多个bundle。  


context只能是字符串
context资源入口的路径前缀，在配置时必须使用绝对路径的形式，配置context的目的是让entry的编写更加简洁，尤其是多入口情况下。context可以省略，默认值为当前工程目录的根目录
```
//效果相同，入口都为<工程根路径>/src/scripts/index.js
module.exports = {
  context: path.join(__dirname, './src'),
  entry: './scripts/index.js',
};
module.exports = {
  context: path.join(__dirname, './src/scripts'),
  entry: './index.js',
}
```

entry 
enrty的配置可以有多种形式：字符串、数组、对象、函数。可根据不同场景选择

1. 字符串类型入口
```
module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js'
  }
};
```
2. 数组类型入口
```
module.exports = {
  entry: ['babel-polyfill', './src/index.js'],
};
等同于：
//webpack.config.js
module.exports = {
  entry: './src/index.js',
};

//index.js
import 'babel-polyfill';
```

3. 对象类型入口
如果想要定义多入口，必须使用对象的形式。对象的属性名 key 是 chunk name，属性值 value 是入口路径。
```
module.exports = {
  entry: {
    //chunk name 为 index，入口路径为 ./src/index.js
    index: './src/index.js',
     //chunk name 为 lib，入口路径为 ./src/lib.js
    lib: './src/lib.js,'
  },
};
```

对象的属性值也可以为字符串或者数组
```
module.exports = {
  entry: {
    index: ['babel-polyfill', './src/index.js'],
    lib: './src/lib.js',
  },
};
```

4. 函数类型入口（只需返回上面介绍的任何配置形式即可）
```
//返回一个字符串类型的入口
module.exports = {
  entry: () => './src/index.js',
};

//返回一个对象类型的入口
module.exports = {
  entry: () => ({
    index： './src/index.js',
    lib: ['babel-polyfill', './src/lib.js'],
  })，
}；

//传入一个函数的优点在于我们可以在函数体内添加一些动态的逻辑来获取工程的入口。另外，函数也支持返回一个 Promise 对象来进行异步操作
module.exports = {
  entry: () => new Promise((resolve, reject) =>  {
    //模拟异步
    setTimeout(() => {
      resolve('./src/index.js');
    }, 1000);
  }),
};
```



