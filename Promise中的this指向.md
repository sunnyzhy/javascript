# Promise 中的 this 指向

## 回调函数是匿名函数

```javascript
class Demo {
    constructor() {
        this.name = 'this is a demo'; 
    }
    print() {
        this.p().then(function() {console.log(this);});
    }
    p() {
        return new Promise(function(resolve, reject) {
            console.log(this);
            resolve('p');
        });
    }
}

window.onload = function load() {
    let demo = new Demo();
    demo.print();
}
```

输出：

```
undefined
undefined
```

## 回调函数是 lambda 函数

```javascript
class Demo {
    constructor() {
        this.name = 'this is a demo'; 
    }
    print() {
        this.p().then(() => {console.log(this);});
    }
    p() {
        return new Promise((resolve, reject) => {
            console.log(this);
            resolve('p');
        });
    }
}

window.onload = function load() {
    let demo = new Demo();
    demo.print();
}
```

输出：

```
Demo {name: 'this is a demo'}
Demo {name: 'this is a demo'}
```

## 总结

- Promise 对象的回调函数是匿名函数时，this 指向 window。

- Promise 对象的回调函数是 lambda 函数时，this 指向本实例。
