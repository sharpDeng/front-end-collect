# JavaScript中数据的转换

## 字符串转数值

```javascript
 // 前期准备
 let strNum = '123',
     str = 'hello',
     res = 0
```

| 原始值 | 转换目标 | 结果 |
| :-:|:-:|:-:|
| number | 布尔值 | ’123‘ => true
* `Number`函数，例子：
    * `Number(strNum)` 输出 `123`
    * `Number(str)` 输出 `NAN`

* 乘于1， `'123' * 1`
    * `res = strNum * 1` 输出 `res -> 123`

