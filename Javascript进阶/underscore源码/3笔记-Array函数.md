> 这一篇到了源码488行，主要是`underscore`对原生Array.prototype的一些扩展，arguments`参数`对象将在所有数组函数中工作 。然而, `_` 函数的设计并不只是针对稀疏（"sparse"）数组的。在下面的阅读学习中，我们会慢慢了解到

## 数组函数 488 - 747行

### first

`first_.first(array, [n])` 返回array（数组）的第一个元素。传递 n参数将返回数组中从第一个元素开始的n个元素（返回数组中前 n 个元素）

```js
_.first = _.head = _.take = function(array, n, guard) {
  if (array == null || array.length < 1) return void 0;
  if (n == null || guard) return array[0];
  return _.initial(array, array.length - n);
};
```

该方法可接收3个参数，函数里进行了判断和安全处理，我们来看看：

    if (array == null || array.length < 1) return void 0;

> Javascript中void是一个操作符，该操作符指定要计算一个表达式但是不返回值。

这是类型处理，所以这里的void 0，是返回了同一个空 空间，（可以理解为多个返回对内存的依赖更少了，从而节省了性能开销）

