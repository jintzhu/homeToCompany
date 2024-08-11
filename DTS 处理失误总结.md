

# DTS 处理失误总结

出现问题的原因：

1. 没有充分理解可选链操作符
2. 修改检视意见后未进行验证确认

#### 什么是可选链操作符

可选链操作符( ?. )   允许读取位于连接对象链深处的属性的值，而不必明确验证链中的每个引用是否有效。

该操作符的功能类似于 `.` 链式操作符，不同之处在于，在引用为空 ([`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null) 或者 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)) 的情况下不会引起错误

```javascript
举个例子:
let result = someInterface.customMethod?.();
如果希望允许 someInterface 也为 null 或者 undefined ，需要像这样写
let result = someInterface?.customMethod?.();
```

```
短路计算：
let potentiallyNullObj = null;
let x = 0;
let prop = potentiallyNullObj?.[x++];

console.log(x); // x 将不会被递增，依旧输出 0
当在表达式中使用可选链时，如果左操作数是 null 或 undefined，表达式将不会被计算
```

#### 总结

单看使用可选链是没问题的，result.datas?.errorCode 是为了防止datas为null或undefined时报错

第一次传进的result.datas是

```json
{errorCode:'0',errorMsg:'操作成功’, ...}
```

第二次传进的result.datas是 undefined

造成弹窗错误的原因是如果使用可选链，在第二次传进时会走到else逻辑从而导致问题未修复

```javascript
let datas=undefined
if (datas?.errorCode === "0") {
  console.log("success");
} else {
  # datas为undefined会走到这里
  console.log("error");
}
```

## 总结

- 对每一个修改都要重视，保持对代码的敬畏。一个小小的？号就会导致整个逻辑发生变化。
- 每个检视意见都要充分验证才能进行修改。开发作为修改者永远是第一责任人。