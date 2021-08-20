node

## 1.开发概述

### 1.为什么学习服务器端开发

- 能够和后端程序员更加紧密的配合
- 网站业务逻辑前置，学习前端技术需要后端技术支撑
- 扩展知识视野，能够站在更高的角度审视整个项目



### 2.干什么

- 实现网站的业务逻辑
- 数据的增删改查



### 3.是什么

- node是一个基于chorme v8引擎的JavaScript代码运行环境。（软件）



### 4.javasript组成

- ECMAScript
- DOM
- BOM

### 5.node组成

- ECMAScript
- 文件，网络，路径等附加API



## 2.模块化开发

**一个功能就是一个模块，多个模块可以组成完整应用，抽离一个模块不会影响其他功能的运行**

**模块开发规范**

- **node.js规定一个一个javascript文件就是一个模块**

- **模块内部定义的变量和函数默认情况下在外部无法得到**

- **模块内部可以使用exports对象进行成员导出，使用require方法导入其他模块**

  ```js
  exports.verson=verson;
  
  a=require("文件路径")；
  
  a.verson;
  
  ```

  **module.exports也可以**

- **exports是module.exports的别名（地址引用关系），导出对象最终以module.exports为准**



## 3.系统模块

### 1.fs文件操作系统

- **引用：const fs=require("fs");**

- **读取文件内容：fs.readFile('文件路径‘[,编码格式]，callback);**

  ```js
  fs.readFile('../css/base.css','utf8',(err,doc)=>{
  	if(err==null){
  		console.log(doc);
  	}
  })
  ```

  

  - 如果读取错误，err为错误对象，否则返回null
  - doc为文件内容

- **写入文件内容：fs.writeFile（‘文件路径’，‘数据’，callback）；方法**

  ```js
  const fs=require("fs");
  fs.writeFile("./06-hello0.txt","你怎么样了？",err=>{
      if(err !=null){
          console.log(err);
          return;
      }
      console.log('文件写入成功');
  })
	```
	
	
	
	- 如果没有文件则api会自动生成文件
	- err保存错误信息，如果为不为空则写入失败

### 2.path路径操作

- **path.join('路径','路径', ...)**

  ```js
  const path=require('path');
  let finalPath=path.join("node","public");
  console.log(finalPath);
  ```
  
  
  
- 相对路径和绝对路径

	- 大多数情况下使用绝对路径，因为相对路径有时候相对的是命令行工具的当前工作目录
	
	- 在读取或者设置文件路径时都会选择绝对路径

	- **使用_ _dirname获取当前文件所在的绝对路径**
	
	  ```js
	  const fs=require("fs");
	  const path=require("path");
	  fs.writeFile(path.join(__dirname,"06-hello.txt"),"你还好吗？还爱我吗？",err=>{
	      if(err !=null){
	          console.log(err);
	          return;
	      }
	      console.log("文件写入成功");
	  });
	  fs.readFile(path.join(__dirname,"06-hello.txt"),'utf8',(err,doc)=>{
	      if(err!=null){
	          console.log(err);
	      }
	      console.log(doc);
	  });
	  console.log("文件写入成功");
	  ```
	  
	  
	  
	- **require比较特殊，可以使用相对路径**

## 4.配置文件

### package.json

- 项目描述文件，记录了当前项目信息，项目名称，版本，作者，GitHub地址，当前依赖了那些第三方模块等
- 使用 npm init -y 生成
- "dependencies"（项目依赖）
- "devDependencies"（开发依赖）
- 线下开发版本：npm install（可下载全部依赖）
- 线上服务运行：npm install --production（仅下载项目依赖）

### package-lock.json

- 锁定包的版本，确保再次下载时不会因为包版本不同而产生问题
- 加快下载速度

- 里面可以添加别名

  ```json
  "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "build":"gulp first"
    },
  ```
  
  
  
- 执行时：npm run 别名

  PS D:\html\node\public\gulp-demo> npm run build
  
  > gulp-demo@1.0.0 build D:\html\node\public\gulp-demo
  > gulp first
  
  [10:00:14] Using gulpfile D:\html\node\public\gulp-demo\gulpfile.js
  [10:00:14] Starting 'first'...
  第一个任务执行成功.
  [10:00:14] Finished 'first' after 9.77 ms
  
## 5.node.js中的模块加载机制

1. require方法根据模块路径查找模块, 如果是完整路径，直接引入模块。
2. 如果模块后缀省略,先我同名S文件再找同名S文件夹
3. 如果找到了同名文件夹，找文件夹中的index.js
4. 如果文件夹中没有index.js就会去当前文件夹中的package.js文件中查找main选项中的入口文件
5. 如果找指定的入口文件不存在或者没有指定入口文件就会报错,模块没有被找到

**当模块没有路径且没有后缀时**

1. Node.js会假设它是 系统模块
2. Node.js会 去node_ modules文件夹中
3. 首先看是否有该名字的JS文件
4. 再看是否有该名字的文件夹
5. 如果是文件夹看里面是否有indexjs
6. 如果没有index.js查看该文件夹中的package.json中的main选项确定模块入口文件
7. 否则找不到报错



## 6.第三方模块

### 1.什么是第三方模块

- 别人写好的，具有特点功能的，我们能直接使用的模块即第三方模块，由于第三方模块通常都是由多个文件组成并且被放置在一个文件夹中，又名包

### 2.存在形式

- 以js文件的形式存在，提供实现项目具体功能的api接口
- 以命令行工具形式存在，辅助项目开发

### 3.获取第三方模块

- npm:node的第三方模块管理工具
- 下载：npm install 模块名称
- 卸载：npm uninstall 模块名称 or npm remove 模块名称
- 全局安装：命令行工具

	- **如 npm install nodemon -g**

		- **-g 全局安装**

- 本地安装：库文件



## 7.第三方模块nodemon

**在node.js中，每次修改文件都要在命令行工具中重新执行该文件，非常繁琐**

**使用步骤**：

- 安装：npm install nodemon -g
- 在命令行工具中用nodemon命令替代node命令执行文件



## 8.第三方模块nrm

**nrm:npm下载地址切换工具**

**npm默认的下载地址在国外，国内下载速度慢**

**使用步骤**

- npm install nrm -g
- 查询可用下载地址列表 nrm ls
- 切换npm下载地址 nrm use 下载地址名称



## 9.第三方模块gulp(已经淘汰了，webpack更好用)

**什么是gulp**

- 基于node平台开发的前端构建工具
- 将机械化操作编写成任务，想要执行机械化操作时执行一个命令行命令任务就能自动执行了
- 用机器替代手工，提高开发效率

### 能干什么

- 项目上线，html,css,js文件压缩合并
- 语法转换（es6,less...）
- 公共文件抽离
- 修改文件浏览器自动刷新

### 使用步骤

- npm install gulp 下载gulp库文件
- 在项目根目录下建立gulpfile.js文件
- 重构项目的文件夹结构：src目录放置源代码文件 ，dist目录放置构建后文件
- 在gulpfile.js文件中=编写任务
- 在命令行工具中执行gulp任务

	- npm install gulp-cli -g安装命令行工具
	- gulp 任务名称

### api

- gulp.src():获取任务要处理的文件

	- gulp-src(['路径1'，‘路径2’])

- gulp.dist():输出文件
- gulp.task():建立gulp任务
- gulp.watch():监控文件的变化
- 例子

  const gulp=require("gulp");
  const htmlmin=require("gulp-htmlmin");
  const path=require("path");
  const fileinclude=require("gulp-file-include");
  gulp.task("first",async ()=>{
      await console.log("第一个任务执行成功.");
      await gulp.src(path.join(__dirname,"src","html","header.html"))
      .pipe(gulp.dest(path.join(__dirname,"dist","html")));
  });
  gulp.task("htmlmin",async ()=>{
      await console.log("第2个任务执行成功");
      await gulp.src("./src/html/*html")
      .pipe(fileinclude())
      .pipe(htmlmin({ collapseWhitespace: true }))
      .pipe(gulp.dest('dist'));
  });
  
### 插件（具体用法可以去官网查）

- gulp-htmlmin：压缩html文件

	- *所有的：gulp.src("./src/html/*html")

- gulp-csso：压缩css
- gulp-babel：javascript语法转换
- gulp-less：less语法转换
- gulp-uglify：压缩混淆JavaScript
- gulp-file-include：公共文件包含

  gulp.task("htmlmin",async ()=>{
      await console.log("第2个任务执行成功");
      await gulp.src("./src/html/*html")
      .pipe(fileinclude())
      .pipe(htmlmin({ collapseWhitespace: true }))
      .pipe(gulp.dest('dist'));
  });
  
	- 新建文件夹common
	- 把文件公共部分提取到common文件夹的一个文件里，如heade.html
	- 通过@@include（‘公共部分文件路径’）；引回来

	  @@include("../common/head.html")
	
- 创建构建任务

	- **gulp.task('default', gulp.series('htmlmin', 'cssmin','jsmin','copy'))**

- browsersync:浏览器实时同步

## 10.服务器（重点）

### 1.服务器端基本概念

服务器端:在服务器中运行的部分，负责存储数据和处理应用逻辑。

**1.Node网站服务器**

- 能够提供网站访问服务的机器就是网站服务器,它能够接收客户端的请求,能够对请求做出响应。

**2.IP地址**

- IP是Internet Protocol Address的简写,代表互联网协议地址.

**3.城名**

- 由于IP地址难于记忆，所以产生了域名的概念，所谓域名就是平时，上网所使用的网址。
- http://www.itheima.com => http://124.165.219.100/
- 虽然在地址栏中输入的是网址但是最终还是会将域名转换为ip才能访问到指定的网站服务器。

**4.端口**

- 端口是计算机与外界通讯交流的出口，用来区分服务器电脑中提供的不同的服务.

**5.URL**

- 统一资源定位符，又叫URL (Uniform ResourceLocator),专为标识Internet网上资源位置而设的一种编址方式，我们平时所说的网页地址指的即是URL.
- 组成

	- 传输协议://服务器IP或域名:端口/资源所在位置标识
	- http://www.itcast.cn/news/20181018/09152238514.html
	- http:超文本传输协议，提供了一种发布和接收HTML页面的方法。

### 2.创建web服务器

![image-20210820213322067](%E7%AC%94%E8%AE%B0/1/img/image-20210820213322067.png)

### 3.HTTP协议

超文本传输协议(英文: HyperText Transfer Protocol,缩写: HTTP)规定了如何从网站服务器传输超文本到本地浏览器,它基于客户端服务器架构工作，是客户端(用户)和服务器端(网站)请求和应答的标准。

### 4.报文

在HTTP请求和响应的过程中传递的数据块就叫报文,包括要传送的数据和一些附加信息,并且要遵守规定好的格式。

请求报文

- get请求数据

- post发送数据

- 请求地址

  ![image-20210820213521158](%E7%AC%94%E8%AE%B0/1/img/image-20210820213521158.png)

  代码![image-20210820213537297](%E7%AC%94%E8%AE%B0/1/img/image-20210820213537297.png)

响应报文

- HTTP状态码

	- ●200 请求成功
	- ●404 请求的资源没有被找到
	- ●500 服务器端错误
	- ●400客户端请求有语法错误

- 内容类型

	- ●text/html
	- ●text/css
	- ●application/javascript
	- ●image/jpeg
	- ●application/json

- 代码

  ```js
  res.writeHead(状态码,{'content-type':'数据类型'})
  //例如 res.writeHead(200, 'Content-Type':'text/html;charset=utf8'})；
  
  //charset=utf8(设置编码格式)
  ```

  

get请求参数

- 客户端向服务器端发送请求时，有时需要携带一些客户信息， 客户信息需要通过请求参数的形式传递到服务器端，比如登录操作。
- 引用内置模块

	- const url=require('url')
	- **const {pathname,query}=url.parse(req.url,true)**

		- 应用解构
		- true表示转换成对象
		- pathname地址ming
		- query传输来的数据
- 代码![image-20210820213842390](%E7%AC%94%E8%AE%B0/1/img/image-20210820213842390.png)
- **post请求参数**
  - 参数被放置在请求体中进行传输
  - 获取POST参数需要使用data事件和end事件
  - 使用querystring系统模块将参数转换为对象格式

代码

![image-20210820214036146](%E7%AC%94%E8%AE%B0/1/img/image-20210820214036146.png)



### 5.静态资源访问

- 服务器端不需要处理，可以直接响应给客户端的资源就是静态资源，例如CSS、JavaScript、 image文件。
- 代码
- ![image-20210820214106652](%E7%AC%94%E8%AE%B0/1/img/image-20210820214106652.png)
	- 通过writeHead设置编码格式
	- 核心逻辑：
	
		- 把URL地址转换为物理路径
		- 然后把成功读取的文件返回
	
	- 可以通过第三方模块mime动态分析请求路径上的资源类型
	
		- mime.getType(路径)
		- 返回值是类型



### 6.动态资源访问

- 相同的请求地址不同的响应资源，这种资源就是动态资源。



### 7.路由

- 路由是指客户端请求地址与服务器端程序代码的对应关系。简单的说，就是请求什么响应什么。
- 核心逻辑

	- 判断请求类型
	- 获取请求地址
	- 对不同的请求类型依据地址做出不同的响应

- 代码![image-20210820214139697](%E7%AC%94%E8%AE%B0/1/img/image-20210820214139697.png)



## 11.同步异步

### 1.同步api

- 同步API:只有当前API执行完成后，才能继续执行下-一个API



### 2.异步api

- 异步API:当前API的执行不会阻塞后续代码的执行

- 例如

  ```js
  console. log('before') ;
  setTimeout (() => { console.log('last')}，2000);
  console. log('after') ;
  ```
  
  
  
### 3.区别

- 同步AP可以从返回值中拿到API执行的结果,但是异步API是不可以的
- 回调函数

  - 自己定义函数让别人去调用。
  - ![image-20210820214529390](%E7%AC%94%E8%AE%B0/1/img/image-20210820214529390.png)
  - 通过回调函数我们可以拿到异步API的返回结果

- 同步API从上到下依次执行，前面代码会阻塞后面代码的执行
- 执行顺序

	- 先同步后异步
	- ![image-20210820214545196](%E7%AC%94%E8%AE%B0/1/img/image-20210820214545196.png)
	
	- ![image-20210820214614575](%E7%AC%94%E8%AE%B0/1/img/image-20210820214614575.png)

### 4.回调地狱

- 异步api解决顺序问题只能用回调函数嵌套
- 因此容易导致回调地狱
- ![image-20210820214645915](%E7%AC%94%E8%AE%B0/1/img/image-20210820214645915.png)

### 5.Promise

- Promise出现的目的是解决Nodejs异步编程中回调地狱的问题。

- 语法

  ```js
  const fs=require('fs');
  let promise=new Promise((resolve,reject)=>{
      fs.readFile('./txt/11.txt','utf8',(error,result)=>{
          if(error!=null){
              reject(error);
          }else{
              resolve(result);
          }
        })  
      });
  promise.then((resolve)=>{
      console.log(resolve);
  }).catch((reject)=>console.log(reject));
  ```
  
  
  
- 例子![image-20210820214707815](%E7%AC%94%E8%AE%B0/1/img/image-20210820214707815.png)

### 6.异步函数

- **异步函数是异步编程语法的终极解决方案，它可以让我们将异步代码写成同步的形式，让代码不再有回调函数嵌套,使代码变得清晰明了。**
- **async关键字**

	- **在普通函数定义的前面加上async关键字 普通函数就变成了异步函数**
	- **异步函数默认的返回值是promise对象(return相当于resolve)**
	- **在异步函数内部使用throw关键字进行错误的抛出（相当于reject）**
	- **调用异步函数再链式调用then方法获取异步函数执行结果**
	- **调用异步函数再链式调用catch方法获取异步函数执行的错误信息**

- **await关键字**

	- **1. await关键字只能出现在异步函数中**
	- **2. await promise await后面只能写promise对象其他类型的API不不可以的**
	- **3. await关键字可是暂停异步函数向下执行直到promise返回结果**
	- 代码![image-20210820214741923](%E7%AC%94%E8%AE%B0/1/img/image-20210820214741923.png)

- **promisify方法**
	- **改造现有异步函数api让其返回promise对象从而支持异步函数语法**
	- **const promisify = require( ' util' ). promisify;**
	- 代码![image-20210820214757376](%E7%AC%94%E8%AE%B0/1/img/image-20210820214757376.png)



## 12.数据库

### 1.为什么要使用数据库

- 动态网站中的数据都是 存储在数据库中的
- 数据库可以用来持久存储客户端通过表单收集的用户信息
- 数据库软件本身可以对数据进行高效的管理

**在一个数据库软件中可以包含多个数据仓库，在每个数据仓库中可以包含多个数据集合,每个数据集合中可以包含多文档(具体的数据)。**

### **2.相关概念**

- **database**

	- **数据库，mongoDB数据库软件中可以建立多个数据库**

- **collection**
	- **集合，一组数据的集合，可以理解为JavaScript中的数组**
	
- **document**

	- **文档，一条具体的数据，可以理解为JavaScript中的对象**

- **field**
- **字段，文档中的属性名称，可以理解为JavaScript中的对象属性**

### 3.Mongoose第3方包

- 使用Node.js操作MongoDB数据库需要依赖Node.js第三方包mongoose
- 使用npm install mongoose命令下载

### 4.数据库相关操作(重点)

- **net start(stop) mongodb打开/关闭数据库**

- **使用mongoose提供的connect方法即可连接数据库。**

	- 代码![image-20210820214931730](%E7%AC%94%E8%AE%B0/1/img/image-20210820214931730.png)

- **在MongoDB中不需要显式创建数据库，如果正在使用的数据库不存在，MongoDB会自动创建。**

  **1.创建集合**

  一.是对对集合设定规则，

  二. 是创建集合,创建mongoose Schema构造函数的实例即可创建集合。

  代码![image-20210820215008214](%E7%AC%94%E8%AE%B0/1/img/image-20210820215008214.png)

  **2.创建文档**

  - 创建文档实际上就是向集合中插入数据。
  - ①创建集合实例。
  - ②调用实例对象下的save方法将数据保存到数据库中。
  - 第一种方法![image-20210820215021187](%E7%AC%94%E8%AE%B0/1/img/image-20210820215021187.png)
  - **常用**（第二种方法）![image-20210820215031991](%E7%AC%94%E8%AE%B0/1/img/image-20210820215031991.png)

  **3.导入数据**

  - **找到mongodb数据库的安装目录，将安装目录下的bin目录放置在环境变量中。才能执行mongoimport**
  - **mongoimport  - d数据库名称 - c集合名称 --file要导入的数据文件**

  **4.查询数据**

  - **find里面传递对象作为查找条件**

    ```js
    Course.find().then(result => console.log (result))//返回一组文档
    ```

    ```js
    Course.findOne({name: ' node. js基础'}).then (result => console.log(result) )//返回一个文档
    ```

    ![image-20210820215839299](%E7%AC%94%E8%AE%B0/1/img/image-20210820215839299.png)

  - **匹配大于小于**

    ```js
    User.find({age:{$gt: 20，$lt: 50}}).then(result =>console.log(result))
    ```

  - **匹配包含**

    ```js
    User.find({hobbies:{ $in ['睡觉'，‘吃饭’]}}).then (result => console.log (result))//in后面是数组，可以包含多个
    ```

  - **选择要查询字段**

    ```js
    User.find().select('name email').then(result =>console.log(result))
    
    //不同字段要用空格隔开
    
    //不想显示某些字段在前面加-号
    ```

    

  - **升序排序**

    ```js
    User.sort('age').then(result =>console.log(result))//改为降序在age前面加-号
    ```

  - **skip 跳过多少条数据limit 限制查询数量**

    ```js
    User.find().skip(2).limit(2).then(result =>console.log(result))
    ```

    ![image-20210820220351500](%E7%AC%94%E8%AE%B0/1/img/image-20210820220351500.png)

  **5.删除数据**

  - **删除单个**

    ```js
    Course.findoneAndDelete ({}).then(result => console.log(result))//返回删除文档
    ```

  - **删除多个**

    ```js
    User.deleteMany({}).then(result => console.log(result) )
    //返回删除个数和操作是否成功
    //注意：传空对象代表删除所有
    ```

    

  **6.更新数据**

  - **更新单个**

    ```js
    User.updateOne({查询条件}，{要修改的值}).then(result => console.log(result))
    ```

    

  - **更新多个**

    ```js
    User.updateMany({查询条件}，{要更改的值}).then(result => console.log(result))
    ```

    

  **7.验证字段**

  - 在创建集合规则时，可以设置当前字段的验证规则，验证失败就则输入插入失败。
  - **属性：[true,'自定义报错信息']**
  - **required:true(必选字段)**
  - **minlength(最小长度)**
  - **maxlength(最大长度)**
  - **trim:true(删除两边空格)**
  - **enum: ['html', 'ss, javascript', 'nodejs']**

  	- **只有符合数组的才能通过验证**
  	- **enum:{values:[],message:''}**

  - **validate: 自定义验证器**

  	- **validate:{validator:{(v)=>{定义验证函数}},message:'传错误提示信息'}**

  - 若是Date类型

  	- **可以设置dafault:Date.now**

  - 代码![image-20210820220808279](%E7%AC%94%E8%AE%B0/1/img/image-20210820220808279.png)
  - 获取错误信息![image-20210820220842049](%E7%AC%94%E8%AE%B0/1/img/image-20210820220842049.png)

  **8.集合关联**

  - 通常不同集合的数据之间是有关系的，例如文章信息和用户信息存储在不同集合中，但文章是某个用户发表的，要查询文章的所有信息包括发表用户，就需要用到集合关联。

  - 使用id对集合进行关联

  - 使用populate方法进行关联集合查询

  - ![image-20210820220908192](%E7%AC%94%E8%AE%B0/1/img/image-20210820220908192.png)

  - **代码**

    ![image-20210820220930254](%E7%AC%94%E8%AE%B0/1/img/image-20210820220930254.png)

    - **关联使用type:mongoose.Schema.Types.ObjectId,ref:'被关联集合名称'**
    - **使用populate方法里传关联字段**

## 13.案例：用户信息增删改查

1. 搭建网站服务器，实现客户端与服务器端的通信

2. 连接数据库，创建用户集合，向集合中插入文档

3. 当用户访问/list时，将所有用户信息查询出来

4. 
   - 实现路由功能
   - 呈现用户列表页面

5. 从数据库中查询用户信息 将用户信息展示在列表中

6. 将用户信息和表格HTML进行拼接并将拼接结果响应回客户端

7. 当用户访问/add时，呈现表单页面，并实现添加用户信息功能

8. 当用户访问/modify时，呈现修改页面，并实现修改用户信息功能

9. - 增加页面路由 呈现页面

   	- 在点击修改按钮的时候 将用户ID传递到当前页面
   	- 从数据库中查询当前用户信息 将用户信息展示到页面中

   - 实现用户修改功能

   	- 指定表单的提交地址以及请求方式
   	- 接受客户端传递过来的修改信息 找到用户 将用户信息更改为最新的

10. 当用户访问/delete时，实现用户删除功能

11. ### 代码

```js
// 搭建网站服务器，实现客户端与服务器端的通信
// 连接数据库，创建用户集合，向集合中插入文档
// 当用户访问/list时，将所有用户信息查询出来
//  实现路由功能
//  呈现用户列表页面
//  从数据库中查询用户信息 将用户信息展示在列表中
// 将用户信息和表格HTML进行拼接并将拼接结果响应回客户端
// 当用户访问/add时，呈现表单页面，并实现添加用户信息功能
// 当用户访问/modify时，呈现修改页面，并实现修改用户信息功能
//  修改用户信息分为两大步骤
//      1.增加页面路由 呈现页面
//          1.在点击修改按钮的时候 将用户ID传递到当前页面
//          2.从数据库中查询当前用户信息 将用户信息展示到页面中
//      2.实现用户修改功能
//          1.指定表单的提交地址以及请求方式
//          2.接受客户端传递过来的修改信息 找到用户 将用户信息更改为最新的
// 当用户访问/delete时，实现用户删除功能

const http = require('http');

const url = require('url');
const querystring = require('querystring');

require('./model/index.js');
const User = require('./model/user');



// 创建服务器
const app = http.createServer();

// 为服务器对象添加请求事件
app.on('request', async (req, res) => {
    // 请求方式
    const method = req.method;
    // 请求地址
    const { pathname, query } = url.parse(req.url, true);

    if (method == 'GET') {
        // 呈现用户列表页面
        if (pathname == '/list') {
            // 查询用户信息
            let users = await User.find();
            // html字符串
            let list = `
                <!DOCTYPE html>
                <html lang="en">
                <head>
                    <meta charset="UTF-8">
                    <title>用户列表</title>
                    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css">
                </head>
                <body>
                    <div class="container">
                        <h6>
                            <a href="/add" class="btn btn-primary">添加用户</a>
                        </h6>
                        <table class="table table-striped table-bordered">
                            <tr>
                                <td>用户名</td>
                                <td>年龄</td>
                                <td>爱好</td>
                                <td>邮箱</td>
                                <td>操作</td>
                            </tr>
            `;

            // 对数据进行循环操作
            users.forEach(item => {
                list += `
                    <tr>
                        <td>${item.name}</td>
                        <td>${item.age}</td>
                        <td>
                `;

                item.hobbies.forEach(item => {
                    list += `<span>${item}</span>`;
                })

                list += `</td>
                        <td>${item.email}</td>
                        <td>
                            <a href="/remove?id=${item._id}" class="btn btn-danger btn-xs">删除</a>
                            <a href="/modify?id=${item._id}" class="btn btn-success btn-xs">修改</a>
                        </td>
                    </tr>`;
            });

            list += `
                        </table>
                    </div>
                </body>
                </html>
            `;
            res.end(list);
        }else if (pathname == '/add') {
            // 呈现添加用户表单页面
            let add = `
                <!DOCTYPE html>
                <html lang="en">
                <head>
                    <meta charset="UTF-8">
                    <title>用户列表</title>
                    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css">
                </head>
                <body>
                    <div class="container">
                        <h3>添加用户</h3>
                        <form method="post" action="/add">
                          <div class="form-group">
                            <label>用户名</label>
                            <input name="name" type="text" class="form-control" placeholder="请填写用户名">
                          </div>
                          <div class="form-group">
                            <label>密码</label>
                            <input name="password" type="password" class="form-control" placeholder="请输入密码">
                          </div>
                          <div class="form-group">
                            <label>年龄</label>
                            <input name="age" type="text" class="form-control" placeholder="请填写邮箱">
                          </div>
                          <div class="form-group">
                            <label>邮箱</label>
                            <input name="email" type="email" class="form-control" placeholder="请填写邮箱">
                          </div>
                          <div class="form-group">
                            <label>请选择爱好</label>
                            <div>
                                <label class="checkbox-inline">
                                  <input type="checkbox" value="足球" name="hobbies"> 足球
                                </label>
                                <label class="checkbox-inline">
                                  <input type="checkbox" value="篮球" name="hobbies"> 篮球
                                </label>
                                <label class="checkbox-inline">
                                  <input type="checkbox" value="橄榄球" name="hobbies"> 橄榄球
                                </label>
                                <label class="checkbox-inline">
                                  <input type="checkbox" value="敲代码" name="hobbies"> 敲代码
                                </label>
                                <label class="checkbox-inline">
                                  <input type="checkbox" value="抽烟" name="hobbies"> 抽烟
                                </label>
                                <label class="checkbox-inline">
                                  <input type="checkbox" value="喝酒" name="hobbies"> 喝酒
                                </label>
                                <label class="checkbox-inline">
                                  <input type="checkbox" value="烫头" name="hobbies"> 烫头
                                </label>
                            </div>
                          </div>
                          <button type="submit" class="btn btn-primary">添加用户</button>
                        </form>
                    </div>
                </body>
                </html>
            `;
            res.end(add)
        }else if (pathname == '/modify') {
            let user = await User.findOne({_id: query.id});
            let hobbies = ['足球', '篮球', '橄榄球', '敲代码', '抽烟', '喝酒', '烫头', '吃饭', '睡觉', '打豆豆']
            console.log(user)
            // 呈现修改用户表单页面
            let modify = `
                <!DOCTYPE html>
                <html lang="en">
                <head>
                    <meta charset="UTF-8">
                    <title>用户列表</title>
                    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css">
                </head>
                <body>
                    <div class="container">
                        <h3>修改用户</h3>
                        <form method="post" action="/modify?id=${user._id}">
                          <div class="form-group">
                            <label>用户名</label>
                            <input value="${user.name}" name="name" type="text" class="form-control" placeholder="请填写用户名">
                          </div>
                          <div class="form-group">
                            <label>密码</label>
                            <input value="${user.password}" name="password" type="password" class="form-control" placeholder="请输入密码">
                          </div>
                          <div class="form-group">
                            <label>年龄</label>
                            <input value="${user.age}" name="age" type="text" class="form-control" placeholder="请填写邮箱">
                          </div>
                          <div class="form-group">
                            <label>邮箱</label>
                            <input value="${user.email}" name="email" type="email" class="form-control" placeholder="请填写邮箱">
                          </div>
                          <div class="form-group">
                            <label>请选择爱好</label>
                            <div>
                                
                            
            `;

            hobbies.forEach(item => {
                // 判断当前循环项在不在用户的爱好数据组
                let isHobby = user.hobbies.includes(item);
                if (isHobby) {
                    modify += `
                        <label class="checkbox-inline">
                          <input type="checkbox" value="${item}" name="hobbies" checked> ${item}
                        </label>
                    `
                }else {
                    modify += `
                        <label class="checkbox-inline">
                          <input type="checkbox" value="${item}" name="hobbies"> ${item}
                        </label>
                    `
                }
            })

            modify += `
                            </div>
                          </div>
                          <button type="submit" class="btn btn-primary">修改用户</button>
                        </form>
                    </div>
                </body>
                </html>
            `;
            res.end(modify)
        }else if (pathname == '/remove') {
            // res.end(query.id)
            await User.findOneAndDelete({_id: query.id});
            res.writeHead(301, {
                Location: '/list'
            });
            res.end();
        }
    }else if (method == 'POST') {
        // 用户添加功能
        if (pathname == '/add') {
            // 接受用户提交的信息
            let formData = '';
            // 接受post参数
            req.on('data', param => {
                formData += param;
            })
            // post参数接受完毕
            req.on('end', async () => {
                let user = querystring.parse(formData)
                // 将用户提交的信息添加到数据库中
                await User.create(user);
                // 301代表重定向
                // location 跳转地址
                res.writeHead(301, {
                    Location: '/list'
                });
                res.end();
            })
        }else if (pathname == '/modify') {
            // 接受用户提交的信息
            let formData = '';
            // 接受post参数
            req.on('data', param => {
                formData += param;
            })
            // post参数接受完毕
            req.on('end', async () => {
                let user = querystring.parse(formData)
                // 将用户提交的信息添加到数据库中
                await User.updateOne({_id: query.id}, user);
                // 301代表重定向
                // location 跳转地址
                res.writeHead(301, {
                    Location: '/list'
                });
                res.end();
            })
        }
    }

});
// 监听端口
app.listen(3000);
```



## 14.模板引擎(由于vue等框架的出现传统网页开发过时了)

模板引擎是第三方模块。

让开发者以更加友好的方式拼接字符串，使项目代码更加清晰、更加易于维护。

### art-template模板引擎

- 1.在命令行工具中使用npm install art-template命令进行下载
- 2.使用const template = require ('art-template')引入模板引擎
- 3.告诉模板引擎要拼接的数据和模 板在哪const html = template( '模板路径’ ,数据);

### art-template模板语法

- 将某项数据输出在模板中，标准语法和原始语法如下:

	- ●标准语法: {{数据}}
	- ●原始语法: <%=数据%>
	- 图示

- 原文输出

	- 标准语法: {{@数据}}
	- 原始语法:<%-数据%>

- 条件判断

	- 标准语法

		- **{{if条件}} ... {{/if}}**
		- **{{if v1}} ... {{else if v2}} ... {{/if}}**

	- 原始语法

		- **<&if(value){8>...<8}8>**
		- **<8 if (v1) { 8>...<8} else if (v2) { 8>... <8 } 8>**

- 循环

	- 标准语法

		- **{{each target}}
     {{$index}} {{$value}}
    {{/each}}**

	- 原始语法

		- **<% for(var i = 0; i < target.length; i++){ %>
     <%= i %> <%= target[i] %>
    <% } %>**

### 子模板

- 使用子模板可以将网站公共区块(头部、底部)抽离到单独的文件中。
- **标准语法：{{include '模板'}}**
- **原始语法：<%include('模板') %>**

### 模板继承

- 使用模板继承可以将网站HTML骨架抽离到单独的文件中，其他页面模板可以继承骨架文件

   ```html
   <!doctype html>
   <html>
       <head>
         <meta charset="utf-8">
         <title>HTML骨架模板</title>
         {{block 'head'}}{{/block}}
    	</head>
   	<body>
         {{block 'content'}}{{/block}}
     </body>
   </html>
   ```

   ```html
     <head>
         <meta charset="utf-8">
         <title>HTML骨架模板</title>
         {{block 'head'}}{{/block}}
     </head>
   ```

   - **通过{{block “名称”}}{{/block}}在模板中预留位置**

- ** <!--index.art 首页模板-->
 {{extend './layout.art'}}
	 {{block 'head'}} <link rel="stylesheet" href="custom.css"> {{/block}}
	 {{block 'content'}} <p>This is just an awesome page.</p> {{/block}}
	**

	- **通过{{extend “路径”}}引入模板**
	- **通过{{block “名称”}} 内容{{/block}}填空**

### 模板配置

- **向模板中导入变量 template.defaults.imports.变量名 = 变量值;**
- **设置模板根目录 template.defaults.root = 模板目录**
- **设置模板默认后缀 template.defaults.extname = '.art'**



### 15.第三方模块dataformat

- 返回处理时间的方法
- 可查官网



## 16.案例-学生档案管理

### 1.流程

- 建立项目文件夹并生成项目描述文件
- 创建网站服务器实现客户端和服务器端通信
- 连接数据库并根据需求设计学员信息表
- 创建路由并实现页面模板呈递
- 实现静态资源访问
- 实现学生信息添加功能

	- 添加学生信息功能步骤分析
	- 1.在模板的表单中指定请求地址与请求方式
	- 2.为每一个表单项添加name属性
	- 3. 添加实现学生信息功能路由
	- 4.接收客户端传递过来的学生信息
	- 5.将学生信息添加到数据库中
	- 6.将页面重定向到学生信息列表页面

- 实现学生信息展示功能

### 2.技术点

- 第三方模块router(路由)

	- 功能:实现路由
	- 使用步骤:

		- 1.获取路由对象
		- 2.调用路由对象提供的方法创建路由
		- 3.启用路由， 使路由生效

	- **注意：调用router(req,res,()=>{});第三个参数必填**
	- 示例代码

- 第三方模块serve-static

	- 功能:实现静态资源访问服务
	- 步骤:

		- 1.引入serve-static模块 获取创建静态资源服务功能的方法
		- 2.调用方法创建静态资源服务并指定静态资源服务目录
		- 3.启用静态资源服务功能

	- **注意：调用serve(req,res,()=>{});第三个参数必填**
	- 示例代码

## 17.express框架(重点)

**Express是一个基于Node平台的web应用开发框架，它提供了一系列的强大特性，帮助你创建各种Web应用**

### 1.Express框架特性

- 提供了方便简洁的路由定义方式
- 对获取HTTP请求参数进行了简化处理
- 对模板引擎支持程度高，方便渲染动态HTML页面
- 提供了中间件机制有效控制HTTP请求
- 拥有大量第三方中间件对功能进行扩展

### 2.中间件

- 中间件概念

	- 中间件就是一堆方法, 可以接收客户端发来的请求、可以对请求做出响应，也可以将请求继续交给下一一个中间件继续处理。
	- **中间件方法由Express提供，负责拦截请求，请求处理函数由开发人员提供，负责处理请求.**

		- 图示

	- **可以针对同一个请求设置多个中间件,对同一个请求进行多次处理。**
	- **默认情况下，请求从上到下依次匹配中间件，一旦匹配成功，终止匹配。**
	- **可以调用next方法将请求的控制权交给下一个中间件，直到遇到结束请求的中间件。**

- 中间件方法

  - app.get()
  - app.post()
  - app.use()

  	- app.use 匹配所有的请求方式，可以直接传入请求处理函数，代表接收所有的请求。
  	- app.use 第一个参数也可以传入请求地址，代表不论什么请求方式，只要是这个请求地址就接收这个请求。

- 中间件应用

	- **路由保护，客户端在访问需要登录的页面时，可以先使用中间件判断用户登录状态，用户如果未登录，则拦截请求，直接响应，禁止用户进入需要登录的页面。**
	- **网站维护公告，在所有路由的最上面定义接收所有请求的中间件，直接为客户端做出响应，网站正在维护中。**
	- **自定义404页面**

- 错误处理中间件

	- 在程序执行的过程中，不可避免的会出现一些无法预料的错误，比如文件读取失败，数据库连接失败。错误处理中间件是一个集中处理错误的地方
	- ** app.use((err, req, res, next) => {
     res.status(500).send('服务器发生未知错误');
    })**
	- **同步自动捕获**
	- **当程序出现错误时，调用next()方法，并且将错误信息通过参数的形式传递给next()方法，即可触发错误处理中间件。**

- 捕获错误

	- **在node.js中,异步API的错误信息都是通过回调函数获取的，支持Promise对象的异步API发生错误可以通过catch方法捕获。**
	- **try catch可以捕获异步函数以及其他同步代码在执行过程中发生的错误，但是不能获取其他类型的API发生的错误。**

- 请求处理函数

	- res.send()方法

		- 1.send方法内部会检测响应内容的类型
		- 2.send方法会自动设置http状态码
		- 3.send方法会帮我们自动设置响应的内容类型及编码
		- 4.可以传递json对象

	- res.status(状态码)
	- req.url(请求路径)
	- res.redirect(‘重定向路径’)

### 模块化构建路由

- ![image-20210820221917093](%E7%AC%94%E8%AE%B0/1/img/image-20210820221917093.png)
- 图示![image-20210820221929372](%E7%AC%94%E8%AE%B0/1/img/image-20210820221929372.png)

### 请求参数获取

- get

	- **req.query**

- post

	- **Express中接收post请求参数需要借助第包body-parser.**
	- 图示

		- **extended: false 方法内部使用querystring模块处理请求参数的格式**
		- **extended:true方法内部使用第三方模块qs处理请求参数的格式**

### 路由参数

- 图示

  ![image-20210820221949064](%E7%AC%94%E8%AE%B0/1/img/image-20210820221949064.png)

- 注意：必须得有请求参数，不然访问不到路由

### 静态资源访问

- **通过Express内置的express.static可以方便地托管静态文件，例如img、CSS. JavaScript 文件等。**

  ```js
  app.use('/list',express.static(path.join(__dirname,'public')));
  //'/list'为指定路径（虚拟）
  ```

### 模板引擎(在前后端分离模式中淘汰)

- 为了使art-template模板引擎能够更好的和Express框架配合，模板引擎官方在原art-template模板引擎的基础.上封装了express- art-template.
- **使用npm install art-template express-art-template命令进行安装。**
- res.render()

  - **1.拼接模板路径**
  - **2.拼接模板后缀**
  - **3.哪一个模板和哪一个数据进行拼接**
  - **4.将拼接结果响应给了客户端**


### 3.app.locals（重点）

- 将变量设置到app.locals对象下面，这个数据在所有的模板中都可以获取到。

## 大型案例-多人博客管理系统

### **项目案例初始化**

-  建立项目所需文件夹

	- public 静态资源
	- model 数据库操作
	- route 路由
	- views 模板

- 初始化项目描述文件

	- npm init -y

- 下载项目所需第三方模块

	- npm install express mongoose art-template express-art-template

- 创建网站服务器
- 构建模块化路由
-  构建博客管理页面模板

	- 解决外链问题

		- **模板文件中，外链用绝对路径（/）表示绝对路径**

	- common

		- 抽离公共部分（子模板）
		- 建立骨架模板（继承模板）

### **登录**

- 创建用户集合，初始化用户

	- 连接数据库
	- 创建用户集合

		- **unique:true(保证唯一)**

	- 初始化用户

- 为登录表单项设置请求地址、请求方式以及表单项name属性
- 当用户点击登录按钮时，客户端验证用户是否填写了登录表单
-  如果其中一项没有输入，阻止表单提交
- 服务器端接收请求参数，验证用户是否填写了登录表单
- 如果其中一项没有输入，为客户端做出响应，阻止程序向下执行
- 根据邮箱地址查询用户信息
- 如果用户不存在，为客户端做出响应，阻止程序向下执行
- 如果用户存在，将用户名和密码进行比对
-  比对成功，用户登录成功
- 比对失败，用户登录失败
-  保存登录状态
-  密码加密处理

### **密码加密bcrypt**

- **图示**

  ![image-20210820222317674](%E7%AC%94%E8%AE%B0/1/img/image-20210820222317674.png)

- 依赖

	- python 2.x
	- node-gyp

		- npm install -g node-gyp

	- windows-build-tools

		- npm install --global --production windows-build-tools

## 18.**cookie和session**

- **cookie：浏览器在电脑硬盘中开辟的一块空间，主要供服务器端存储数据**

	- **cookie中的数据是以域名的形式进行区分的**
	- **cookie中的数据是有过期时间的，超过时间数据会被浏览器自动删除**
	- **cookie中的数据会随着请求被自动发送到服务器端**

- **session：实际上就是一个对象，存储在服务器端的内存中，在session对象中也可以存储多条数据，每一条数据都有一个sessionid做为唯一标识**

- **实现登录原理**![image-20210820222526944](%E7%AC%94%E8%AE%B0/1/img/image-20210820222526944.png)

- **在node.js中需要借助express-session实现session功能**

  ```js
  const session = require('express-session');
  
  app.use(session({
  resave:true,
  saveUninitialized:true,
  secret: 'secret key'}));
  ```

  - **重启服务器session失效**
  - [**session常见用法见博客**](https://blog.csdn.net/void_fan/article/details/110679200)

### 登录拦截

- 图示![image-20210820222541125](%E7%AC%94%E8%AE%B0/1/img/image-20210820222541125.png)

### **新增用户**

- 1. 为用户列表页面的新增用户按钮添加链接
- 2. 添加一个连接对应的路由，在路由处理函数中渲染新增用户模板
- 3 .为新增用户表单指定请求地址、请求方式、为表单项添加name属性
- 4. 增加实现添加用户的功能路由
- 5. 接收到客户端传递过来的请求参数
- 6. 对请求参数的格式进行验证
- 7. 验证当前要注册的邮箱地址是否已经注册过
- 8. 对密码进行加密处理
- 9. 将用户信息添加到数据库中
- 10. 重定向页面到用户列表页面

### 第三方模块Joi

- JavaScript对象的规则描述语言和验证器。
- 定义
- 处理
- 详情操作查官网

### **JSON. stringify()将对象数据类型转换为字符串数据类型**

### **JSON.parse()将字符串类型转换为对象类型**

### 数据分页

- User.countDocuments({})查询数量
- 设置每页数量
- 计算总页数
- 图示![image-20210820222618175](%E7%AC%94%E8%AE%B0/1/img/image-20210820222618175.png)

### **用户信息修改**

- 1. 将要修改的用户ID传递到服务器端
- 2. 建立用户信息修改功能对应的路由
- 3. 接收客户端表单传递过来的请求参数
- 4. 根据id查询用户信息，并将客户端传递过来的密码和数据库中的密码进行比对
- 5. 如果比对失败，对客户端做出响应
- 6. 如果密码对比成功，将用户信息更新到数据库中

### 妙啊

![image-20210820222652312](%E7%AC%94%E8%AE%B0/1/img/image-20210820222652312.png)

### **用户信息删除**

- 1. 在确认删除框中添加隐藏域用以存储要删除用户的ID值
- 2. 为删除按钮添自定义属性用以存储要删除用户的ID值
- 3. 为删除按钮添加点击事件，在点击事件处理函数中获取自定义属性中存储的ID值并将ID值存储在表单的隐藏域中
- 4. 为删除表单添加提交地址以及提交方式
- 5. 在服务器端建立删除功能路由
- 6. 接收客户端传递过来的id参数
- 7. 根据id删除用户

### 文件上传（重点）

- enctype指定表单数据的编码类型
- application/ X -wwW- form- urlencoded

	- name=zhangsan&age=20

- multipart/form-data将表单数据编码成二进 制类型

### 第三方模块formidable（重点）

- 作用：解析表单，支持get请求参数，post请求参数、文件上传
- 图示
- ![image-20210820222743576](%E7%AC%94%E8%AE%B0/1/img/image-20210820222743576.png)

### 文件读取(图片预览)

- 图示
- ![image-20210820222822074](%E7%AC%94%E8%AE%B0/1/img/image-20210820222822074.png)
- 代码![image-20210820222834084](%E7%AC%94%E8%AE%B0/1/img/image-20210820222834084.png)

### **第三方模块mongoose-sex-page**（重点）

```js
const pagination = require ( mongoose-sex -page') ;

pagination(集合构造函数).page(1).size(20).distlay(8).exec();
```



### 第三方模块morgan

- 将客户端发送给服务器的请求信息打印到控制台当中
- **app.use(morgan('dev'));**



### **mongoDB数据库添加账号**

- 1. 以系统管理员的方式运行powershell
- 2. 连接数据库 mongo
- 4. 切换到admin数据库 use admin
- 5. 创建超级管理员账户 db.createUser()

	-  db.createUser({user:'root',pwd:'root',roles:['root']})

- 6. 切换到blog数据 use blog
- 7. 创建普通账号 db.createUser()

	- db.createUser({user:'yeshifu',pwd:'123456',roles:['readWrite']})
	- 输入exit退出数据库环境

- 8. 卸载mongodb服务

	- 1. 停止服务 net stop mongodb
	- 2. mongod --remove

- 9. 创建mongodb服务

	- 

- 10. 启动mongodb服务 net start mongodb
- 11. 在项目中使用账号连接数据库

	- mongoose.connect('mongodb://user:pass@localhost:port/database')

- [配置conf文件](https://sevieryang.blog.csdn.net/article/details/84403964)

### 环境

- 环境，就是指项目运行的地方
- **开发环境**

	- **当项目处于开发阶段，项目运行在开发人员的电脑上,项目所处的环境就是开发环境**

- **生产环境**

	- **当项目开发完成以后，要将项目放到真实的网站服务器电脑中运行,项目所处的环境就是生产环境。**

- **因为在不同的环境中，项目的配置是不一样的， 需要在项目代码中判断当前项目运行的环境，根据不同的环境应用不同的项目配置。**
- 如何区分

	- 图示![image-20210820223051645](%E7%AC%94%E8%AE%B0/1/img/image-20210820223051645.png)
	- 获取系统环境变量

		- **process.env.NODE_ENV**
		- 图示![image-20210820223104495](%E7%AC%94%E8%AE%B0/1/img/image-20210820223104495.png)

### 第三方模块config(重点)

- **作用:允许开发人员将不同运行环境下的应用配置信息抽离到单独的文件中，模块内部自动判断当前应用的运行环境，并读取对应的配置信息，极大提供应用配置信息的维护成本，避免了当运行环境重复的多次切换时,手动到项目代码中修改配置信息**
- **使用步骤**

	- **1.使用npm install config命令下载模块**
	- **2.在项目的根目录 下新建config文件夹**
	- **3.在config文件夹 下面新建defal.json. developmentjson. production.json文件**
	- **4.在项目中通过require方法, 将模块进行导入**
	- **5.使用模块内部提供的get方法获取配置信息**
	- 将敏感配置信息存储在环境变量中

		- 1. 在config文件夹中建立custom- environment variables.json文件
		- 2.配置项属性的值填写系统环境变 量的名字
		- 3.项目运行时config模块查找系统环境变量， 并读取其值作为当前配置项属于的值

### 博客前台页面

- 对首页内容截取输出

	- **{{@$value.content.replace(/<[^>]+>/g,'').substr(0,150)+"..."}}**

### **文章评论**

- 1.创建评论集合
- 2.判断用户是否登录，如果用户登录，再允许用户提交评论表单
- 3.在服务器端创建文章评论功能对应的路由
- 4.在路由请求处理函数中接收客户端传递过来的评论信息
- 5.将评论信息存储在评论集合中
- 6.将页面重定向回文章详情页面
- 7.在文章详情页面路由中获取文章评论信息并展示在页面中
