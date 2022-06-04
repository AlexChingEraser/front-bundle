# 基于Webpack打包vue项目

## vue3 + webpack5
参考：https://cloud.tencent.com/developer/article/1783122
1. project directory tree
```bash
web
|   bebel.config.js
|   index.html
|   package-lock.json
|   package.json
|   webpack.config.js
+---dist
|   |   index.html
|   \---js
|       main.js         
+---node_modules          
\---src
        App.vue
        main.js   
```
2. `package.json`
```javascript
//....
"scripts": {
    "build": "webpack --config ./webpack.config.js",
    "dev": "webpack serve --progress --config ./webpack.config.js"
},
"dependencies": {
    "vue": "^3.2.36"
},
"devDependencies": {
    "@babel/core": "^7.18.2",
    "@babel/preset-env": "^7.18.2",
    "@vue/compiler-sfc": "^3.2.36",
    "babel-loader": "^8.2.5",
    "clean-webpack-plugin": "^4.0.0",
    "css-loader": "^6.7.1",
    "html-webpack-plugin": "^5.5.0",
    "style-loader": "^3.3.1",
    "vue-loader": "^17.0.0",
    "webpack": "^5.73.0",
    "webpack-cli": "^4.9.2",
    "webpack-dev-server": "^4.9.1"
}
```

3. `npm install`
```bash
#如果`npm install vue`，也会默认安装vue3版本
npm install @vue/next

npm install --save-dev @vue/compiler-sfc vue-loader
npm install --save-dev webpack webpack-cli webpack-dev-server
npm install --save-dev clean-webpack-plugin html-webpack-plugin
npm install --save-dev style-loader css-loader
npm install --save-dev @babel/core @babel/preset-env babel-loader
```

4. `webpack.config.js`
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const { VueLoaderPlugin } = require('vue-loader/dist/index');

module.exports = {
    mode: 'development',
    entry: path.resolve(__dirname, './src/main.js'),
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'js/[name].js'
    },
    module: {
        rules: [
            {
                test: /\.vue$/,
                use: ['vue-loader']
            },
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader'
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: path.resolve(__dirname, './index.html'),
            filename: 'index.html'
        }),
        new CleanWebpackPlugin(),
        new VueLoaderPlugin()
    ],
    devServer: {
        static: path.resolve(__dirname, './dist'),
        port: 21101, // feel free to set
        compress: true,
        devMiddleware: {
            publicPath: '/',
        }
    }
}
```
5. `babel.config.js`
```javascript
module.exports = {
    presets: [
        ["@babel/preset-env", {
            "targets": {
                "browsers": ["last 2 versions"]
            }
        }]
    ]
}
```
6. `index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web Name</title>
</head>
<body>
    <div id="root"></div>
</body>
</html>
```
7. `main.js`
```javascript
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)
app.mount('#root')
```

## vue2 + webpack4
参考：https://www.jianshu.com/p/6ff34032a76f
创建vue2项目需要显式指定相关依赖版本，且需要注意安装顺序
1. project directory tree
```bash
web
|   babel.config.js
|   index.html
|   package-lock.json
|   package.json
|   webpack.config.js
+---dist
|       bundle.js
|       index.html
+---node_modules     
\---src
        App.vue
        index.js
```
2. `package.json`
```javascript
// ...
"scripts": {
    "build": "webpack --config webpack.config.js --mode=development"
},
"dependencies": {
    "vue": "^2.6.14" // 注意是@2
},
"devDependencies": {
    "@babel/core": "^7.18.2",
    "@babel/preset-env": "^7.18.2",
    "babel-loader": "^8.2.5",
    "clean-webpack-plugin": "^4.0.0", // 注意是@4
    "css-loader": "^6.7.1",
    "html-webpack-plugin": "^4.5.2", // 注意是@4
    "vue-loader": "^15.9.8", // 注意是@15
    "vue-template-compiler": "^2.6.14", // vue3后就去掉了，转为@vue/comiler-sfc
    "webpack": "^4.46.0", // 注意是@4
    "webpack-cli": "^4.9.2" // 注意是@4
}
```
3. `npm install`
```bash
# 尽量不要变动顺序
npm install vue@2 # 当前vue2最高版本2.6.14
npm install --save-dev vue-loader@15 vue-template-compiler
npm install --save-dev webpack@4 webpack-cli
npm install --save-dev css-loader
npm install --save-dev html-webpack-plugin@4 clean-webpack-plugin
npm install --save-dev @babel/core @babel/preset-env babel-loader
```
4. `webpack.config.js`
```javascript
// webpack.config.js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
const VueLoaderPlugin = require('vue-loader/lib/plugin');

module.exports = {
    entry:  path.join(__dirname, 'src/index.js'),
    mode:'develop',
    output: {
        filename: 'bundle.js',
        path: path.join(__dirname, 'dist')
    },
    module: {
        rules: [
            {
                test: /.vue$/,
                loader: 'vue-loader'
            },
            {
                test: /\.css$/,
                loader: ['css-loader']
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader'
            }
        ]
    },
    plugins:[
        new HtmlWebpackPlugin({
            publicPath: './',
            template: path.resolve(__dirname, './index.html'),
            filename: 'index.html'
        }),
        new VueLoaderPlugin(),
        new CleanWebpackPlugin()
    ],
}
```
5. `babel.config.js`
```javascript
module.exports = {
    presets: [
        ["@babel/preset-env", {
            "targets": {
                "browsers": ["last 2 versions"]
            }
        }]
    ]
}
```
5. `index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue2 + Webpack4</title>
</head>
<body>
    <div id="root"></div>
</body>
</html>
```
6. `index.js`
```javascript
import Vue from 'vue'
import App from './App.vue'

new Vue({
    el: '#root',
    render: (h) => h(App)
})
```