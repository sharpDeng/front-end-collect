# 防抖函数 （debounce）

## 简介

防抖函数: 在连续触发的事件中只执行一次函数; 

## 用途

对于只关心**最后状态**的连续事件中，为了减少函数执行次数，提高应用性能，可以使用防抖函数对目标函数进行封装

## 实现

```JavaScript

/**
 * @param {function} func 目标函数
 * @param {time} wait 定义多久没执行算一次连续操作的结束， 毫秒
 * @param {Boolean} immediate 函数是否立即执行
*/


function debounce(fnc, wait, immediate = false) {
    let timer = null，

    return function() {
        let context = this,
            args = arguments
        if(immediate){
            immediate = false
            func.apply(this, args)
        }
        if(timer){
            clearTimeout(timer)
            timer = null
        }else {
            timer = setTimeout(function(){ // 在周期内db函数被触发会更新定时器，延迟事件函数的执行
                timer = null
                func.apply(context, args)
            }, wait);
        }
    }
}
```
