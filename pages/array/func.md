# 数组方法

<style>
  .note {
    padding: 16px 24px;
    font-size: 14px;
  }
  .promiry {
    background-color: #e1f5fe;
    color: #01579b;
  }
  .warn {
    background: #feefe3;
    color: #bf360c;
  }
  .icon {
    display: inline!import;
    margin: 0!important;
    box-shadow: none;
  }
</style>

## 会改表原数组的

### 添加数组元素（`push/unshift`）

```javascript
let arr = [1,2];

// 在数组最后添加一个元素或多个元素, 返回值是更新后数组的长度
arr.push(12)   // 1
console.log(arr)// [1,2,12]

arr.push(123,324,453,534) // 7
console.log(arr)// [1,2,12,123,324,453,534]

// 在数组头部插入一个或多个元素, 返回值： 新数组长度
arr.unshift(12)  // 3
console.log(arr) // [12,1,2]
arr.unshift(123,34,45) // 6
console.log(arr）// [123,34,45,12,1,2]
```

### 删除数组元素（`pop/shift`）

```javascript
let arr = [1,2,3,4,5];

// 删除数组的最后一个元素， 返回值：被删除的元素
arr.pop()  // 5
console.log(arr) // [1,2,3,4]

// 删除数组的第一个元素， 返回值：被删除的元素
arr.shift()  // 1
console.log(arr) // [2,3,4]

```

### 数组排序（reverse/sort）

```javascript
/**
 * 颠倒原数组元素，不需要传参
 * @return {Array} 元素颠倒后的原数组
 */
let arr = [1,2,3]
let arr1 = arr.reverse() // [3,2,1]
console.log(arr) // [3,2,1]
console.log(arr1 === arr) // true

/**
 * 依据传入的回调函数的返回值进行排序
 * @param {Function} sortby 其返回值return规定排序规则； return >= 0, 元素前后顺序不变，return < 0, 前后元素位置调换
 * @return {Array} 排序后的原数组
 */

let arr = [2,1,3,56,7,3,4]

arr.sort((cur, pre) => {
  return cur - pre
})  // [1, 2, 3, 3, 4, 7, 56]

arr.sort((cur, pre) => {
  return cur - pre
}) // [56, 7, 4, 3, 3, 2, 1]
```

### 添加/删除/替换 数组元素 （splice）

```javascript
/**
 * @param {Number} index 操作的起始位置，接收负数
 * @param {Number} howmany 要删除的项目数量, 不传默认到数组最后一个元素
 * @param {any} item 像数组中添加新项目
 * @return {Array} 包含被删除项目的新数组
 */
let arr = [1,2,3,4,5,6]

// 删除元素
arr.splice(-1,1) // [6]
console.log(arr) // [1,2,3,4,5]
arr.splice(3,0) // []
console.log(arr) // [1,2,3,4,5]
arr.splice(3,1) // [4]
console.log(arr) // [1,2,3,5]
arr.splice(1) // [2,3,5]
console.log(arr) //[1]


```

### 替换元素（copyWithin/fill）

```javascript
let arr = [1,2,3,4,5,6,7,8]
/**
 * Array.prototype.copyWithin(target, start = 0, end = this.length)
 * 将数组内start - end 位置的复制到从target开始的位置（会覆盖原成员）
 * @param {Number} target 开始替换数据的位置
 * @param {Number} start 读取数据开始的位置
 * @param {Number} end 读取数据的结束数据（不包含）
 * @return {Array} 改变后的原数组
 */
arr.copyWithin(1, 4) // [1, 5, 6, 7, 8, 6, 7, 8]
console.log(arr) // [1, 5, 6, 7, 8, 6, 7, 8]
arr = [1,2,3,4,5,6,7,8]
arr.copyWithin(1, -3, -1) // [1,6,7,4,5,6,7,8]

/**
 * ES6
 * Array.prototype.fill(target,start=0,end=this.length)
 * 替换数组元素，一般用于数组初始化使用
 * @param {Number} target 开始替换数据的位置
 * @param {Number} start 读取数据开始的位置
 * @param {Number} end 读取数据的结束数据（不包含）
 * @return {Array} 改变后的原数组
 */
let newArr = new Array(5).fill(1) // [1,1,1,1,1]

// 替换引用类型时注意，填充的是引用类型的引用地址，而不是拷贝
newArr = new Array(2).fill({name: '张三'})  // [{name: '张三'},{name: '张三'}]
newArr[0].name = "李四"
console.log(newArr)  // [{name: "李四"}, {name: "李四"}]
```

## 不会改变原数组

### 添加数组元素/合并数组 （concat）

```javascript
let arr = [1,2]
let arr1 = [3,4,5]
let arr2 = [6,7]
let newArr
/**
 * Array.prototype.concat()
 * 合并两个或多个数组, 不改变原数组
 * @param {any} 任意数据类型
 * @return {Array} 一个新数组
*/

newArr = arr.concat(arr1) // [1,2,3,4,5]
newArr = arr.concat(arr1,arr2) // [1,2,3,4,5,6,7]
newArr = arr.concat('a','b') // [1,2,'a','b']

console.log(arr)  // [1,2]
console.log(arr1) // [3,4,5]
console.log(arr2) // [6,7]
```

### 转换为字符串（join/toString）

```javascript
let arr = ['a','b',1,2]

/**
 * 将数组换行为字符串,数组不是字符串类型的会通过String() 先转换为字符串再拼接
 * @param {} 指定分隔符，默认是 逗号
 * @return {String} 元素和分隔符组成的字符串
*/
arr.join() // 'a,b,1,2'
arr.join('') // 'ab12'
[{a:1},1]   // "[object Object],1"
function fn (){}
[fn,1] // "function fn(){},1"

/**
 * 无传参, 和join()的结果是一致的
 * @return {String} 元素和逗号组成的字符串
 * */

arr.toString() // 'a,b,1,2'
```

### 获取数组元素（slice）

```javascript
/**
 * @param {Number} start 从何处开始截取，负数则从数组尾部数器， -1 表示最后一个，-2 倒数第二个  
 * @param {Number} end 截取到何处，默认到数组最后一个
 * @return {Array} 新数组，包含 start 到 end （不包含）的原数组的元素
 */
  let arr = [1,3,4,5,6]
  arr.slice(3,4) // [5]
  arr.slice(-2)  // [5,6]
```

### 查找元素位置信息（indexOf/lastIndexOf）

```javascript
let arr = ['am', '23', 123, '23', 'am']

// 从数组起始位置开始查找，返回第一个符合的元素的下标，没有返回-1
arr.indexOf('am') // 0
arr.indexOf(1)  // -1

// 从数组结束为止开始查找，返回第一个符合元素的下标，没有返回-1
arr.lastIndexOf('23') // 3
arr.lastIndexOf(23) // -1
```

### 检查数组元素（every/some/includes/find）

```javascript

let arr = [1,2,34,'5',6,7,8,90]

/**
 * 传入一个回调函数，数组的每个元素都会执行一次这个函数
 * @param {function} 出入函数， 函数的参数有：item-数组元素 index-当前元素下标 arr-数组本身
 * @return {Boolean} 所有函数的返回值为true， 则返回true, 只要有一个返回false,则返回false,同时函数不再执行
 */
arr.every((item,index, arr) => {
  return item > 0
})   // true

arr.every(item => {
  return typeof item === 'number'
}) // false

/**
 * Array.prototype.some
 * 传入一个函数，表现上基本和上述一致,返回值不一样
 * @return {Boolean} 如果有一个函数返回true, 则返回true, 同时结束执行，否则返回false
 */
arr.some(item => {
  return typeof item === 'string'
}) // true

arr.some(item => {
  item > 1000
}) // false

/**
 * ES6 新增
 * Array.prototype.includes(item)
 * @param {any} item 待检测元素
 * @return {Boolean} 数组包含item返回true,否则返回false
 */
arr = [1,2,NaN]
arr.include(1) // true
arr.include(NaN) // true
arr.include('1') // false
arr.include('hello') // false

/**
 * ES6新增
 * Array.prototype.find(fnc,thisValue)
 * 用法基本同 some() 只是返回值不一样, 这里返回符合要求的元素
 * @param {function} fnc 每次元素传入该函数，同时执行一次
 * @param {Object} thisValue 函数fnc执行时的this指向
 * @return {any} 执行fnc第一次返回true时的当前元素，都是false时，返回值为undefined
 */
arr = [1,2,3,4,5]
arr.find(function(item){
  return item > 3
}) // 4

arr.find(function(item){
  return item > this.max
},{max: 2}) // 3 其中 this 是传入的对象 {max: 2}

/**
 * ES6新增
 * Array.prototype.findIndex(fnc,thisValue)
 * 和find()基本一致，只是返回值不同
 * @return {any} 执行fnc第一次返回true时的当前元素索引/下标, 都是false 时，返回 -1
 */

arr.find(function(item){
  return item > this.max
},{max: 2}) // 2 其中 this 是传入的对象 {max: 2}

arr.find(function(item){
  return item > 5
}) // -1 
```

<div class="note primary">
indexOf 和 include 最大的区别是 indexOf内部使用的是 (===),所以无法判断 NaN, 而include 可以
</div>

### 迭代（forEach/map）

```javascript
let arr = [1,2]

/**
 * @param {function} 数组每个元素执行一次，函数参数 item-当前元素，index-单签元素下标
 * @return 无返回值
 */
let rt = arr.forEach((item,index,arr) => {
  console.log(item)
}) // 1  2

console.log(rt) // undefined

/**
 * Array.prototype.map
 * @param {function} 数组每个元素执行一次，函数参数 item-当前元素，index-单签元素下标
 * @return 一个回调函数的返回值组成的新数组
 */
arr.map(item => {
  return item + 2
}) // [3,4]
arr.map(function(){}) // [undefind,undefind]
```

### 过滤数组（filter）

```javascript
let arr = [1,2,3,45,5]
/**
 * @param {}
 */
arr.filter(item => {
  return item > 3
}) // [45,5]
```

### 迭代综合（reduce/reduceRight)

```javascript
let arr = [1,2,3,4]
/**
 * Array.prototype.reduce （从左到右）
 * @param {function} 其中函数参数total-初始值，或计算后的返回值 currenValue-当前元素 currentIndex-当前元素的索引，arr当前操作的数组
 * @param {any} 传给函数的初始值
 * @return {any} 最后一次执行函数的返回值
 */
arr.reduce((total, currentValue, currentIndex, arr) => {
  return total + currentValue
}) // 10

// 生成新数组
arr.reduce((total, currentValue) => {
  total.push(currentValue)
},[]) // [1,2,3,4]

/**
 * Array.prototype.reduceRight （从右到左）
 * 和reduce 基本一致
 */
arr.reduceRight((total, currentValue, currentIndex, arr) => {
  return total - currentValue
}) // -2
```

<!-- ### keys() 和 values()

```javascript
/**
 * Array.prototype.keys()
 * @return {Array} 返回数组索引组成的遍历
 */

let arr = ['a', 'b', 'c']
arr.keys() // [0,1,2]

/**
 * Array.prototype.values()
 * @return {Array} 返回数组元素组成的新数组
 */
``` -->

### 扁平化数组（flat/flatMap）

```javascript
/**
 * ES6新增
 * Array.prototype.flat(num=1)
 * @param {Number} num 需要张开的层数
 * @return {Array} 返回处理后的新数组，原数组不变
 */
let arr = [1, 2, [3, 4]]
arr.flat()  // [1,2,3,4]
arr = [1,2,[[3],4]]
arr.flat() // [1,2,[3],4]
arr.flat(2) // [1,2,3,4]

/**
 * ES6新增
 * Array.prototype.flatMap(func, thisValue)
 * @param {Number} num 需要张开的层数
 * @param {Object} thisValue func每次执行时的this指向
 * @return {Array} 返回数组索引组成的遍历
 */
arr = [1, 2, [3, 4]]
arr.flatMap(function(item) {
  return [item]
}) // [1,2,3,4]
// 相当于map和对flat集合
// 即先用map运行得到结果，然后结果调用 flat函数
```

### 类数组转数组 （from）

```javascript
/**
 * Array.prototype.form(arrayLike)
 * 改方法将带有length属性的对象或类数组装换为数组
 * @param {arrayLike} arrayLike 类数组
 * @return {Array} 数组
 */
Array.from({name: 1}) // []
Array.from({0: 1, 1: 123}) // []
Array.from({0: 1, 1: 123, length: 2}) // [1,123]
Array.from({a: 1, b: 123, length: 2}) // [undefined, undefined]

// 通常用于将函数的 arguments
```
