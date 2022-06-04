# 基于vite打包vue项目

## 基本信息
1. /vit/
2. 背景：浏览器开始支持原生ESModule模块
3. 主要由两部分组成：
- `dev server`: 原生esmodule + HMR
- `bundle command`: 基于`Rollup`打包代码
4. 限制: 默认的构建目标浏览器是能在 script 标签上支持原生ESM和原生ESM动态导入

## 创建及打包vue3项目
```bash
 # 使用vue template
npm create vite@latest my-vue-app --template vue

# 打包vite项目，默认会在package.json同级目录下产生/dist文件夹
vite build
```
- 目录结构
```bash
web
|   .gitignore
|   index.html
|   package-lock.json
|   package.json
|   README.md
|   vite.config.js # 配置文件
+---.vscode
|       extensions.json
+---dist
|   |   favicon.ico
|   |   index.html
|   \---assets
|           index.3914429b.js
|           index.c8be967f.css
|           logo.03d6d6da.png      
+---node_modules              
+---public
|       favicon.ico
\---src
    |   App.vue
    |   main.js
    +---assets
    |       logo.png
    \---components
            HelloWorld.vue
```
- `package.json`
```javascript
{
    // ...
    "scripts": {
        "dev": "vite",
        "build": "vite build",
        "preview": "vite preview"
    },
    "dependencies": {
        "vue": "^3.2.25"
    },
    "devDependencies": {
        "@vitejs/plugin-vue": "^2.3.3",
        "vite": "^2.9.9"
    }
}
```
- `vite.config.js`
```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()]
})
```
- `main.js`
```javascript
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```