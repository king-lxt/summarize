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

