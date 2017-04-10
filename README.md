## node  
> 自己学习的时候边学边写的基础知识

* node.js中回调函数格式是约定俗成的，它有两个参数，第一个参数为err，第二个参数为data，顾名思义，err是错误信息，data则是返回的数据
* os模块可提供操作系统的一些基本信息，它的一些常用方法如下：

```javascript
    var os = require("os");
    var result = os.platform(); //查看操作系统平台
    os.release(); //查看操作系统版本
    os.type(); //查看操作系统名称
    os.arch(); //查看操作系统CPU架构
    console.log(result);
```

* process是一个全局内置对象，可以在代码中的任何位置访问此对象，这个对象代表我们的node.js代码宿主的操作系统进程对象。
* 使用process对象可以截获进程的异常、退出等事件，也可以获取进程的当前目录、环境变量、内存占用等信息，还可以执行进程退出、工作目录切换等操作。
* 查看应用程序当前目录时，可以使用cwd函数，使用语法如下：
* Process.cwd();
* 改变应用程序目录，就要使用chdir函数了，它的用法如下：
* process.chdir("目录");
* stdout是标准输出流，它的作用就是将内容打印到输出设备上，console.log就是封装了它。process.stderr.write(输入内容);
* stderr是标准错误流，和stdout的作用差不多，不同的是它是用来打印错误信息的，我们可以通过它来捕获错误信息，基本使用方法如下：
* stdin是进程的输入流,我们可以通过注册事件的方式来获取输入的内容
* 程序内杀死进程，退出程序，可以使用exit函数，示例如下：

```javascript
    process.exit(code);
```

* 在我们的输入输出的内容中有中文的时候，可能会乱码的问题，这是因为编码不同造成的，所以在这种情况下需要为流设置编码

```javascript
    process.stdin.setEncoding(编码);
    process.stdout.setEncoding(编码);
    process.stderr.setEncoding(编码);
```
* fs模块提供writeFile函数，可以异步的将数据写入一个文件, 如果文件已经存在则会被替换。用法如下：fs.writeFile(filename, data, callback)
数据参数可以是string或者是Buffer,编码格式参数可选，默认为"utf8"，回调函数只有一个参数err。
fs模块中还有appendFile函数，它可以将新的内容追加到已有的文件中，如果文件不存在，则会创建一个新的文件。使用方法如下：fs.appendFile(文件名,数据,编码,回调函数(err));
* 检查一个文件是否存在：fs.exists(文件，回调函数(exists));
* 修改文件名称：fs.rename(旧文件，新文件，回调函数(err)）可以通过rename函数来达到移动文件的目的：
* 读取文件是最常用到的功能之一：fs.readFile(文件名, function (err, data) ）回调函数里面的data,就是读取的文件内容。
* 删除文件：fs.unlink(文件,回调函数(err));
* node.js中如何创建目录：fs.mkdir(路径，权限，回调函数(err));
权限：可选参数，只在linux下有效，表示目录的权限，默认为0777，表示文件所有者、文件所有者所在的组的用户、所有用户，都有权限进行读、写、执行的操作
* 删除目录：fs.rmdir(路径，回调函数(err));
* 读取目录下所有的文件：fs.readdir(目录,回调函数(err,files));
* fs模块不但提供异步的文件操作，还提供相应的同步操作方法，需要指出的是，nodejs采用异步I/O正是为了避免I/O时的等待时间，提高CPU的利用率，所以在选择使用异步或同步方法的时候需要权衡取舍。

## Url:
* parse函数的作用是解析url，返回一个json格式的数组

parse函数的第二个参数是布尔类型，当参数为true时，会将查询条件也解析成json格式的对象。也就是query的值是一个json的

* format函数的作用与parse相反，它的参数是一个JSON对象，返回一个组装好的url地址

* resolve函数的参数是两个路径，第一个路径是开始的路径或者说当前路径，第二个则是想要去往的路径，返回值是一个组装好的url
```javascript
    url.resolve('http://example.com/', '/one') // 'http://example.com/one'
```






## Express

* Express是一个简洁、灵活的node.js Web应用开发框架, 它提供一系列强大的功能，比如：模板解析、静态文件服务、中间件、路由控制等等,并且还可以使用插件或整合其他模块来帮助你创建各种 Web和移动设备应用,是目前最流行的基于Node.js的Web开发框架，并且支持Ejs、jade等多种模板，可以快速地搭建一个具有完整功能的网站。
* Express的常用方法 get方法 —— 根据请求路径来处理客户端发出的GET请求。app.get(path,function(request, response));path为请求的路径，第二个参数为处理请求的回调函数，有两个参数分别是request和response，代表请求信息和响应信息。
指定了path路径下页面的处理方法
* response.send('Welcome to the homepage!');向浏览器发送一个字符串
* 什么是中间件：中间件(middleware)就是处理HTTP请求的函数，用来完成各种特定的任务，比如检查用户是否登录、分析数据、以及其他在需要最终将数据发送给用户之前完成的任务。 它最大的特点就是，一个中间件处理完，可以把相应数据再传递给下一个中间件。？？？？？？？？？？？？？？
* 一个不进行任何操作、只传递request对象的中间件，大概是这样：
```javascript
    function Middleware(request, response, next) { 
    next();
```
* 和get函数不同app.all()函数可以匹配所有的HTTP动词，也就是说它可以过滤所有路径的请求，如果使用all函数定义中间件，那么就相当于所有请求都必须先通过此该中间件。app.all(path,function(request, response));
```javascript
    app.all("*", function(request, response, next) ）  //“*“表示对所有路径都使用
```
* use用法1是express调用中间件的方法，它返回一个函数
```javascript
    app.use([path], function(request, response, next){});//path为可选参数
    app.use(express.static(path.join(__dirname, '/')));//使用use函数调用express中间件设定了静态文件目录的访问路径(这里假设为根路径)。
```
* 回调函数的next参数，表示接受其他中间件的调用，函数体中的next()，表示将请求数据传递给下一个中间件。！！！！！！！！！
* 上面代码先调用第一个中间件，在控制台输出一行信息，然后通过next()，调用第二个中间件，输出HTTP回应。由于第二个中间件没有调用next方法，所以req对象就不再向后传递了。
* use用法2 use方法不仅可以调用中间件，还可以根据请求的网址，返回不同的网页内容
* 通过request.url属性，判断请求的网址，从而返回不同的内容。
* Express回调函数有两个参数，分别是request(简称req)和response(简称res)，request代表客户端发来的HTTP请求，response代表发向客户端的HTTP回应，这两个参数都是对象。
* query基本用法：query是一个可获取客户端get请求路径参数的对象属性，包含着被解析过的请求参数对象，默认为{}。通过req.query获取get请求路径的对象参数值   格式：req.query.参数名
Url后面的参数名也可以这样写： /shoes?order=desc&shoe[color]=blue&shoe[type]=converse
获取color: req.query.shoe.color
* param基本用法  通过req.param我们也可以获取被解析过的请求参数对象的值。req.param("属性名)
所谓“路由”，就是指为不同的访问路径，指定不同的处理方法。
* 和param相似，但params是一个可以解析包含着有复杂命名路由规则的请求对象的属性。
* send()方法向浏览器发送一个响应信息，并可以智能处理不同类型的数据
* 解析 json 方式的post请求数据  需要安装 body-parser

---------继续记录---------
