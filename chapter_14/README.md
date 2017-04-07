# 14.2 浏览器定位和导航
## 14.2.1 解析URL
- 例14-2：提取URL的搜索字符串中的参数
  ```javascript
  /*
   * 这个函数用来解析来自URL的查询串中的name=value参数对 
   * 它将name=value对存储在一个对象的属性中，并返回该对象 
   * 这样来使用它 
   * 
   * var args = urlArgs(); // 从URL中解析参数 
   * var q = args.q || ""; // 如果参数定义了的话就使用参数；否则使用一个默认值 
   * var n = args.n ? parseInt(args.n) : 10; 
   */
  function urlArgs() {
    var args = {}; // 定义一个空对象    
    var query = location.search.substring(1); // 查找到查询串，并去掉'? '    
    var pairs = query.split("&"); // 根据"&"符号将查询字符串分隔开    
    for (var i = 0; i < pairs.length; i++) { // 对于每个片段        
      var pos = pairs[i].indexOf('='); // 查找"name=value"        
      if (pos == -1) continue; // 如果没有找到的话，就跳过        
      var name = pairs[i].substring(0, pos); // 提取name        
      var value = pairs[i].substring(pos + 1); // 提取value        
      value = decodeURIComponent(value); // 对value进行解码        
      args[name] = value; // 存储为属性    
    }    
    return args; // 返回解析后的参数
  }
  ```

# 14.7 作为 Window对象属性的文档元素
- 如果在HTML文档中用id属性来为元素命名，并且如果Window对象没有此名字的属性，Window对象会赋予一个属性，它的名字是id属性的值，而它们的值指向表示文档元素的HTMLElement对象。