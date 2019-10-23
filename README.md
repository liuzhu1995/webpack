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
