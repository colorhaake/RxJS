### `Rx.Observable.amb(...args)` [&#x24C8;](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/amb.js "View in source")

Propagates the observable sequence or Promise that reacts first. "amb" stands for [ambiguous](http://blogs.msdn.com/b/jeffva/archive/2009/11/18/amb-materialize-and-dematerialize.aspx).
參數裡面的Observable或者Promises們，誰先送event，amb就送他的Observable。
注意：amb只會送一次而已，之後就永遠是那個Observable。

#### Arguments
1. `args` *(Array|arguments)*: Observable sources or Promises competing to react first either as an array or arguments.
Observable或者Promises都可以，或者你把他包成array傳進去也行。

#### Returns
*(`Observable`)*: An observable sequence that surfaces any of the given sequences, whichever reacted first.
回傳先送event的Observable。

#### Example
```js
/* Using Observable sequences */
var source = Rx.Observable.amb(
    Rx.Observable.timer(500).select(function () { return 'foo'; }),
    Rx.Observable.timer(200).select(function () { return 'bar'; })
);
第二個參數他跑的比較快，所以amb就送他的Observable出去。

var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x);
    },
    function (err) {
        console.log('Error: ' + err);
    },
    function () {
        console.log('Completed');
    });

// => Next: bar
// => Completed
所以這邊的log才會印出bar

/* Using Promises and Observables */
var source = Rx.Observable.amb(
    RSVP.Promise.resolve('foo'),
    Rx.Observable.timer(200).select(function () { return 'bar'; })
);

var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x);
    },
    function (err) {
        console.log('Error: ' + err);
    },
    function () {
        console.log('Completed');
    });

// => Next: foo
// => Completed
```

### Location

File:
- [`/src/core/linq/observable/amb.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/amb.js)

Dist:
- [`rx.all.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.all.js)
- [`rx.all.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.all.compat.js)
- [`rx.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.js)
- [`rx.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.compat.js)

Prerequisites:
- None

NPM Packages:
- [`rx`](https://www.npmjs.org/package/rx)

NuGet Packages:
- [`RxJS-Complete`](http://www.nuget.org/packages/RxJS-Complete/)
- [`RxJS-Main`](http://www.nuget.org/packages/RxJS-Main/)

Unit Tests:
- [`/tests/observable/ambproto.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/tests/observable/ambproto.js)
