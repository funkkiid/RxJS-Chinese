### fromPromise

- 语法:

```ts
public static fromPromise(promise: Promise<T>, scheduler: Scheduler): Observable<T>
```

- 功能: 
转化一个Promise为一个Obseervable

将ES2015 Promise转换为Observable。 如果Promise为成功状态，则Observable会将成功的值作为next发出，然后complate。 如果Promise被失败，则输出Observable发出相应的错误。

eg:

```js
//convert the Promise returned by Fetch to an Observable

var result = Rx.Observable.fromPromise(fetch('http://myserver.com/'));
result.subscribe(x => console.log(x), e => console.error(e));
```

f-eg:

```js
// Create a promise which resolves 42
var promise = new RSVP.Promise(function (resolve, reject) {
    resolve(42);
});

var source1 = Rx.Observable.fromPromise(promise);

var subscription1 = source1.subscribe(
    function (x) {
        console.log('Next: ' + x);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: 42
// => Completed
```

```js
// Create a promise which rejects with an error
var promise = new RSVP.Promise(function (resolve, reject) {
    reject(new Error('reason'));
});

var source1 = Rx.Observable.fromPromise(promise);

var subscription1 = source1.subscribe(
    function (x) {
        console.log('Next: ' + x);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Error: Error: reason
```   