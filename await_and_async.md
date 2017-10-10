## await/async 使用简介 (2017.10.10)

1. `async and await`
```
// async函数返回一个 Promise 对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，
等到异步操作完成，再接着执行函数体内后面的语句。
function sleep(ms: number) {
    return new Promise(resolve => {
        setTimeout(resolve, ms)
    })
}
 
async function asyncPrint(value: string, ms: number) {
    console.log("before sleep", new Date())
 
    // 等价表达式1
    await sleep(ms)
    console.log("after sleep", new Date())
}
 
async function asyncPrint2(value: string, ms: number) {
    console.log("before sleep", new Date())
 
    // 等价表达式2
    sleep(ms).then(() => {
        console.log("after sleep", new Date())
    })
}
asyncPrint("hello, world", 5000)
asyncPrint2("hello, world", 5000)
 
/* 结果：
H:\workspace\base\build>node app.js
before sleep 2017-03-07T07:15:34.961Z
before sleep 2017-03-07T07:15:34.961Z
after sleep 2017-03-07T07:15:39.983Z
after sleep 2017-03-07T07:15:39.983Z
*/
```
2. `async函数有多种使用形式`
```
 // 函数声明
 async function foo() { }
 
 // 函数表达式
 const foo = async function () { }
 
 // 对象的方法
 let obj = { async foo() { } }
 obj.foo().then(...)
 
 // Class 的方法
 class Storage {
     private cachePromise: any
     constructor() {
         this.cachePromise = caches.open('avatars')
     }
 
     async getAvatar(name: string) {
         const cache = await this.cachePromise
         return cache.match(`/avatars/${name}.jpg`)
     }
 }
 
 const storage = new Storage()
 storage.getAvatar('jake').then(...)
 
 // 箭头函数
 const foo = async () => { }
```
3. `async函数返回一个Promise对象`
```
async function f() {
    return 'hello world'
}
 
f().then(v => console.log(v))       // "hello world"
// console.log(await f())           // 等价上式
 
// async函数内部抛出错误，会导致返回的Promise对象变为reject状态。
抛出的错误对象会被catch方法回调函数接收到
async function f2() {
    throw new Error('出错了')
}
 
f2().then(
    v => console.log("result:", v),
    e => console.log("error:", e)
)

/* 结果：
error: Error: 出错了
at H:\workspace\base\build\app.js:91:23
at Generator.next (<anonymous>)
at H:\workspace\base\build\app.js:7:71
*/

```