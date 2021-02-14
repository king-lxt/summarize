```
class Promise {
    constructor(executor) {
        this.state = 'pending'; // 状态
        this.value = undefined; // 成功的值；
        this.reason = undefined;

        // =================
        // 为了处理异步的执行的问题
        // 采用发布订阅者模式
        this.onResolvedCallBack = [];
        this.onrejectedCallBack = [];
        // =================

        const resolve = (value) => {
            if (this.state === 'pending') {
                this.state = 'fulfilled';
                this.value = value;
                // =================为了处理异步的执行的问题
                this.onResolvedCallBack.forEach((fn) => fn());
            }
        }

        const reject = (reason) => {
            if (this.state === 'pending') {
                this.state = 'rejected';
                this.reason = reason;
                // =================为了处理异步的执行的问题
                this.onResolvedCallBack.forEach((fn) => fn());
            }
        }

        executor(reject, resolve);
    }

    then(onFulfilled, onRejected) {
        if (this.state === 'fulfilled') {
            onFulfilled(this.value);
        }
        if (this.state === 'rejected') {
            onRejected(this.reason);
        }
        // =================为了处理异步的执行的问题
        if (this.state === 'pending') {
            this.onResolvedCallBack.push(() => {
                onFulfilled(this.value);
            });
            this.onResolvedCallBack.push(() => {
                onRejected(this.reason);
            });
        }
    }
}

```

 2、promise 里 resolve返回promise

```
class Promise {
    constructor(executor) {
        this.state = 'pending'; // 状态
        this.value = undefined; // 成功的值；
        this.reason = undefined;

        // =================
        // 为了处理异步的执行的问题
        // 采用发布订阅者模式
        this.onResolvedCallBack = [];
        this.onrejectedCallBack = [];
        // =================

        const resolve = (value) => {
            if (this.state === 'pending') {
                this.state = 'fulfilled';
                this.value = value;
                // =================为了处理异步的执行的问题
                this.onResolvedCallBack.forEach((fn) => fn());
            }
        }

        const reject = (reason) => {
            if (this.state === 'pending') {
                this.state = 'rejected';
                this.reason = reason;
                // =================为了处理异步的执行的问题
                this.onResolvedCallBack.forEach((fn) => fn());
            }
        }

        executor(reject, resolve);
    }

    then(onFulfilled, onRejected) {
        const promise2 = new Promise((resolve, reject) => {
            if (this.state === 'fulfilled') {
                setTimeout(() => {
                    // 因为promise2可能还没有，所以压进队列，下次执行
                    const promiseOrNot = onFulfilled(this.value); // 可能是一个promise或者其他类型
                    resolvePromise(promise2, promiseOrNot, resolve, reject);
                }, 0);
            }

            if (this.state === 'rejected') {
                setTimeout(() => {
                    const promiseOrNot = onRejected(this.reason);
                    resolvePromise(promise2, promiseOrNot, resolve, reject);
                }, 0);
            }

            // =================为了处理异步的执行的问题
            if (this.state === 'pending') {
                this.onResolvedCallBack.push(() => {
                    setTimeout(() => {
                        const promiseOrNot = onFulfilled(this.value);
                        resolvePromise(promise2, promiseOrNot, resolve, reject);
                    }, 0);
                });
                this.onResolvedCallBack.push(() => {
                    setTimeout(() => {
                        const promiseOrNot = onRejected(this.reason);
                        resolvePromise(promise2, promiseOrNot, resolve, reject);
                    }, 0);
                });
            }
        });
        return promise2;
    }
}



const resolvePromise = (promise2, promiseOrNot, resolve, reject) => {
    if (promise2 === promiseOrNot) {
        return reject(new TypeError('promise循环引用'));
    }

    // 判断 promiseOrNot 是不是promise
    if (typeof promiseOrNot === 'function' || (typeof promiseOrNot === 'object' && promiseOrNot !== null)) {
        try {
            const then = promiseOrNot.then;
            if (typeof then === 'function') {
                // 认为是Promise
                then.call(promiseOrNot, y => {
                     // 返回的有可能还是promise
                    resolvePromise(promise2, y, resolve, reject);
                }, r => {
                    nreject(r);
                });
            }
        } catch (error) {
            reject(error);
        }
    } else {
        resolve(promiseOrNot);
    }
}
```

