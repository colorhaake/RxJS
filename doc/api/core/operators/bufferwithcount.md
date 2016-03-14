### `Rx.Observable.prototype.bufferWithCount(count, [skip])`
[&#x24C8;](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/bufferwithcount.js)

Projects each element of an observable sequence into zero or more buffers which are produced based on element count information.

用count來當作buffer的數量大小，當buffer滿了之後，就會送出所有在buffer裡的event，清空buffer。

#### Arguments
1. `count` *(`Function`)*: Length of each buffer.
2. `[skip]` *(`Function`)*: Number of elements to skip between creation of consecutive buffers. If not provided, defaults to the count.

count是buffer的數量大小
skip是當buffer滿了之後，要清空buffer時，清掉buffer裡面的數量。預設值是count，也就是說當buffer滿了之後，把buffer裡面count數量的值給清光。
舉例來說，count為3，skip為1，buffer裡面的值假設是[2, 3, 4]，此時buffer已經滿了，他會把buffer裡面skip，也就是1的數量清空，清完的buffer剩下為[3, 4]。

#### Returns
*(`Observable`)*: An observable sequence of buffers.

#### Example
```js
/* Without a skip */
var source = Rx.Observable.range(1, 6)
    .bufferWithCount(2);

var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x.toString());
    },
    function (err) {
        console.log('Error: ' + err);
    },
    function () {
        console.log('Completed');
    });

// => Next: 1,2
// => Next: 3,4
// => Next: 5,6
// => Completed

/* Using a skip */
var source = Rx.Observable.range(1, 6)
    .bufferWithCount(2, 1);

var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x.toString());
    },
    function (err) {
        console.log('Error: ' + err);
    },
    function () {
        console.log('Completed');
    });

// => Next: 1,2
// => Next: 2,3
// => Next: 3,4
// => Next: 4,5
// => Next: 5,6
// => Next: 6
// => Completed
```
### Location

File:
- [`/src/core/linq/observable/bufferwithcount.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/bufferwithcount.js)

Dist:
- [`rx.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.js)
- [`rx.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.compat.js)
- [`rx.all.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.all.js)
- [`rx.all.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.all.compat.js)
- [`rx.lite.extras.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.lite.extras.js)

Prerequisites:
- None

NPM Packages:
- [`rx`](https://www.npmjs.org/package/rx)

NuGet Packages:
- [`RxJS-All`](http://www.nuget.org/packages/RxJS-All/)
- [`RxJS-Main`](http://www.nuget.org/packages/RxJS-Main/)
- [`RxJS-Lite`](http://www.nuget.org/packages/RxJS-Lite/)

Unit Tests:
- [`/tests/observable/bufferwithcount.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/tests/observable/bufferwithcount.js)
