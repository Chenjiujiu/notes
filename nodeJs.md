# nodeJs 
1. 包引入`const fs = require('fs')`
2. nodejs第三方包`nodemon`   
持续编译nodejs文件  
3. fs 自带包,文件管理  
```javascript
fs.readFile('./文件名',(err,data)=>{
  console.log(data.toString())
})
```
4. util自带包,工具  
until含有{promisify},可用于包装成promis 
```javascript
const {promisify} = require('until');
const readFile = promisify(fs.readFile);
readFile('./文件名').then().then()
```
# buffer  
```javascript
const buf= Buffer.alloc(10)//开辟10字节的内存空间,00,00,00,00,...10位
Buffer.from([1,2,3])//二进制赋值, 01,02,03,00,00,00,00
Buffer.from('这是一段文字')//二进制赋值,直接赋值
buf.write('hello')//写入
buf.concat([buf1,buf2])//链接buf1,buf2
buf.toString()//buffer转字符串
buf.toJSON()//buffer转json
```
# http 
1.引入
```javascript
const http = require('http');
const server = http.createServer((req,res)=>{
	const {url, method, headers} = req;
	/*文本*/
	res.writeHead(500,{'Content-Type':'text/plain'})
	res.end('resssss...')
    /*网页*/
    res.statusCode=200;
	res.setRequestHeader('Content-Type','text/html');
	res.end(data)
    /*JSON*/
    res.statusCode=200;
	res.setRequestHeader('Content-Type','application/json');
	res.end(JSON.stringify(data2))
    /*图片*/
	if(method === 'GET' && headers.accept.indexOf('image/*') !== -1){
		fs.createReadStream('.'+url).pipe(res)
	}
})
server.listen(3000)
```
# express 
npm i express
```javascript
const express = require('express');
const app = express();
app.get('/',(req,res)=>{
	res.end("hello")
})
app.get('/user',(req,res)=>{
	res.end(JSON.stringify({name:'damao'}))
})
app.listene(3000,()=>{
	console.log("开始监听吗")
})
```
# tcp 


