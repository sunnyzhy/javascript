# Promise 状态

Promise 的三种状态：
- ```pending```: Promise 的初始状态，不会触发 then 和 catch 回调函数
- ```fulfilled```: 成功状态，执行 resolve 的时候会将 Promise 的状态置为 fullfield，并会触发后续的 then 回调函数
- ```rejected```: 失败状态执行 reject 的时候会将 Promise 的状态置为 rejected，并会触发后续的 catch 回调函数

Promise 的状态变化：
- ```pending –> resolved``` 成功
- ```pending –> rejected``` 失败
- 变化不可逆

示例：

- ```pending```:
    ```javascript
    const p = new Promise(function(resolve, reject) { });
    console.log(p);
    ```
  
    输出：
    
    ```
    Promise {<pending>}
    ```
- ```fulfilled```:
    ```javascript
    const p = new Promise((resolve, reject) => {
        resolve('p');
    });
    console.log(p);
    p.then(v => console.log(v));
    ```
  
    输出：
    
    ```
    Promise {<fulfilled>: 'p'}
    p
    ```
- ```rejected```:
    ```javascript
    const p = new Promise((resolve, reject) => {
        reject('p');
    });
    console.log(p);
    p.catch(v => console.log(v));
    ```
  
    输出：
    
    ```
    Promise {<rejected>: 'p'}
    p
    ```
