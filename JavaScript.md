# JavaScript中的一些问题

## 原型以及原型链
### prototype
首先来介绍下 prototype 属性。这是一个显式原型属性，只有函数才拥有该属性。基本上所有函数都有这个属性，但是也有一个例外 `let fun = Function.prototype.bind()` 如果你以上述方法创建一个函数，那么可以发现这个函数是不具有 prototype 属性的。
### prototype 如何产生的？
当我们声明一个函数时，这个属性就被自动创建了。`function Foo() {}` ,并且这个属性的值是一个对象（也就是原型），只有一个属性 constructor 对应着构造函数，也就是 Foo。<br>
![函数prototype](https://github.com/18134906388/Summary-Document/blob/master/image/prototype.png?raw=true)
### constructor
constructor 是一个公有且不可枚举的属性。一旦我们改变了函数的 prototype ，那么新对象就没有这个属性了。原型的 constructor 属性指向构造函数，构造函数又通过 prototype 属性指回原型，但是并不是所有函数都具有这个属性，Function.prototype.bind() 就没有这个属性。<br>
![constructor](https://github.com/18134906388/Summary-Document/blob/master/image/constructor.jpg?raw=true "constructor")

那么你肯定也有一个疑问，这个属性到底有什么用呢？其实这个属性可以说是一个历史遗留问题，在大部分情况下是没用的，在我的理解里，我认为他有两个作用：
- 让实例对象知道是什么函数构造了它
- 如果想给某些类库中的构造函数增加一些自定义的方法，就可以通过 xx.constructor.method 来扩展

### _proto\_
这是每个对象都有的隐式原型属性，指向了创建该对象的构造函数的原型。其实这个属性指向了 [[prototype]]，但是 [[prototype]] 是内部属性，我们并不能访问到，所以使用 _proto_ 来访问。<br>
![_proto_](https://github.com/18134906388/Summary-Document/blob/master/image/_proto_1.png?raw=true "_proto_")

因为在 JS 中是没有类的概念的，为了实现类似继承的方式，通过 _proto_ 将对象和原型联系起来组成原型链，得以让对象可以访问到不属于自己的属性。

### 实例对象的 _proto\_ 如何产生的
从上图可知，当我们使用 new 操作符时，生成的实例对象拥有了 _proto_属性。所以可以说，在 new 的过程中，新对象被添加了 _proto_ 并且链接到构造函数的原型上。在调用 new 的过程中会发生四件事情
1. 新生成了一个对象
2. 链接到原型
3. 绑定 this
4. 返回新对象

### Function.proto === Function.prototype
对于对象来说，xx.__proto__.contrcutor 是该对象的构造函数，但是在图中我们可以发现 Function.__proto__ === Function.prototype，难道这代表着 Function 自己产生了自己?答案肯定是否认的，要说明这个问题我们先从 Object 说起。

从图中我们可以发现，所有对象都可以通过原型链最终找到 Object.prototype ，虽然 Object.prototype 也是一个对象，但是这个对象却不是 Object 创造的，而是引擎自己创建了 Object.prototype 。所以可以这样说，所有实例都是对象，但是对象不一定都是实例。

接下来我们来看 Function.prototype 这个特殊的对象，如果你在浏览器将这个对象打印出来，会发现这个对象其实是一个函数。

`Function.prototype//ƒ () { [native code] }`

我们知道函数都是通过 new Function() 生成的，难道 Function.prototype 也是通过 new Function() 产生的吗？答案也是否定的，这个函数也是引擎自己创建的。首先引擎创建了 Object.prototype ，然后创建了 Function.prototype ，并且通过 __proto__ 将两者联系了起来。这里也很好的解释了上面的一个问题，为什么 let fun = Function.prototype.bind() 没有 prototype 属性。因为 Function.prototype 是引擎创建出来的对象，引擎认为不需要给这个对象添加 prototype 属性。
有了 Function.prototype 以后才有了 function Function() ，然后其他的构造函数都是 function Function() 生成的。

现在可以来解释 Function.__proto__ === Function.prototype 这个问题了。因为先有的 Function.prototype 以后才有的 function Function() ，所以也就不存在鸡生蛋蛋生鸡的悖论问题了。对于为什么 Function.__proto__ 会等于 Function.prototype ，个人的理解是：其他所有的构造函数都可以通过原型链找到 Function.prototype ，并且 function Function() 本质也是一个函数，为了不产生混乱就将 function Function() 的 __proto__ 联系到了 Function.prototype 上。

### 总结
![原型以及原型链](https://user-gold-cdn.xitu.io/2018/11/16/1671d387e4189ec8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
1. Object 是所有对象的爸爸，所有对象都可以通过 __proto__ 找到它
2. Function 是所有函数的爸爸，所有函数都可以通过 __proto__ 找到它
3. Function.prototype 和 Object.prototype 是两个特殊的对象，他们由引擎来创建
4. 除了以上两个特殊对象，其他对象都是通过构造器 new 出来的
5. 函数的 prototype 是一个对象，也就是原型
6. 对象的 __proto__ 指向原型， __proto__ 将对象和原型连接起来组成了原型链

## 浅拷贝和深拷贝
