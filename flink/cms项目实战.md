1.项目集成prettier

```
1.通过vscode的prettier插件
2.通过prettier依赖
```

创建vue项目命令

```

npm init vue@latest

选择
typescript
先不安装vue-router,pinia 手动安装

vue3.5.18可以选择生成空白的项目


生成的项目目录待整理

1.vite.config.ts
项目的vite配置文件,主要配置的vue的解析插件与文件的路径跳转
2. eslint.config.ts
3. .prettierrc.json
4. tsconfig.json
```

2.安装vue-router

```
npm install vue-router

配置路由页面并集成到main.ts中
```

3.安装pinia

```
npm install pinia
集成pinia到main.ts中
```

4.安装axios

```
npm install axios
封装好axios工具
```

5.配置mode区分开发环境与生成环境

```

```

