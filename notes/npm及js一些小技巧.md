
### 查看npm上各种包的版本等信息

查看所有版本
> npm view <packagename> versions 
查看最近版本的详细信息
> npm view <packagename> 

npm安装最新版本的bao
> npm install <packagename>@latest

npm升级包的版本
> npm update <packagename>
> npm update <packagename>@2.5  // 指定升级到某个版本

npm删除某个包
> npm remove <packagename>

> `npm init` 命令用来初始化一个简单的 `package.json` 文件

依赖包安装
> npm install <packagename>
> 依赖管理是 `npm` 的核心功能，原理就是执行 `npm install` 从 `package.json` 中的 `dependencies, devDependencies` 将依赖包安装到当前目录的 `./node_modules` 文件夹中


`CSS` 的伪元素是个很強大的东西，我们可以利用他做很多运用，通常为了做一些效果，`content:" "` 多半会留空，但其实可以在里面写上 `attr` 抓资料哦

```js
<div data-msg="Open this file in Github Desktop">  
hover
</div>

div{
  width:100px;
  border:1px solid red;  
  position:relative;
}
div:hover:after{
  content:attr(data-msg);
  position:absolute;
  font-size: 12px;
  width:200%;
  line-height:30px;
  text-align:center;
  left:0;
  top:25px;
  border:1px solid green;
}
```

最终效果如图：
![图片](https://i.loli.net/2019/04/06/5ca8625e475dc.png)

### 单行写一个评级组件
> "★★★★★☆☆☆☆☆".slice(5 - rate, 10 - rate);定义一个变量rate是1到5的值

效果如图：
![评论](https://i.loli.net/2019/04/06/5ca863ab0a5f7.png)

### JavaScript 错误处理的方式的正确姿势
```js
try {
    something
} catch (e) {
    window.location.href =
        "http://stackoverflow.com/search?q=[js]+" +
        e.message;
}
```
> 报错会自动跳到stackoverflow.com上去搜素

### 一行代码找到网页上所有的标签并标注不同颜色
```js
[].forEach.call($$("*"),function(a){
    a.style.outline="1px solid #"+(~~(Math.random()*(1<<24))).toString(16)
})
```

### 优雅得取随机字符串
```js
Math.random().toString(16).substring(2)
// "aa2ce73b56d44"
Math.random().toString(36).substring(2) 
// "l6pdgttxk3o"
```

### 多种写法的匿名函数自执行
```js
(function(){})();
~ function() {}();
! function() {}();
+ function() {}();
- function() {}();
```

### js中优雅的取整
```js
let a = ~~2.5 // 2
let b = 2.35 >> 0 // 2
// 相当于parseInt(2.5) -- // 2 
```

### 优雅的实现金钱格式化：1234567890 --> 1,234,567,890
```js
/*正则方式*/
var test1 = '1234567890'
var format = test1.replace(/\B(?=(\d{3})+(?!\d))/g, ',')

console.log(format) // 1,234,567,890

/*非正则方式*/
 function formatCash(str) {
       return str.split('').reverse().reduce((prev, next, index) => {
            return ((index % 3) ? next : (next + ',')) + prev
       })
}
console.log(formatCash('1234567890')) // 1,234,567,890
```

### 实现标准JSON的深拷贝
```js
var a = {
    a: 1,
    b: { c: 1, d: 2 }
}
var obj=JSON.parse(JSON.stringify(a))
```
### 用js的方法下载图片
```js
function downloadImg(url) { 
  fetch(url).then(res =>
    res.blob().then(blob => {
      const a = document.createElement('a')
      const url = window.URL.createObjectURL(blob)
      a.href = url
      a.download = '哈哈'
      a.click()
      window.URL.revokeObjectURL(url)
    })
  )
}
```

### js中sort排序的正序和倒序
```js
/**
* sort() 方法用于对数组的元素进行排序
* arrayObject.sort(sortby)
* sortby 可选。规定排序顺序。必须是函数。
*/

const arr=[3,1,16,34,30];
arr.sort((m,n)=>m-n) // [1, 3, 16, 30, 34]
arr.sort((m,n)=>n-m) // [34, 30, 16, 3, 1]
```

### 用最短的代码实现一个长度为m(6)且值都n(8)的数组
```js
Array(6).fill(8)  // [8,8,8,8,8,8]
```
### 一句代码找出当前页面所有的图片
```js
let arr = []
Array.from(document.images).forEach(item=>{
	arr.push(item.getAttribute('src'))
})
console.log(JSON.stringify(arr))
```

### 手机上的多行省略
```css
.overflow-hidden{
    display: box !important;
    display: -webkit-box !important;
    overflow: hidden;
    text-overflow: ellipsis;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 4;/*第几行出现省略号*/
    /*text-align:justify;不能和溢出隐藏的代码一起写，会有bug*/
}

```

### 使用Boolean过滤数组中的所有假值
我们知道JS中有一些假值：false，null，0，""，undefined，NaN，怎样把数组中的假值快速过滤呢，可以使用Boolean构造函数来进行一次转换
```js
const compact = arr => arr.filter(Boolean)
compact([0, 1, false, 2, '', 3, 'a', 'e' * 23, NaN, 's', 34])   // [1, 2, 3, "a", "s", 34]
```

### 双位运算符 ~~
可以使用双位操作符来替代正数的 `Math.floor( )`，替代负数的`Math.ceil( )`。双否定位操作符的优势在于它执行相同的操作运行速度更快。
```js
~~4.5                // 4
Math.floor(4.5)      // 4
Math.ceil(4.5)       // 5

~~-4.5                // -4
Math.floor(-4.5)     // -5
Math.ceil(-4.5)      // -4
```

### 取整 | 0
对一个数字`| 0`可以取整，负数也同样适用，`num | 0`
```js
1.3 | 0         // 1
-1.9 | 0        // -1
```
