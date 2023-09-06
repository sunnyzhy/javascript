# async/await

## 前言

async 函数，也就是我们常说的 async/await，是在 ES2017(ES8) 引入的新特性，主要目的是为了简化使用基于 Promise 的 API 时所需的语法。async 和 await 关键字让我们可以用一种更简洁的方式写出基于 Promise 的异步行为，而无需链式调用 Promise。

## 详解

async 表示函数里有异步操作，await 表示紧跟在后面的表达式需要等待结果。需要注意的是 await 关键字只在 async 函数内有效，如果在 async 函数体之外使用它，会抛出语法错误。

### async

async 函数返回一个 Promise 对象，可以使用 then 方法添加回调函数。只要使用 async，不管函数内部返回的是不是 Promise 对象，都会被包装成 Promise 对象。

#### 返回非 Promise 对象

```javascript
async function f1() {
    return 100;
}

let v = f1();
console.log(v);
```

输出：

```
Promise {<fulfilled>: 100}
```

```javascript
async function f1() {

}

let v = f1();
console.log(v);
```

输出：

```
Promise {<fulfilled>: undefined}
```

- async 函数有返回值的时候，返回的是 Promise 对象，PromiseResult 结果为 100
- async 函数没有返回值的时候，返回的也是 Promise 对象，PromiseResult 结果为 undefined

#### 返回 Promise 对象

```javascript
async function f1() {
    return new Promise((resolve, reject) => {
        resolve(100);
    });
}

let v = f1();
console.log(v);
```

输出：

```
Promise {<pending>}
  [[Prototype]]:Promise
  [[PromiseState]]:"fulfilled"
  [[PromiseResult]]:100
```

### await

await 关键字可以跟在任意变量或者表达式之前，但通常 await 后面会跟一个异步过程。await 使用时，会阻塞后续代码执行。await 只能在 async 标识的函数内使用。

```javascript
function f1() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('complete');
            resolve(100);
        }, 3000);
    });
}

async function f2() {
    console.log('begin');
    let v = await f1();
    console.log('v', v);
    console.log('end');
}

f2();
```

输出：

```
begin
complete
v 100
end
```

### async、await 结合使用

```javascript
function f1() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('f1');
            resolve(100);
        }, 1000);
    });
}

function f2() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('f2');
            resolve(200);
        }, 2000);
    });
}

async function f3() {
    console.log('begin');
    let v1 = await f1();
    console.log('v1', v1);
    let v2 = await f2();
    console.log('v2', v2);
    console.log('end');
}

f3();
```

输出：

```
begin
f1
v1 100
f2
v2 200
end
```
