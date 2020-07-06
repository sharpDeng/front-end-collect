# 节流函数

## 简介

节流是指 连续触发的事件在n秒内只执行一次函数, 作用在于减少函数的次数，让函数以一定的频率执行。

## 用途

根据定义即可得知，主要是使用在一些连续触发的事件中，用于避免用户快速操作，函数执行频率过高，影响程序性能；
连续触发的时间大概有下面几种：

1. mousemove
2. touchmove
3. 键盘监听事件
4. window.onresize
5. window.onscroll
...

## 实现

```javascript

function throttle(func, wait, imm = false){
    let timer = null,
        args = []

    return function() {
        args = arguments
        if(imm) {
            imm = false
            func.apply(this, args)
        }
        if(!timer){
            timer = setTimeout(() => {
                timer = null
                func.apply(this, args)
            }, wait)
        }
    }
}

```
