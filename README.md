## webpack4 之后的版本是将 webpack webpack-cli 分离开来，必须同时安装才可以使用

- mkdir my-project
- npm init -y (-y 表示所有的选项默认为 yes)
- npm install webpack webpack-cli --save-dev
- ./node_modules/.bin/webpack -v 打印使用 webpack 的版本

## entry 的用法
- 单入口 entry 是一个字符串
```javascript
module.exports = {
  entry: 'index.js'
}
```
- 多入口 entry 是一个对象
```javascript
module.exports = {
  entry: {
    app: './src/app.js',
    adminApp: './src/adminApp.js'
  }
}
```
## 核心理念 output 的用法
- output 用来告诉 webpack 如何将编译后的文件输出到磁盘 
- output 的用法：单入口配置
```javascript
module.exports = {
  entry: 'index.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + 'dist' 
  }
}
```
- output 的用法：多入口配置
```javascript
module.exports = {
  entry: {
    app: './src/app.js',
    adminApp: './src/adminApp.js'
  },
  output: {
    filename: '[name].js', // 通过占位符 [name] 确保文件名称的唯一
    path: __dirname + '/dist'
  }
}
```