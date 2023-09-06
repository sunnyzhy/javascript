# Promise 链式调用

## Promise 构造函数

Promise 对象是一个构造函数，用来生成 Promise 实例。

```javascript
const promise = new Promise(function (resolve, reject) {
  // ... some code 
  if (/* 异步操作成功 */) {
    resolve(value)
  } else {
    reject(error)
  }
})
```

- Promise 对象调用 reslove 才能触发之后的 then 回调函数
- Promise 对象调用 reject 才能触发之后的 catch 回调函数
- then 回调函数会返回一个新的 Promise 对象
- 待异步操作完成之后再调用 reslove，这样才能保证多个异步的顺序执行

## Promise.resolve

```javascript
let p1 = new Promise((resolve, reject) => {
    resolve(1);
});
p1.then(v => v + 1).then(v => console.log(v));
```

输出：

```
2
```

***Promise 对象调用 reslove 才能把值传递给之后的 then 回调函数。***

```javascript
let p1 = new Promise((resolve, reject) => {
    return 1;
});
p1.then(v => console.log(v));
```

***Promise 对象调用 return 不能把值传递给之后的 then 回调函数。***

## Promise.reject

```javascript
let p1 = new Promise((resolve, reject) => {
    reject(1);
});
p1.catch(v => console.log(v));
```

输出：

```
1
```

## Promise.race

```javascript
let p1 = new Promise((resolve, reject) => {
    const interval = setInterval(() => {
        clearInterval(interval);
            console.log('p1');
            resolve(1);
    }, 5000);
});
let p2 = new Promise((resolve, reject) => {
    const interval = setInterval(() => {
        clearInterval(interval);
            console.log('p2');
            resolve(2);
    }, 1000);
});
let p = Promise.race([p1, p2]);
p.then(v => console.log(v));
```

输出：

```
p2
2
p1
```

***只要p1，p2之中有一个实例率先改变状态，p 的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给 p 的回调函数。***

## Promise.all

```javascript
let p1 = new Promise((resolve, reject) => {
    const interval = setInterval(() => {
        clearInterval(interval);
            console.log('p1');
            resolve(10);
    }, 5000);
});
let p2 = new Promise((resolve, reject) => {
    const interval = setInterval(() => {
        clearInterval(interval);
            console.log('p2');
            resolve(20);
    }, 1000);
});
let p = Promise.all([p1, p2]);
p.then(v => {
    for(let i = 0; i < v.length; i++) {
        console.log(v[i]);
    }
});
```

输出：

```
p2
p1
10
20
```

- ***p1，p2 两个实例同时执行，如果全部成功执行，则以数组的方式返回所有 Promise 实例的执行结果。***
- ***只要有一个实例失败，p 的状态就变为 rejected，返回值将直接传递给 catch 回调函数。***

## 顺序调用

```javascript
class Demo {
    constructor() {
        this.name = 'this is a demo'; 
    }
    p1() {
        return new Promise((resolve, reject) => {
            this.print('p1', 1000, resolve);
            })
    }
    p2() {
        return new Promise((resolve, reject) => {
            this.print('p2', 3000, resolve);
            })
    }
    p3() {
        return new Promise((resolve, reject) => {
            this.print('p3', 6000, resolve);
            })
    }
    print(p, timeout, resolve) {
        console.log(p + ' begin');
        const arr = [1, 2, 3, 4, 5];
        let i = 0;
        const interval = setInterval(() => {
            console.log(p, arr[i]);
            i++;
            if (i >= arr.length) {
                clearInterval(interval);
                console.log(p + ' complete');
                resolve();
            }
        }, timeout);
        console.log(p + ' end');
    }
}

window.onload = function load() {
    let demo = new Demo();
    demo.p1().then(() => demo.p2()).then(() => demo.p3());
}
```

输出：

```
p1 begin
p1 end
p1 1
p1 2
p1 3
p1 4
p1 5
p1 complete
p2 begin
p2 end
p2 1
p2 2
p2 3
p2 4
p2 5
p2 complete
p3 begin
p3 end
p3 1
p3 2
p3 3
p3 4
p3 5
p3 complete
```

***待异步操作完成之后再调用 reslove，这样才能保证多个异步的顺序执行。***
