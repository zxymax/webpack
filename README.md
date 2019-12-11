## webpack4 之后的版本是将 webpack webpack-cli 分离开来，必须同时安装才可以使用

- `mkdir my-project`
- `npm init -y` (-y 表示所有的选项默认为 yes)
- `npm install webpack webpack-cli --save-dev`
- `./node_modules/.bin/webpack -v` 打印使用 `webpack` 的版本

## entry 的用法
- 单入口 `entry` 是一个字符串
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
- `output` 用来告诉 webpack 如何将编译后的文件输出到磁盘 
- `output` 的用法：单入口配置
```javascript
module.exports = {
  entry: 'index.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + '/dist' 
  }
}
```
- `output` 的用法：多入口配置
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
## 核心理念之 loaders 的用法
  `webpack` 开箱即用只支持 `JS` 和 `JSON` 两种类型，通过 Loaders 去支持其它文件类型并且把它们转化成有效的模块，并且可以添加到依赖图中。
  Loaders 本身是一个函数，接受源文件作为参数，返回转换的结果。
|  名称   | 描述  |
|  -  | - |
| babel-loader  | 转换 ES6、ES7 等 JS 新特性语法 |
| css-loader  | 支持 .css 文件的加载和解析 |
| less-loader | 将 less 文件转换成 css |
| ts-loader | 将 TS 文件转换成 JS |
| file-loader | 进行图片、字体等的打包 |
| raw-loader | 将文件以字符串的形式导入 |
| thread-loader | 多进程打包 JS 和 CSS |

- Loaders 的用法
```javascript
module.exports = {
  entry: 'index.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + 'dist' 
  },
  module: {
    rules: [
      {
        test: /\.txt$/, // test 指定匹配规则
        use: 'raw-loader' // use 指定使用 loader 的名称
      }
    ]
  }
}
```
## 核心理念之 Plugins
- 插件用于 bundle 文件的优化，资源管理和环境变量注入
- 作用于整个构建过程
  
常见的 Plugins 有哪些
| 名称 | 描述 |
|-|-|
| CommonsChunkPlugin | 将 chunks 相同的模块代码提取成公共 JS |
| CleanWebpackPlugin | 清理构建目录 |
| ExtractTextWebpackPlugin | 将 CSS 从 bundle 文件里提取成一个独立的 CSS 文件 |
| CopyWebpackPlugin | 将文件或文件夹拷贝到构建的输出目录 |
| HtmlWebpackPlugin | 创建 html 文件去承载输出的 bunndle |
| UglifyjsWebpackPlugin | 压缩 JS |
| ZipWebpackPlugin | 将打包的资源生成一个 zip 包 |

- Plugins 的用法
```javascript
module.exports = {
  entry: 'index.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + 'dist' 
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ]
}
```
## 核心理念之 Mode
- Mode 用来指定当前的构建环境是：`production`、`development` 还是 `none`
- 设置 mode 可以使用 `webpack` 内置的函数，默认值为 `production`
  
Mode 的内置函数和功能
| 选项 | 描述 |
| - | - |
| `development` | 设置 `process.env.NODE_ENV` 的值为 `development`，开启 `NamedChunksPlugin` 和 `NamedModulesPlugin` |
| `production` | 设置 `process.env.NODE_ENV` 的值为 `production`，开启 `FlagDependencyPlugin`，`FlagIncludedChunksPlugin`，`ModuleConcatenationPlugin`，`NoEmitOnErrorsPlugin`，`OccurrenceOrderPlugin`，`SideEffectFlagPlugin` 和 `TerserPlugin` |
| `none` | 不开启任何优化选项 |
## 资源解析：解析 ES6
- 使用 `babel-loader`
- babel 的配置文件是 `.babelrc`
```javascript
{
  "presets": ["@babel/preset-env"],

}
```
```javascript
module.exports = {
  entry: 'index.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + 'dist' 
  },
  module: {
    rules: [
      {
        test: /\.js$/, 
        use: 'babel-loader'
      }
    ]
  }
}
```
## 资源解析：解析 CSS
- `css-loader` 用于加载 .css 文件，并且转换成 commonjs 对象
- `style-loader` 将样式通过 `style` 标签插入到 head 中
```javascript
module.exports = {
  entry: 'index.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + 'dist' 
  },
  module: {
    rules: [
      {
        test: /\.css$/, 
        use: ['style-loader', 'css-loader']
      }
    ]
  }
}
```
## 资源解析：解析 Less 和 SaSS
- `less-loader` 用于将 less 转换成 css
```javascript
module.exports = {
  entry: 'index.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + 'dist' 
  },
  module: {
    rules: [
      {
        test: /\.less$/, // 使用 less-loader 的时候 必须安装 less less-loader
        use: ['style-loader', 'css-loader', 'less-loader']
      }
    ]
  }
}
```