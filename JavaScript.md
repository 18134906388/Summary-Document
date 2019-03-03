# JavaScript中的一些问题

### 原型以及原型链
#### prototype
首先来介绍下 prototype 属性。这是一个显式原型属性，只有函数才拥有该属性。基本上所有函数都有这个属性，但是也有一个例外 ```
let fun = Function.prototype.bind()
``` 如果你以上述方法创建一个函数，那么可以发现这个函数是不具有 prototype 属性的。
#### prototype 如何产生的？
当我们声明一个函数时，这个属性就被自动创建了。```
function Foo() {}
``` 并且这个属性的值是一个对象（也就是原型），只有一个属性 constructor。
![函数prototype](https://github.com/18134906388/Summary-Document/blob/master/image/prototype.png?raw=true)
