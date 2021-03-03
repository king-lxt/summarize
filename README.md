

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

