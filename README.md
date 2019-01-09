# mongoDB入门教程五：搭建一个简单的登陆注册界面

一：数据库开启开始连接连接MongoDB 

1：打开一个cmd窗口(右键以管理员身份)来运行mongo.exe。同样打开bin文件，执行mongo.exe
```
cd\
cd Program Files\MongoDB\Server\4.0\bin
```
![](https://upload-images.jianshu.io/upload_images/5640239-031485e066581d0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2：输入连接命令
```
mongo
```
![](https://upload-images.jianshu.io/upload_images/5640239-375c99e09a864e88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3:我们的连接链接：
connecting to: mongodb://127.0.0.1:27017
来到浏览器测试一下
http://localhost:27017
当然了，可以设置每次开机自动连接数据库。


![](https://upload-images.jianshu.io/upload_images/5640239-fb653d52d60ed9b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



二：项目创建及其运行

1：初始化一个项目
进入D盘，使用命令，开始创建一个项目
```
d:
express loginproject  -e
```


项目创建成功
![](https://upload-images.jianshu.io/upload_images/5640239-3e5eeb801be3de3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们可以看见D盘多了一个刚刚的项目文件夹
![](https://upload-images.jianshu.io/upload_images/5640239-a321b31d77c8ff2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/5640239-a24abd37a4ac940a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



2：执行提示命令，进入项目，在项目里面安装相关依赖，把项目跑起来
```
cd loginproject
npm install
npm start 
```


![](https://upload-images.jianshu.io/upload_images/5640239-9eb9877ff1f50520.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以发现项目里面多出了两个自动生成的依赖文件

![](https://upload-images.jianshu.io/upload_images/5640239-11c866d7ab347ff9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

启动命令：npm start 
![](https://upload-images.jianshu.io/upload_images/5640239-e521b47fb91c8d20.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



3:打开浏览器，输入：http://localhost:3000/，可以访问到初始项目
![](https://upload-images.jianshu.io/upload_images/5640239-3ca987fe92ce39d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

三：打开项目，了解项目目录开始写页面代码
1：查看项目自动生成的目录
![](https://upload-images.jianshu.io/upload_images/5640239-cd2c48529479a8f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

简单的介绍一下啊

```
项目创建成功之后，生成四个文件夹，主文件app.js与配置信息文件packetage.json

bin是项目的启动文件，配置以什么方式启动项目，默认 npm start

public是项目的静态文件，放置js css img等文件

routes是项目的路由信息文件,控制地址路由

views是视图文件，放置模板文件ejs或jade等（其实就相当于html形式文件啦~)

express这样的MVC框架模式，是一个Web项目的基本构成。
```
2：开始写一些简单的界面代码，在views下面建一些需要用到的界面 ，所有代码就不一一的展示了，有兴趣的可以去我的github上面下载一下。

![](https://upload-images.jianshu.io/upload_images/5640239-7d5d9298cd726cf0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



3：新建一个models文件夹，在该文件夹下新建user.js并且写好代码
![](https://upload-images.jianshu.io/upload_images/5640239-51865df2859a4fd5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
var mongoose = require("mongoose");  //  顶会议用户组件
var Schema = mongoose.Schema;    //  创建模型
var userScheMa = new Schema({
    userid: String,
    password: String
}); //  定义了一个新的模型，但是此模式还未和users集合有关联
exports.user = mongoose.model('users', userScheMa); //  与users集合关联
```

4：在routes目下的index.js配置路由：
```
var express = require('express');
var router = express.Router();
var mongoose = require('mongoose');
var user = require('../models/user').user;
mongoose.connect('mongodb://localhost/admin');
 
/* GET home page. */
router.get('/', function(req, res) {
      res.render('index', { title: 'index' });
});
 
/*login*/
router.get('/login', function(req, res) {
    res.render('login', { title: 'login' });
});
 
/*logout*/
router.get('/logout', function(req, res) {
      res.render('logout', { title: 'logout' });
});
 
/*hompage*/
router.post('/homepage', function(req, res) {
    var query_doc = {userid: req.body.userid, password: req.body.password};
    (function(){
        user.count(query_doc, function(err, doc){
            if(doc == 1){
                console.log(query_doc.userid + ": login success in " + new Date());
                res.render('homepage', { title: 'homepage' });
            }else{
                console.log(query_doc.userid + ": login failed in " + new Date());
                res.redirect('/');
            }
        });
    })(query_doc);
});
 
module.exports = router;
```

好了

四：在mongoDB数据库里面创建用户名和密码
```
use admin
//插入用户名和密码
db.users.insert({userid:"super",password:"123"})
//查看所有
db.users.find()
```

在插入一个用户名和密码，这两个用户名密码都可以登陆。
```
db.users.insert({userid:"admin",password:"123456"})
```

![](https://upload-images.jianshu.io/upload_images/5640239-3c7929437f82e95c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开可视化工具可以看到创建的用户名密码

![](https://upload-images.jianshu.io/upload_images/5640239-a55603d44221c789.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

五：一切准备就绪，回到浏览器，查看效果
从登陆界面登陆进去，再退出来，一套流程就是如此。css就不写了，时间不多，如果感兴趣的可以自己写。

![](https://upload-images.jianshu.io/upload_images/5640239-ef43f9c60d3311ae.gif?imageMogr2/auto-orient/strip)

项目github地址：https://github.com/wangxiaoting666/loginproject

> 原文作者：祈澈姑娘 技术博客：[https://www.jianshu.com/u/05f416aefbe1](https://link.jianshu.com?t=http%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fwww.jianshu.com%2Fu%2F05f416aefbe1)
> 90后前端妹子，爱编程，爱运营，爱折腾。
坚持总结工作中遇到的技术问题，坚持记录工作中所所思所见，对于博客上面有不会的问题，可以加入qq群聊来问我：473819131.
