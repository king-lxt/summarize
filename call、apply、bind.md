### call 和 Applay 的共同点

```
它们的共同点是，都能够改变函数执行时的上下文，将一个对象的方法交给另一个对象来执行，并且是立即执行的。

```

### call 和 Applay 的区别

call

```
1、调用 call 的对象，必须是个函数 Function。
2、call 的第一个参数，是一个对象。 Function 的调用者，将会指向这个对象。如果不传，则默认为全局对象 window。
3、第二个参数开始，可以接收任意个参数。每个参数会映射到相应位置的 Function 的参数上。但是如果将所有的参数作为数组传入，它们会作为一个整体映射到 Function 对应的第一个参数上，之后参数都为空。
```

apply

```
1、它的调用者必须是函数 Function，并且只接收两个参数，第一个参数的规则与 call 一致。
2、第二个参数，必须是数组或者类数组，它们会被转换成类数组，传入 Function 中，并且会被映射到 Function 对应的参数上。这也是 call 和 apply 之间，很重要的一个区别。
```

## call 和 apply 的用途

```
function superClass () {
    this.a = 1;
    this.print = function () {
        console.log(this.a);
    }
}

function subClass () {
    superClass.call(this);
    this.print();
}

subClass();
// 1
```

#### apply 的一些妙用

```
1、用它来获取数组中最大的一项。
let max = Math.max.apply(null, array);

2、实现两个数组合并。

let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];

Array.prototype.push.apply(arr1, arr2);
console.log(arr1); // [1, 2, 3, 4, 5, 6]
```

## bind 的使用

最后来说说 bind。在 MDN 上的解释是：bind() 方法创建一个新的函数，在调用时设置 this 关键字为提供的值。并在调用新函数时，将给定参数列表作为原函数的参数序列的前若干项

```
function add (a, b) {
    return a + b;
}

function sub (a, b) {
    return a - b;
}

add.bind(sub, 5, 3); // 这时，并不会返回 8
add.bind(sub, 5, 3)(); // 调用后，返回 8
```





Call的实现

```
var obj = {
    value: '1'
}

function fn(name, age) {
    this.name = name;
    this.age = age;
    this.say = function() {
        alert(this.name + this.age)
    }
}
// ========================================================================

Function.prototype.call2 = function(context) {
    var context = context || window;
    context.fn = this;
    var args = [];
    var result;
    for (var i = 1; i < arguments.length; i++) {
        args.push(`arguments[${i}]`)
    }
    result = eval(`context.fn(${args})`)
    delete context.fn;
    return result;
}

或者

Function.prototype.call = function (context, ...args) {
  context = context || window;
  
  const fnSymbol = Symbol("fn");
  context[fnSymbol] = this;
  
  context[fnSymbol](...args);
  delete context[fnSymbol];
}

// ========================================================================

fn.call2(obj, 'biao', 20);
obj.say()
```

Apply

```
var obj = {
    value: '1'
}

function fn(name, age) {
    this.name = name;
    this.age = age;
    this.say = function() {
        alert(this.name + this.age)
    }
}

// ========================================================================
Function.prototype.apply2 = function(context,arr){
    var context = Object(context) || window;
    context.fn = this;
    var args = [];
    var result;
    if(!arr){
        result = context.fn()
    }else{
        for(var i=0;i<arr.length;i++){
            args.push(`arr[${i}]`)
        }
        result = eval(`context.fn(${args})`)
    }
    delete context.fn;
    return result;
}

或者

Function.prototype.apply = function (context, argsArr) {
  context = context || window;
  
  const fnSymbol = Symbol("fn");
  context[fnSymbol] = this;
  
  context[fnSymbol](...argsArr);
  delete context[fnSymbol];
}

// ========================================================================

fn.apply2(obj, ['biao', 20])
obj.say()
```



bind



```

Function.prototype.bind = function (context, ...args) {
  context = context || window;
  const fnSymbol = Symbol("fn");
  context[fnSymbol] = this;
  
  return function (..._args) {
    args = args.concat(_args);
    
    context[fnSymbol](...args);
    delete context[fnSymbol];   
  }
}
 
```

