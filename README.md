### For in 和 for of的区别

简单说  for in是获取属性名，for of获取属性值。

for in 的特点

1、for in 循环返回的是对象的键名(key)，返回数组的下标

2、遍历原型上的值和手动添加的值

for of 的特点

1、for of可以break、continue、return，可以随时退出循环

2、for of  适合遍历 iterator 接口的数据结构