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

