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
```bash
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
网页图标网站：https://www.iconfinder.com/ ，可在此网站下载网页Logo。
在src下新建js、css、images文件夹，分别存放不同的文件类型，并在js下新建components文件夹与root.js文件（root.js为总组件）。
### PC端
1. 在components文件夹下新建pcheader.js文件。
页面布局采用flex布局，并使用antd里的Menu组建，导入antd:
```bash
import { Row, Col } from 'antd';
import { Menu, Icon} from 'antd';
const SubMenu = Menu.SubMenu;
const MenuItemGroup = Menu.ItemGroup;
```
然后在header标签中引用，写入Logo、导航栏与注册登陆。
```jsx
<header>
        <Row>
          <Col span={2}></Col>
          <Col span={4}>
              <a href="/" className="logo">
                <img src="./src/images/logo.png" alt="logo"/>
                <span>ReactNews</span>
              </a>
          </Col>
          <Col span={16}>
          </Col>
          <Col span={2}></Col>
        </Row>
</header>
```

Menu模块：
```jsx
<Menu mode="horizontal" onClick={this.handleClick.bind(this)} selectedKeys={[this.state.current]}>
     <Menu.Item key="top">
         <Icon type="appstore" />头条
     </Menu.Item>
     <Menu.Item key="shehui">
         <Icon type="appstore" />社会
     </Menu.Item>
     <Menu.Item key="guonei">
         <Icon type="appstore" />国内
     </Menu.Item>
     <Menu.Item key="guoji">
          <Icon type="appstore" />国际
     </Menu.Item>
     <Menu.Item key="yule">
          <Icon type="appstore" />娱乐
     </Menu.Item>
     <Menu.Item key="tiyu">
          <Icon type="appstore" />体育
     </Menu.Item>
     <Menu.Item key="jingji">
          <Icon type="appstore" />经济
     </Menu.Item>
</Menu>
``` 
2.  在components文件夹下新建pcfooter.js文件。
```jsx
import React from 'react';
import { Row, Col } from 'antd';

export default class PcFooter extends React.Component{
  render(){
    return(
      <footer>
        <Row>
          <Col span={2}></Col>
          <Col span={20} className="footer">
              &copy;&nbsp;2018 ReactNews. All Rights Reserved.
          </Col>
          <Col span={2}></Col>
        </Row>
      </footer>
    );
  }
}
```

### 移动端
安装插件react-responsive：
```bash
cnpm install react-responsive --save 
```
在root.js文件里导入：
```bash
import MediaQuery from 'react-responsive';
```
当设备宽度大于1224px时，显示PC端Header,否则，反之。
```bash
<div>
   <MediaQuery query="(min-device-width:1224px)">
       <PcHeader></PcHeader>
       <PcFooter></PcFooter>
   </MediaQuery>
   <MediaQuery query="(max-device-width:1224px)">
       <MobileHeader></MobileHeader>
       <MobileFooter></MobileFooter>
   </MediaQuery>
</div>
```
在components文件夹下新建mobileheader.js文件。
```bash
<div id="mobileheader">
   <header>
     <img src="./src/images/logo.png" alt="logo"/>
   <span>ReactNews</span>
   </header>
</div>
```
mobilefooter.js文件与pcfooter.js文件一样。
            
## 项目注册登录模块
实现点击注册登录跳出模态框，并用Tab进行注册与登陆的切换。

首先安装fetch框架，在js中进行API以及一些HTTP的Ajax请求:
```bash
cnpm install fetch --save 
```
### PC端
使用Modal模态框和Tab标签，导入相关ntd:
```bash
import {
  Menu,
  Icon ,
  Tabs,
  message,
  Form,
  Input,
  Button,
  CheckBox,
  Modal
} from 'antd';
const FormItem = Form.Item;
const TabPane = Tabs.TabPane;
```
弹出框注册内容，因为是隐藏内容，所以放哪都可以：
```jsx
<Modal title="用户中心" wrapClassName="vertical-center-modal" visible={this.state.modalVisible} onOk={()=>this.setModalVisible(false)} okText="关闭" onCancel={()=>this.setModalVisible(false)}>
            <Tabs type="card">
               <TabPane tab="注册" key="2">
                  <Form horizontal onSubmit={this.handleSubmit.bind(this)}>
                      <FormItem label="账户">
                          <Input placeholder="请输入您的帐号" {...getFieldProps('r_userName')}/>
                      </FormItem>
                      <FormItem label="密码">
                          <Input type="password" placeholder="请输入您的密码" {...getFieldProps('r_password')}/>
                      </FormItem>
                      <FormItem label="确认密码">
                          <Input type="password" placeholder="请再次输入您的密码" {...getFieldProps('r_confirmPassword')}/>
                      </FormItem>
                      <Button type="primary" htmlType="submit">注册</Button>
                   </Form>
               </TabPane>
            </Tabs>
</Modal>
```
定义userShow判断用户登录状态,并在Menu模块最后调用{userShow}：
```bash
const userShow =this.state.hasLogined
    ?
    <Menu.Item key="logout" className="register">
     <Button type="primary" htmlType="button">{this.state.userNickName}</Button>
     &nbsp;&nbsp;
     <Link target="_blank">
      <Button type="dashed" htmlType="button">个人中心</Button>
     </Link>
     &nbsp;&nbsp;
     <Button type="ghost" htmlType="button">退出</Button>
    </Menu.Item>
    :
    <Menu.Item key="register" className="register">
     <Icon type="appstore" />注册/登录
    </Menu.Item>;
```

控制模态框是否显示或隐藏
```bash
  setModalVisible(value){
    this.setState({modalVisible: value});
  };
```
点击事件
```bash
handleClick(e){
    if(e.key = "register"){
      this.setState({current:'register'});
      this.setModalVisible(true);
    }
    else{
      {
      this.setState({current: e.key});
    }
    }
  };
```
页面开始向 API进行提交数据:
```bash
  handleSubmit(e){
		e.preventDefault();
		var myFetchOptions = {
			method: 'GET'
		};
		var formData= this.props.form.getFieldsValue();
		console.log(formData);
		fetch("http://newsapi.gugujiankong.com/Handler.ashx?action=register&username=userName&password=password&r_userName="+formData.r_userName+"&r_password="+formData.r_password+"&r_confirmPassword="+formData.r_confirmPassword,myFetchOptions).
		then(response=>response.json()).then(json=>{
			this.setState({userNickName:json.NickUserName,userid:json.UserId});

		});
		message.success("请求成功！");
		this.setModalVisible(false);
};
```
### 移动端


## 项目首页模块
## 项目详情模块
## 项目个人中心模块
## 网页优化
