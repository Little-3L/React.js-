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
