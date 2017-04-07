# wxapp-webpack-plugin
微信小程序 webpack 插件

## 为什么要使用 webpack

很多前端开发者都使用过 [webpack](https://webpack.js.org/)，通过 webpack 开发 JavaScript 项目可以带来很多好处

- 支持通过 `yarn` 或 `npm` 引入和使用 `node_modules` 模块
- 支持丰富且灵活的 `loaders` 和 `plugins`
- 支持 `alias`
- 还有很多...


## 用法

**注意：在 `v0.0.0` 发布之前，使用方法可能会有所改变**

#### 安装

```bash
yarn add -D wxapp-webpack-plugin
```

#### 配置 webpack

1. 在 `entry` 上引入 `{ app: './src/app.js' }`（支持[数组或对象方式](https://webpack.js.org/configuration/entry-context/#entry)）
2. 在 `output` 上设置 `filename: '[name].js`
3. 添加 `new WXAppWebpackPlugin()` 到 `plugins`
4. 添加 `file-loader` 到 `module.rules`

###### 完整 webpack.config.js 示例

```js
const path = require('path');
const WXAppWebpackPlugin = require('wxapp-webpack-plugin');

module.exports = {

    // 引入 `app.js`
    entry: {
        app: './src/app.js',
    },

    output: {
        filename: '[name].js',

        // 此处 `dist` 为微信开发者工具引入的开发目录
        path: path.resolve(__dirname, 'dist'),
    },
    plugins: [

        // 引入插件
        new WXAppWebpackPlugin(),

    ],
    module: {
        rules: [ // 各种 loaders 在这里添加
            {
                test: /\.(wxss|wxml|json)$/,
                include: /src/,
                loader: 'file-loader',
                options: {
                    useRelativePath: true,
                    name: '[name].[ext]',
                }
            },
        ],
    },
    devtool: 'source-map',
    resolve: {
        modules: ['src', 'node_modules'],
    },
};
```

#### 开始开发小程序

现在可以通过在终端输入 `webpack -w` 开始使用 webpack 开发微信小程序


## 注意

- 暂时只在 `webpack@v2.3.2` 测试通过，不确定其他版本下是否兼容性，欢迎提交反馈
- 程序的开发方式与 [微信小程序开发文档](https://mp.weixin.qq.com/debug/wxadoc/dev/) 一样，开发者需要在 `src` （源）目录创建 `app.js`、`app.json`、`app.wxss`、`pages/index/index.js` 之类的文件进行开发


## License

MIT © Cap32
