### `Rx.Observable.prototype.buffer()`
[&#x24C8;](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.coincidence.js#L572-L585 "View in source")

The `buffer` method periodically gathers items emitted by a source Observable into buffers, and emits these buffers as its own emissions.

Note that if the source Observable issues an `onError` notification, `buffer` will pass on this notification immediately without first emitting the buffer it is in the process of assembling, even if that buffer contains items that were emitted by the source Observable before it issued the error notification.

There are a number of ways with which you can regulate how `buffer` gathers items from the source Observable into buffers:

就是會buffer你傳進來的event，然後用bufferClosingSelector, bufferOpennings或者bufferBoundaries來決定什麼時候把buffer裡的event一起送出去，然後清空buffer，繼續buffer events。

#### With buffer closing selector
```js
Rx.Observable.prototype.buffer(bufferClosingSelector);
```

bufferClosingSelectr是個function，他會回傳一個Observable，當回傳的Observable送出event的時候，就是把buffer裡面event送出去，然後清空buffer的時候。

Returns an Observable that emits buffers of items it collects from the source `Observable`. The resulting `Observable` emits connected, non-overlapping buffers. It emits the current buffer and replaces it with a new buffer whenever the `Observable` produced by the specified `bufferClosingSelector` emits an item.

#### Arguments
1. `bufferClosingSelector` *(`Function`)*: A function invoked to define the closing of each produced window.

#### Returns
*(`Observable`)*: An observable sequence of windows.

#### Example
```js
// With closings
var source = Rx.Observable.timer(0, 50) // 從時間0開始，每50 milliseconds送出0, 1, 2, ...依此類推
  .buffer(function () { return Rx.Observable.timer(125); }) // bufferSelector會回傳一個timer的Observable，這個Observable會在第125 milliseconds的時候送出event，來清空buffer。
  .take(3);

var subscription = source.subscribe(
  function (x) {
    console.log('Next: %s', x);
  },
  function (err) {
    console.log('Error: %s', err);
  },
  function () {
    console.log('Completed');
  });

  // => Next: 0,1,2
  // => Next: 3,4,5
  // => Next: 6,7
  // => Completed
```

#### With buffer opening and buffer closing selector
```js
Rx.Observable.prototype.buffer(bufferOpenings, bufferClosingSelector);
```

bufferOpenings是一個Observable，用來決定什麼時候要開始buffer event；bufferClosingSelector，則是在buffer event開始之後，用來決定什麼時候要結束這個buffer，清空events。

This version of `buffer` monitors an `Observable`, `bufferOpenings`, that emits Observable objects. Each time it observes such an emitted object, it creates a new bundle to begin collecting items emitted by the source Observable and it passes the `bufferOpenings` Observable into the `bufferClosingSelector` function. That function returns an `Observable`. `buffer` monitors that `Observable` and when it detects an emitted object, it closes its bundle and emits it as its own emission.

1. `bufferOpenings` *(`Observable`)*: Observable sequence whose elements denote the creation of new windows.
2. `bufferClosingSelector` *(`Function`)*: A function invoked to define the closing of each produced window.

#### Returns
*(`Observable`)*: An observable sequence of windows.

#### Example

```js
/* Using Openings and Closing Selector */
var openings = Rx.Observable.interval(200);

var source = Rx.Observable.interval(50)
.buffer(openings, function (x) { return Rx.Observable.timer(x + 100); })
.take(3);

var subscription = source.subscribe(
  function (x) {
    console.log('Next: %s', x);
  },
  function (err) {
    console.log('Error: %s', err);
  },
  function () {
    console.log('Completed');
  });

  // => Next: 3,4
  // => Next: 7,8
  // => Next: 11,12
  // => Completed
  ```

#### With boundaries
```js
Rx.Observable.prototype.buffer(bufferBoundaries);
```

bufferBoundaries是一個Observable，當它送出event的時候，就是要清空buffer的時候。預設是從0開始。

Returns an `Observable` that emits non-overlapping buffered items from the source `Observable` each time the specified boundary `Observable` emits an item.

#### Arguments
1. `bufferBoundaries` *(`Observable`)*: Sequence of buffer boundary markers. The current buffer is closed and a new buffer is opened upon receiving a boundary marker.

#### Returns
*(`Observable`)*: An observable sequence of windows.

#### Example
```js
/* Using buffer boundaries */
var openings = Rx.Observable.interval(500);

// Convert the window to an array
var source = Rx.Observable.timer(0, 100)
  .buffer(openings)
  .take(3);

var subscription = source.subscribe(
  function (x) {
    console.log('Next: %s', x);
  },
  function (err) {
    console.log('Error: %s', err);
  },
  function () {
    console.log('Completed');
  });

  // => Next: 0,1,2,3,4
  // => Next: 5,6,7,8,9,10
  // => Next: 11,12,13,14,15
  // => Completed
```

### Location

File:
- [`/src/core/linq/observable/buffer.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/buffer.js)

Dist:
- [`rx.all.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.all.js)
- [`rx.all.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.all.js)
- [`rx.coincidence.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.coincidence.js)

Prerequisites:
- [`rx.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.js) | [`rx.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.compat.js) | [`rx.lite.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.lite.js) | [`rx.lite.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.lite.compat.js)

NPM Packages:
- [`rx`](https://www.npmjs.org/package/rx)

NuGet Packages:
- [`RxJS-All`](http://www.nuget.org/packages/RxJS-All/)
- [`RxJS-Coincidence`](http://www.nuget.org/packages/RxJS-Coincidence/)

Unit Tests:
- [`/tests/observable/buffer.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/tests/observable/buffer.js)
