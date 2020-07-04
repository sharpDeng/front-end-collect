# JavaScript中数据的转换

## 数据类型转换结果表


| 原始值 | 转为字符串 | 数字 | 布尔值 | 对象 |
| ------| -------- | ---- | ----- | ---- |
| undefind <br/> null | "undefined" <br> "null" | NaN <br> 0 | false <br> false | throws TypeError <br> TypeError
| true <br> false | "true" <br> "false" | 1 <br> 0 | | new Boolean(true) <br/> new Boolean(false)
| ""(空字符串) <br/> "1.2"(非空，数字) <br/> "one"(非空，非数字) | | 0 <br> 1.2 <br> NaN | false <br/> true <br/> true | new String('') <br> new String('1.2') <br> new String('one')
| 0 <br> -0 <br> NaN <br> Infinity <br> -Infinity <br> 1 | "0" <br> "0" <br> "NaN" <br> "Infinity" <br> "-Infinity" <br> "1" | | false <br> false <br> false <br> true <br> true <br> true | new Number(0) <br> new Number(-0) <br> new Number(NaN) <br> new Number(Infinity) <br> new Number(-Infinity) <br> new Number(1)
| {} （任意对象） <br> <span>[]（任意对象）</span> <br> [9]（1个数字元素） <br> ['a']（其他数组）<br> funtion(){} (任意函数) | 参考1.3 <br> "" <br> "9" <br> 使用join() <br> 参考1.5 | 参考2.1 <br> 0 <br> 9 <br> NaN <br> NaN | 都为true | |

## 详解

js 数据转换也分为隐形转换和显性转换两个，常用的显性转换有三种，装换为 字符串、数字和布尔值，下面进行展开说明；
而对于转化为对象,后续再补充
  
### 转换目标  **字符串**

显性方法有 `String(target)`、 `target.toString()` 、 `new String().valueOf()`(基本不会用)  
隐形方法有 `target + ''`

#### 1. String()

```JavaScript
// 1.1.1 数值, 结果同上表
    String(1) ==> "1"
    String(NaN) ==> "NaN"

// 1.1.2 布尔值 结果同上表

// 1.1.3 普通对象 转换过程见 注解 对象转换字符串和数字
    String({})  ==> "[object Object]"
    String({ a: 1, b: []}) ==> "[object Object]"
    // 特殊
    String({toString: function() { return 'hello'}}) ==> 'hello'
    String({valueOf: function(){return 'hello'}, toString: null})  ==> 'hello'
    String({toString: function() {}}) ==> 'undefined'

// 1.1.4 数组, 结果同调用 join()
    String([]) ==> ""
    String([9,2]) ==> "9,2"
    String([{a: 1}]) ==> "[object Object]"

// 1.1.5 函数, 将函数代码转化字符串
    String(function() { f(x) })  ==> "function() { f(x) }"

// 1.1.6 正则表达式
    String(/^abc/g) ==> "/^abc/g"
    String(/^abc/) ==> "/^abc/"

// 1.1.7 时间
    String(new Date()) ==> "Sat Jul 04 2019 11:56:23 GMT+0800 (China Standard Time)"

// 1.1.8 null 和 undefined
    String(null)  ==> "null"
    String(undefined) ==> "undefined"
```

#### 2. toSring() 函数  

toStirng函数是各自数据类型的原型链上的方法;  
结果基本和 `String()` 一致, 不同点有：

* 调用方式不一样，`toString` 是作为值的一个方法调用；
* `null` 和 `undefined` 不具备 `toString` 函数

```javascript

// 1.2.1数值 数字不可以直接调用，否则会报 SyntaxError: Invalid or unexpected token
// 结果同上表
    (1).toString()  ==>  "1"
    NaN.toString()  ==>  "NaN"

// 1.2.2布尔值，同上表

// 1.2.3 普通对象 对象不能直接调用， 否则 {} 会被看着是一个 代码块
// 1.2.4 可以使用 Object.prototype.toString.call() 进行数据类型的判断
    ({}).toString() ==> "[object Object]"
    ({ a: 1}).toString() ==> "[object Object]"

// 1.2.5 数组 相当于 join()
    [].toString() ==> ""
    [1].toString() ==> "1"
    [{}].toString() ==> "[object Object]"

// 1.2.6 函数 结果同 String()

// 1.2.7 null 和 undefined 不具备 toString函数
    null.toString()  ==> // VM7370:1 Uncaught TypeError: Cannot read property 'toString' of null

```

#### 3. 隐形转换 `target + ‘’`

```javascript
// 1.3.1 数值 结果同上表
    1 + ''  ==> ''
    0 + ''  ==> ''
    NaN + '' ==> "NaN"

// 1.3.2 布尔值 结果同上表
    false + "" ==> "false"

// 1.3.3 普通对象
    {} + "" ==> 0
    { a: 1 } + "" ==> 0

// 1.3.4 数组 结果同 join()
    [] + "" ==> ""
    ['9'] + "" ==> "9"

// 1.3.5其他同 String()
```

#### 4. 不常用 `new String().valueOf()`, 结果基本同 `String()`

### 转换目标为 **数字**

#### 1. Number()

```javascript
// 2.1.1 字符串
// 数字类字符串直接装换为对应的数字，其他转换为NaN
    Number('123') ==> 123
    Number('234df') ==> NaN
    Number('-0') ==> -0

// 2.1.2 布尔值 ,同上表
    Number(false) ==> 0
    Number(true) ==> 1

// 2.1.3 普通对象 具体过程见 注解：对象转基本类型的过程
    Number({}) ==> NaN
    Number({valueOf: function(){ return 123}}) ==> 123

// 2.1.4 数组
    Number([]) ==> 0
    Number(['4']) ==> 4
    Number([7]) ==> 7
    // 其他为NaN

// 2.1.5 函数 ==> NaN

// 2.1.6 日期
   Number(new Date(2019,1,1)) ==> 1548950400000

// 2.1.7 正则 ==> NaN

// 2.1.8 null 和 undefined
    Number(null) ==> 0
    Number(undefind) ==> NaN
```

#### 2. 符号运算

* 使用 `-` 运算符 `target - 0` 或 `target - ''`

结果基本同 `Number`, 处理普通对象

```javascript
// 对象
    {} - 0 ==> -0
    { a: 1} - 0 ==> -0
```

* `+String` 其他类型先转字符串后再进行计算

```javascript

 +'123' ==> 123
 +[] ==> 0
 +{} ==> NaN
```

#### `parseInt()` 和 `parseFloat()`

1. `parseInt(string, radix)`  
    第一个参数为字符串，如果不是字符串则隐形转换为字符串再进行计算

    ```javascript
    // 数字字符串取整返回
    parseInt('123') ==> 123
    parseInt('123.99') ==> 123

    // 数字 + 其他的字符串
    parseInt('123abc') ==> 123

    // 字符串 + 数字/字符串 的字符串
    parseInt('abc123') ==> NaN
    parseInt('abc') ==> NaN

    ```

2. `parseFloat(string)`

    ```javascript
    // 数字字符串去到小数返回
    parseFloat('123.34') ==> 123.34
    parseFloat('-123.34') ==> -123.34
    parseFloat('123.00') ==> 123

    // 数字 + 其他字符串
    parseFloat('123abc') ==> 123
    parseFloat('123.34abc') ==> 123

    // 其他字符串开头的
    parseFloat('abc') ==> NaN
    parseFloat('abc123') ==> NaN
    parseFloat('-abc') ==> NaN
    ```

### 转换目标为 **布尔值**

* 转换方式有下来几种

    * Boolean()
    * !!target
* 转换结果，除了以下列出的为`false`,其他均为`true`
    * `""` (空字符串)
    * 0
    * NaN
    * null
    * undefined
    * -0

注解：  

1. 对象转字符串
    * 如果对象的`toString` 是一个方法,则调用这个方法, 方法放回一个原始值，将这个原始值转换为字符串后，返回这个字符串结果
    * 如果 `toString` 不是一个方法，或者返回值不是一个原始值，就会调用`valueOf`方法，这个方法返回一个原始值，则见这个原始值转换为字符串返回
    * 上面两个方法都没有或者返回值都不是原始值，则会抛出一个类型错误；

2. 对象转数字，和上面的基本一致，只是先调用`valueOf` 方法，没有得到结果，再调用`toString()`方法，如果没有或调用两个方法都没有得到结果，则抛出一个类型错误
