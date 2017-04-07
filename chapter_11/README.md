# 11.4 迭代
## 11.4.2 迭代器
```javascript
// 这个函数返回了一个迭代器，它可以迭代某个范围内的整数
function rangeIter(first, last) {
  let nextValue = Math.ceil(first);
  return {
    next: function() {
      if(nextValue > last) throw 'StopIteration';
      return nextValue++;
    }
  }
}
// 使用这个范围迭代器实现一次糟糕的迭代
let r = rangeIter(1, 5); // 获得迭代器对象
while (true) { // 在循环中使用它   
  try {        
    console.log(r.next()); // 调用next()方法    
  } catch (e) {        
     if (e == 'StopIteration') break; // 抛出StopIteration时退出循环        
     else throw e;    
  }
}
```

## 11.4.3 生成器
- 任何使用关键字yield的函数（哪怕yield在代码逻辑中是不可达的）都称为“生成器函数”（generator function）。生成器函数通过yield返回值。这些函数中可以使用return来终止函数的执行而不带任何返回值，但不能使用return来返回一个值。除了使用yield，对return的使用限制也使生成器函数更明显地区别于普通函数。
- 生成器是一个对象，用以表示生成器函数的当前执行状态。它定义了一个next()方法，后者可恢复生成器函数的执行，直到遇到下一条yield语句为止。这时，生成器函数中的yield语句的返回值就是生成器的next()方法的返回值。

  ```javascript
  // 一个用以产生一个Fibonacci数列的生成器函数
  function fibonacci() {    
    let x = 0, y = 1;    
    while (true) {        
      yield y;        
      [x, y] = [y, x + y];    
    }}
  // 调用生成器函数以获得一个生成器
  f = fibonacci();
  // 将生成器当做迭代器，输出Fibonacci数列的前10个数
  for (let i = 0; i < 10; i++) console.log(f.next());
  ```