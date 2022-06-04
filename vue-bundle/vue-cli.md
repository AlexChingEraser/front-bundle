# 基于vue-cli打包vue项目

vue-cli官网：https://cli.vuejs.org/zh/guide/

## @vue/cli
1. 定位： 项目脚手架
2. 依赖模块汇总
- `@vue/cli`: `vue`原型命令，如`vue create`、`vue serve`
- `@vue/cli-service-global`、`@vue/cli-service`: vue-cli服务，提供一系列命令如`serve`、`create`、`inspect`
- `@vue/cli-plugin-xxx`、`vue-cli-plugin-xxx`
3. 创建一个vue项目
```bash
npm install -g @vue/cli # 全局安装@vue/cli
vue create your-project-name

# 若要创建vue2项目，则采用以下命令：
npm install -g @vue/cli-init
vue init your-vue2-project-name
```
4. 打包vue项目
- `package.json`
```javascript
{
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build"
  }
}
```
- dev server
```bash
npm run serve # or `npx vue-cli-service serve`
```
- bundle
```bash
npm run build # or `npx vue-cli-service build`
```
## 【老版本】vue-cli
