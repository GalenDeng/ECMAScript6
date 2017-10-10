## promise 简介 （2017.10.10）
1. `Promise特性`
```
1. 对象的状态不受外界影响。Promise 对象代表一个异步操作，有三种状态：Pending（进行中）、Resolved（已完成，又称 Fulfilled）和 Rejected（已失败）。
只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。

2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise 对象的状态改变，只有两种可能：从 Pending 变为 Resolved 和从 Pending 变为 Rejected。
只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。

有了 Promise 对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise 对象提供统一的接口，使得控制异步操作更加容易。
```
2.`Promise构造函数的参数的特点`
```
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。
```
```
var promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```
```
resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，
作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，
并将异步操作报出的错误，作为参数传递出去。
```

3.`promise的使用方式`
```
const promise = new Promise((resolve, reject) => {
    if (/* 异步操作成功 */)
        return resolve(value)   // 成功，调用resolve函数把状态从pending变成resolved
    reject(error)   // 失败，调用reject函数把状态从pending变成rejected
})
 
promise.then(value => {     // value为上面resolve(value)中的参数
    // success
}).catch(error => {               // error为上面reject(error)中的参数
    // failure
})
/*  等价表达式
promise.then(value => {     // value为上面resolve(value)中的参数
    // success
}, error => {               // error为上面reject(error)中的参数
    // failure
})
*/
```
4.`最常用的例子，休眠` 
```
function sleep(ms: number) {
    console.log("run sleep")    // Promise新建后就会立即执行
    return new Promise(resolve => setTimeout(resolve, ms))
}
sleep(1000).then(() => {
    console.log("wakeup")
})
console.log("sleep")
```
``` 
result:
 run sleep
 sleep
 wakeup
```
5. `catch用于捕获promise中的异常，包括Error和reject返回的结果。如果不捕获，程序将会异常退出`
```
let throwError = () => new Promise((resolve, reject) => {
    reject(new Error("test"))   // 同 throw new Error("test")
})
throwError()
    .then(() => console.log("ok"))
    .catch(err => console.log(err))

```
6.`typescript/ES6+`
```
在typescript/ES6+，Promise主要用于把异步回调变为同步的async/await写法，上述的基础够用了
```
7.`理解promise` 
```
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('resolved.');
});

console.log('Hi!');

// Promise
// Hi!
// resolved
上面代码中，Promise 新建后立即执行，所以首先输出的是Promise。然后，then方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，所以resolved最后输出。
```
