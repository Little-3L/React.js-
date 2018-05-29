# React.js项目开发（PC端与移动端）
此项目需先安装Node.js

## 开发环境配置（webpack4）
在这之前我使用了国内淘宝镜像源，因为外网太慢
```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
npm config set registry https://registry.npm.taobao.org  
```

1. 进入工作文件夹，进行初始化，生成package.json文件
```bash
cnpm init
```
2. 在项目中安装webpack和webpack-cli和webpack-dev-server
```bash
cnpm install webpack webpack-cli webpack-dev-server --save 
```
3. 在项目文件目录下新建index.html，新建文件夹src，在src中新建文件index.js
注：webpack4默认入口文件为src中的index.js，可以在webpack.config.js中修改

4. 安装react
```bash
cnpm install react react-dom  
```

5. 安装babel
```bash
cnpm install babel-loader@next @babel/core @babel/preset-react --save 
```
6. 项目目录下新建配置文件webpack.config.js:
```jsx
var path = require("path");

module.exports={
  module:{
    rules:[
      {
        test:/\.js$/,
        exclude:/(node_modules|bower_components)/,
        use:{
          loader:'babel-loader',
          options:{
            presets: ['@babel/preset-react'],
            plugins: ['react-html-attrs']
          }
        }
      },
    ]
  },
};
```

7. 使用webpack打包一下,发现在项目目录下生成了一个文件夹dist，文件夹里生成一个文件main.js,这是webpack4默认的打包输出文件，把它引入到index.html中:
```jsx
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>ReactNews</title>
  </head>
  <body>
    <div id="root"></div>
    <script src="./dist/main.js"></script>
  </body>
</html>
```
8. 实现热加载，cmd输入：
```bash
npx webpack-dev-server --mode development --output-public-path dist  
```
打开http://localhost:8080/ 即可。
注：8080为默认端口，可以在webpack.config.js中修改


## Ant Design框架的引入
根据官网的步骤安装引用：
http://design.alipay.com/develop/web/docs/introduce

## 项目页头页脚模块
