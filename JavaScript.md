# JavaScript中的一些问题

### 原型以及原型链
#### prototype
首先来介绍下 prototype 属性。这是一个显式原型属性，只有函数才拥有该属性。基本上所有函数都有这个属性，但是也有一个例外 ```let fun = Function.prototype.bind()``` 如果你以上述方法创建一个函数，那么可以发现这个函数是不具有 prototype 属性的。
#### prototype 如何产生的？
当我们声明一个函数时，这个属性就被自动创建了。```function Foo() {}``` 并且这个属性的值是一个对象（也就是原型），只有一个属性 constructor 对应着构造函数，也就是 Foo。
![函数prototype](https://github.com/18134906388/Summary-Document/blob/master/image/prototype.png?raw=true)
#### constructor
constructor 是一个公有且不可枚举的属性。一旦我们改变了函数的 prototype ，那么新对象就没有这个属性了。

那么你肯定也有一个疑问，这个属性到底有什么用呢？其实这个属性可以说是一个历史遗留问题，在大部分情况下是没用的，在我的理解里，我认为他有两个作用：

- 让实例对象知道是什么函数构造了它
- 如果想给某些类库中的构造函数增加一些自定义的方法，就可以通过 xx.constructor.method 来扩展
