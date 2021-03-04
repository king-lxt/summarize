

### 1、For in 和 for of的区别

```
简单说  for in是获取属性名，for of获取属性值。

for in 的特点

1、for in 循环返回的是对象的键名(key)，返回数组的下标

2、遍历原型上的值和手动添加的值

for of 的特点

1、for of可以break、continue、return，可以随时退出循环

2、for of  适合遍历 iterator 接口的数据结构
```

### 2、箭头函数与普通函数的区别

```
1、语法简洁
2、箭头函数没有 prototype(原型), 所以箭头函数本身没有this
3、call | apply | bind 无法改变箭头函数中this的指向
4、箭头函数不会创建自己的this
5、箭头函数不能作为构造函数使用

```

### 3、typeof 和 instanceof 的区别

```
基本类型（简单类型、原始类型）：String、Number、Boolean、Null、Undefined、Symbol
引用类型（复杂类型）：Object（对象、Function、Array）
1、typeof对于原始类型来说，除了null都可以显示正确类型
2、typeof对于对象来说，除了函数都会显示object
3、instanceof 只能用来判断两个对象是否属于实例关系， 而不能判断一个对象实例具体属于哪种类型。
总结：
1、typeof能够检测出了null之外的原型类型（String、Number、Boolean、Undefined），对于对象类型能判断出function、其他的都为Object
   对象类型有（String、Number、Boolean、 Array、Date、RegExp、Math、 Error、 Object、Function、 Global）
2、如果需要判断一个值是否为null，最直接就是与null比较

```

### 4、js 判断数据类型的4中方法

```
1、typeof 能够检测出了null之外的原型类型（String、Number、Boolean、Undefined），对于对象类型能判断出function、其他的都为Object
    对象类型有（String、Number、Boolean、 Array、Date、RegExp、Math、 Error、 Object、Function、 Global）
2、instanceof用来判断A是否为B的实例，内部机制是通过判断对象的原型链中是否有类型的原型。
3、constructor
   当一个函数F被定义时，JS引擎会为 F 添加prototype原型，然后在prototype上添加一个constructor属性，并让其指向 F 的引用，F 利用原型对象的constructor属性引用了自身，当 F 作为构造函数创建对象时，原型上的constructor属性被遗传到了新创建的对象上，从原型链角度讲，构造函数F就是新对象的类型。这样做的意义是，让对象诞生以后，就具有可追溯的数据类型。
  eg:
    ''.constructor === String  // true
    new Function.constructor === Function // true
    new Number(1).constructor === Number  // true
    new Date().constructor === Date // true
    new Error().constructor === Error // true
    [].constructor === Array // true
  注意： 
  a、undefined、 null 为无效对象，不会有constructor 
  b、函数的 constructor 是不稳定的，这个主要体现在自定义对象上，当开发者重写 prototype 后，原有的 constructor 引用会丢失，constructor 会默认为 Object
  
  4、Object.prototype.toString()
  toString()是Object的原型方法，调用该方法，默认返回当前对象的[[Class]]。这是一个内部属性，其格式为[object Xxx],其中Xxx就是对象的类型。
     对于Object对象，直接调用toString()就能返回[object Object],而对于其他对象，则需要通过call、apply来调用才能返回正确的类型信息。
console.log(Object.prototype.toString.call(123));             //[object Number]
console.log(Object.prototype.toString.call('123'));           //[object String]
console.log(Object.prototype.toString.call(undefined));       //[object Undefined]
console.log(Object.prototype.toString.call(true));            //[object Boolean]
console.log(Object.prototype.toString.call({}));              //[object Object]
console.log(Object.prototype.toString.call([]));              //[object Array]
console.log(Object.prototype.toString.call(function(){}));    //[object Function]
console.log(Object.prototype.toString.call(null));            //[[object Null]]
   
```
### 5、0.1 + 0.2 !== 0.3

```
我们在对浮点数进行运算的过程中，需要将十进制转换成二进制。十进制小数转为二进制的规则如下：

对小数点以后的数乘以2，取结果的整数部分（不是1就是0），然后再用小数部分再乘以2，
再取结果的整数部分……以此类推，直到小数部分为0或者位数已经够了就OK了。然后把取的整数部分按先后次序排列。

根据上面的规则，最后 0.1 的表示如下：
0.000110011001100110011（0011无限循环）……

所以说，精度丢失并不是语言的问题，而是浮点数存储本身固有的缺陷。
```

### 6、为什么 JavaScript 是单线程

```
因为这是 JavaScript语言的一大特点就是单线程，同一个时间只能做一件事。那么，为什么JavaScript不可以有多个线程呢？这样可以提高效率啊。
JavaScript的单线程，与它的用途有关。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？
所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变。
```

### 7、为什么 JavaScript 是单线程

```
1、map【常用】: 遍历数组，返回回调返回值组成的新数组


2、forEach【常用】: 无法break，可以用try/catch中throw new Error来停止


3、filter【常用】: 过滤


4、some: 有一项返回true，则整体为true


5、every: 有一项返回false，则整体为false


6、join【常用】: 通过指定连接符生成字符串


7、push / pop: 末尾推入和弹出，改变原数组， push 返回数组长度, pop 返回原数组最后一项；


8、unshift / shift: 头部推入和弹出，改变原数组，unshift 返回数组长度，shift 返回原数组第一项 ；


9、sort(fn) / reverse【常用】: 排序与反转，改变原数组


10、concat【常用】: 连接数组，不影响原数组， 浅拷贝


11、slice(start, end): 返回截断后的新数组，不改变原数组


12、splice(start, number, value...)【常用】: 返回删除元素组成的数组，value 为插入项，改变原数组


13、indexOf / lastIndexOf(value, fromIndex): 查找数组项，返回对应的下标


14、reduce / reduceRight(fn(prev, cur)， defaultPrev): 两两执行，prev 为上次化简函数的return值，cur 为当前值
    当传入 defaultPrev 时，从第一项开始；
		当未传入时，则为第二项

```

### 8、浏览器输入 url 之后发生了什么

```
1、通过器请求解析域名所对应的 IP 地址;
2、TCP连接（三次握手）;
3、浏览RL 中域名后面部分对应的文件)的HTTP 请求，该请求报文作为 TCP 三次握手的第三个报文的数据发送给服务器;
4、浏览响应，并把对应的 html 文本发送给浏览器;
5、释放TCP连接（四次挥手）;
6、浏览器显示html页面
```

### 9、什么是BFC

```
即块级格式化上下文，它是一块独立的渲染区域

BFC的布局规则：
1、内部的Box会在垂直方向上一个接一个的排列
2、同一个BFC的两个相邻（上下、左右）的元素margin会发生重叠，同时会取margin的最大值做重叠部分。
3、BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。
4、BFC的不会与float box重叠。
5、计算BFC的高度时，浮动元素也参与计算。

触发BFC：
1、body根元素或其它包含它的元素
2、浮动元素 (元素的 float 不是 none)
3、绝对定位元素 (元素具有 position 为 absolute 或 fixed)
4、内联块 (元素具有 display: inline-block)
5、具有overflow 且值不是 visible 的块元素（hidden/auto/scroll)
除了兼容性问题，用display: flow-root来触发BFC最佳,不会产生任何副作用

```

###