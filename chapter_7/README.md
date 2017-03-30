# 7.11 类数组对象
```javascript
// 判定o是否是一个类数组对象
// 字符串和函数有length属性，但是它们
// 可以用typeof检测将其排除。在客户端JavaScript中，DOM文本节点
// 也有length属性，需要用额外判断o.nodeType != 3 将其排除
function isArrayLike(o) {
  if(o &&                                   // o 非 null、undefined等
    typeof o === 'object' &&                // o 是对象
    isFinite(o.length) &&                   // o.length 是有限数值
    o.length >= 0 &&                        // o.length 为非负值
    o.length ==== Math.floor(o.length) &&   // o.length　是整数
    o.length < 4294967296)                  // o.length < 2^32
    return true;                            // o 是类数组对象
  else {
    return false;
  }
}
```