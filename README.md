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
## 资源解析：解析图片
- `file-loader` 用于处理文件
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
        test: /\.(png|jpe?g|gif)$/i,
        use: 'file-loader'
      }
    ]
  }
}
```
## CSS 文件的压缩
- 使用 optimize-css-assets-webpack-plugin
- 同时使用 cssnano
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
    }),
    new OptimizeCssAssetsWebpackPlugin({
      assetNameRegExp: /\.css$/g,
      cssProcesor: require('cssnano')
    })
  ]
}
```
## html 文件的压缩
- 修改 html-webpack-plugin,设置压缩参数
```javascript
new htmlWebpackPlugin({
  tmeplate: path.join(__dirname, 'src/index.html'),
  filename: 'index.html',
  chunks: ['index'],
  inject: true,
  minify: {
    html5: true,
    minifyCss: true,
    minifyJs: true,
    collapseWhitespace: true,
    removeComments: true
  }
})
```
## 当前构建时的问题
- 每次构建时候会清理目录，造成构建的输出目录 output 文件越来越多
- 通过 npm scripts 清理构建目录
  rm -rf ./dist && webpack
  rimraf ./dist && webpack
## 自动清理构建目录
- 避免构建前每次都需要手动清理 dist
- 使用 clean-webpack-plugin
  默认会删除 output 指定的输出目录
```javascript
module.exports = {
  entry: 'index.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + 'dist' 
  },
  plugins: [
    new CleanWebpackPlugin()
  ]
}
```
## CSS3 的属性为什么需要前缀
- IE Trident(-ms)
- 火狐 Geko(-moz)
- 谷歌Chrome Webkit(-webkit)
- 欧朋 Presto(-o)
## PostCss 插件 autoprefixer 自动补齐 CSS3前缀
- 使用 autoprefixer
- 根据  规则 [Can I Use](https://caniuse.com)
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
        use: ['style-loader', 'css-loader', 'less-loader',
        {
          loader: 'postcss-loader',
          plugins: () => [
            require('autoprefixer')({
              browsers: ['last 2 version', '>1%', 'ios 7']
            })
          ]
        }]
      }
    ]
  }
}
```
## 浏览器的分辨率
- CSS 媒体查询实现响应式布局（缺陷：需要写多套适配样式代码）
- rem 是什么 ？
    W3C 对 rem 定义：font-size of the root element
- rem 和 px 对比
  rem 是相对单位
  px 是绝对单位
## 移动端 CSS px 自动转换成 rem
- 使用 px2rem-loader
- 页面渲染时，计算根元素的 font-size 值
- 可以使用手淘的 [lib-flexible](https://github.com/amfe/lib-flexible) 库
```javascript
module.exports = {
  entry: 'index.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + 'dist' 
  },
  modul e: {
    rules: [
      {
        test: /\.less$/, // 使用 less-loader 的时候 必须安装 less less-loader
        use: ['style-loader', 'css-loader', 'less-loader',
        {
          loader: 'px2rem-loader',
          options: {
            remUnit: 75, // rem 相对于 px 的转换单位，这里代表 1rem 为 75px
            remPrecision: 8 // 代表转换成 rem 后小数点的位数
          }
        }
        ]
      }
    ]
  }
}
```

## 静态资源内联（静态资源内联的意义）

代码层面
  - 页面框架的初始化脚本
  - 上报相关打点
  - css 内联避免页面闪动
  
  请求层面：减少 HTTP 网络请求数
  - 小图片或者字体内联（url-loader）
  
## HTML 和 JS 内联

- `raw-loader` 内联 html
```javascript
<script>${require('raw-loader!babel-loader!./meta.html')}</script>
```
- `raw-loader` 内联 JS
```javascript
<script>${require('raw-loader!babel-loader!../nodule_modules/lib-flexible/flexible.js')}</script>
```

## CSS 内联

- 方案一：借助 `style-loader`
- 方案二：`html-inline-css-webpack-plugin`
```javascript
// 以下代码 style-loader webpack4中 options属性已废弃
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
        use: [{
          loader: 'style-loader',
          options: {
            insertAt: 'top', // 样式插入到 <head> webpack 4 此属性已废弃
            singleton: true // 将所有的 style 标签合并成一个 webpack 4 此属性已废弃
          }
        }, 'css-loader']
      }
    ]
  }
}
```
以下是正确的用法
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
        use: [{
          loader: 'style-loader',
          options: {
              injectType: 'singletonStyleTag'
          },
        }, 'css-loader']
      }
    ]
  }
}
```

## 多页面应用（MPA）概念

- 每次页面跳转的时候，后台服务器都会返回一个新的 html 文档，这种类型的网站也就是多页面网站，也叫做多页应用

## 多页面打包基本路由

- 每个页面对应一个 `entry`, 一个 `html-webpack-plugin`
- 缺点：每次新增或删除页面需要改 webpack 配置
```javascript
module.exports = {
  entry: {
    index: './src/index.js',
    search: './src/search.js'
  }
}
```

## 多页面打包通用方案

- 动态获取 `entry` 和设置 `html-webpack-plugin` 数量
- 利用 glob.sync (需要安装 glob 包)
- `entry: glob.sync(path.join(__dirname, './src/*/index.js'))`
- 比如有两个页面分别是 index.html 和 search.html 都需要打包到 bundle.js 中，分别创建 index 和 search 目录
- 分别把文件 index.html search.html 放置 index search 目录中，命名 index.html index.js 各个名称都叫 index

 1. index/
    index.html
    index.js
    css/
    image/

 2. search/
    index.html
    index.js
    css/
    image/

```javascript
const path = require('path')
const setMPA = () => {
  const entry = {}
  const htmlWebpackPlugin = []
  const entryFiles = glob.sync(path.join(__dirname, 'src/*/index.js'))
  // console.log(entryFiles)
  Object.keys(entryFiles)
    .map((index) => {
      const entryFile = entryFiles[index];
      const match = entryFile.match(/src\/(.*)\/index\.js/);
      const pageName = match && match[1]
      console.log('pageName', pageName)
      entry[pageName] = entryFile;
      htmlWebpackPlugin.push(
        new HtmlWebpackPlugin({
          template: path.join(__dirname, `src/${pageName}/index.html`),
          filename: `${pageName}.html`,
          chunks: [pageName],
          minify: {
            html5: true,
            collapseWhitespace: true,
            removeComments: false
          }
        })
      )
    })
  return {
    entry,
    htmlWebpackPlugin
  }
}
const { entry, htmlWebpackPlugin } = setMPA();
module.exports = {
  mode: 'production',
  entry: entry,
  output: {
    filename: '[name]_[chunkhash:8].js',
    path: path.join(__dirname, 'dist')
  },
  plugins: [
    new OptimizeCssAssetsWebpackPlugin({
      assetNameRegExp: /\.css$/g,
      cssProcessor: require('cssnano'),
    })
  ].concat(htmlWebpackPlugin)
}
```