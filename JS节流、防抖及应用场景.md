  ### 				JS节流、防抖及应用场景
#### 概念和例子
##### 函数防抖(debounce)

```
在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。
```
举个栗子：
```
//模拟一段ajax
function ajax(content){
  console.log('ajax request' + content)
}
let inp = document.querySelector('input')
inp.addEventListener('keyup',function(e){
  ajax(e.target.value)
})
```
每次键盘按下，就会触发ajax，造成资源浪费

优化之后
```
//模拟一段ajax
function ajax(content){
  console.log('ajax request' + content)
}
function debounce(fun,delay){
  return function(args){
    let that = this;
    let _args = args;
    clearTimeout(fun.id);
    fun.id = setTimeout(function(){
      fun.call(that,_arfs)
    },delay)
  }
}
let inp = document.querySelector('input');
let debounceAjax = debounce(ajax,500);
inp.addEventlistener('keyup',function(e){
  debounceAjax(e.target.value)
})
```
加入防抖之后，频繁输入不会发送请求，只有在指定时间间隔内没有输入时，才会执行函数。如果停止输入但在指定间隔内输入，会重新触发记时
#####函数节流(throttle)
```
规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效。
```
栗子：
```
function throttle(fun, delay) {
    let last, deferTimer
    return function (args) {
      let that = this
      let _args = arguments
      let now = +new Date()
      if (last && now < last + delay) {
        clearTimeout(deferTimer)
        deferTimer = setTimeout(function () {
            last = now
            fun.apply(that, _args)
        }, delay)
      }else {
        last = now
        fun.apply(that,_args)
      }
    }
}

let throttleAjax = throttle(ajax, 1000)
let inp = document.querySelector('input')
inp.addEventListener('keyup', function(e) {
    throttleAjax(e.target.value)
})
```