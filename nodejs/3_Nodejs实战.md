# 一、安装nodejs

## 1. 下载nodejs的tar.xz文件
## 2. 命令行解压后，修改文件名
>(1) 首先：xz -d ***.tar.xz 解压得到tar文件

>(2) 其次：tar -xvf  ***.tar

>(3) 改名：

## 3. 环境变量设置
>(1) 打开/etc/profile，在末尾添加以下三行

>(2) export NODE_HOME=/opt/node

>(3) export PATH=$PATH:$NODE_HOME/bin

>(4) export NODE_PATH=$NODE_HOME/lib/node_modules

>(5) 注意：若非root，root要生效，则另需vim /root/.bashrc,并在文件末尾加入一行source etc/profile命令

# 二、开始一个新的Node项目

## 1. 创建文件夹，并初始化：
```plain
mkdir my_module
cd my_module
npm init -y  //-y表示yes，不用默认值可以去掉-y
```
## 2. 创建模块
### 2.1 什么是模块？
>模块可是一个人文件或者包含多个文件的目录(常会在目录中以index.js文件为模块入口，默认值可重写)
### 2.2 模块查找顺序？
>核心模块——>当前目录——>node_modules
### 3. 模块的引入require
>require是同步的，I/O密集最好别用，因为同步会阻塞Node
```plain
const req = require('./lib/...')  
//()内写引入模块路径，require函数返回模块中exports对象内容
```
## 3. 用module.exports微调模块的创建
### 3.1 调用模块返回多个函数/变量：exports.func/ji即可
```plain

```
### 3.2 调用模块只返回一个函数——>关于exports和module.exports
```plain
exports = Currency  //错误，Node不允许重写exports
//可改写为 module.exports = exports = Currency
module.exports = Currency  //正确，
//exports、module.exports同时存在，前者被忽略，后者被返回。
//exports只是对module.exports的一个全局引用，被定义为可添加属性的空对象
//exports.myFunc只是module.exports.myFunc的简写，因此exports重新设定
//，会打破module.exports与exports之间的引用关系
```
## 4. 用node_modules重用模块：
模块查找步骤

![图片](https://github.com/huzi0007/language/blob/main/pictures/nodejs1.png?raw=true)


## 5. 注意事项
### 5.1 模块是目录
![图片](https://github.com/huzi0007/language/blob/main/pictures/nodejs2.png?raw=true)
![图片](https://github.com/huzi0007/language/blob/main/pictures/nodejs3.png?raw=true)

### 5.2 Node可以把模块对象缓存起来：
>若两文件引入相同模块，第一个require会把模块返回的数据存到内存中，第二个require不用再去访问和计算源文件了
## 6. 使用异步编程技术
>Node的两种响应逻辑管理方式：回调和事件监听
### 6.1 回调：定义一次性响应逻辑。如何用回调处理一次性事件？
>6.1.1 要限制回调嵌套层级，避免混乱。还可减少if/else引起的嵌套，尽早从函数返回

>6.1.2 Node大多内置模块使用回调，会带2个参数，一个检测错误，一个存放结果
### 6.2 事件监听：
>跟事件相关联的、当有事件出现时就会被触发的回调函数。如何用事件监听器响应重复性事件？

>6.2.1 事件发射器示例;
>>echo服务器是一个处理重复性事件的简单例子。

>>a. 创建echo_server.js并启动(node echo_server.js) 

![图片](https://github.com/huzi0007/language/blob/main/pictures/nodejs4.png?raw=true)
>>b.连接及响应测试

```plain
telnet 127.0.0.1 8888
//终止连接
ctrl+]  //自动显示 telnet>
quit
```
![图片](https://github.com/huzi0007/language/blob/main/pictures/nodejs5.png?raw=true)
>6.2.2 响应只发生一次的事件：once方法
![图片](https://github.com/huzi0007/language/blob/main/pictures/nodejs6.png?raw=true)
>6.2.3 创建事件发射器：一个PUB/SUB的例子l
>>a. 创建发射器并监听
```plain
//下面是一个channel发射器，带一个监听器，可向加入频道的的人做出响应，
//这里用on添加监听(也可用addListener)
const EventEmitter = require('events').EventEmitter;
const channel = new EventEmitter();
channel.on('join',()=>{
  console.log('welcome! ');
})
channel.emit('join');
//事件名称，事件可是任意字符串值的键：data、json、其他事件名。
//仅err事件是特殊的
```
>>b. 使用监听器：EventEmitter实现发布/预定逻辑，做一个通信通道
![图片](https://github.com/huzi0007/language/blob/main/pictures/nodejs7.png?raw=true)

>6.2.4 异步编程的几个难点
