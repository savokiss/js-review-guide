# 9.2 类和构造函数
- 实际上 instanceof 运算符并不会检查 r 是否是由 Range() 构造函数初始化而来，而会检查 r 是否继承自 Range.prototype
  ```javascript
    r instanceof Range // 如果r继承自Range.prototype，则返回true
  ```
- 每个JavaScript 函数（例外见下一条）都自动拥有一个 prototype 属性。这个属性的值是一个对象，这个对象包含唯一一个不可枚举属性 constructor。constructor 属性的值是一个函数对象：
  ```javascript
    var F = function() {}; // 这是一个函数对象
    var p = F.prototype;   // 这是F相关联的原型对象
    var c = p.constructor; // 这是与原型相关联的函数
    c === F                // => true: 对于任意函数 F.prototype.constructor === F
  ```
- 只有 ECMAScript 5 中的 Function.bind() 方法返回的函数不具有 prototype 属性 ( prototype 为 undefined )
- 可以看到构造函数的原型中存在预先定义好的 constructor 属性，这意味着对象通常继承的 constructor 均指代它们的构造函数。由于构造函数是类的“公共标识”，因此这个constructor 属性为对象提供了类。
  ```javascript
  var o = new F(); //创建类F的一个对象
  o.constructor === F // => true，constructor属性指代这个类
  ```

# 9.6 JavaScriopt 中的面向对象技术
## 9.6.3 标准转换方法
- 如果一个对象有toJSON()方法，JSON.stringify()并不会对传入的对象做序列化操作，而会调用toJSON()来执行序列化操作（序列化的值可能是原始值也可能是对象）。
- 如果将对象用于JavaScript的关系比较运算符，比如“<”和“<=”，JavaScript会首先调用对象的valueOf()方法，如果这个方法返回一个原始值，则直接比较原始值。

# 9.7 子类
## 9.7.4 类的层次结构和抽象类
- 多态：不管是在经典的面向对象编程语言中还是在JavaScript中，通行的解决办法是“从实现中抽离出接口”。假定定义了一个AbstractSet类，其中定义了一些辅助方法比如toString()，但并没有实现诸如foreach()的核心方法。这样，实现的Set、SingletonSet和FilteredSet都是这个抽象类的子类，FilteredSet和SingletonSet都不必再实现为某个不相关的类的子类了。

# 9.8 ECMAScript 5 中的类
## 9.8.2 定义不可变的类
- 例9-18：创建一个不可变的类，它的属性和方法都是只读的
```javascript
// 这个方法可以使用new调用，也可以省略new，它可以用做构造函数也可以用做工厂函数
function Range(from, to) {
    // 这些是对from和to只读属性的描述符
    var props = {
        from: {value: from, enumerable: true, writable: false, configurable: false},
        to: { value: to, enumerable: true, writable: false, configurable: false}
    };
    if (this instanceof Range) // 如果作为构造函数来调用
        Object.defineProperties(this, props); // 定义属性
    else // 否则，作为工厂方法来调用
        return Object.create(Range.prototype, // 创建并返回这个新Range对象，
            props); // 属性由props指定
}
```