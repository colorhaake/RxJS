### `Rx.Observable.prototype.and(rightSource)`
[&#x24C8;](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/and.js "View in source")

Propagates the observable sequence that reacts first.

這個operator要和thenDo及When，一起搭配使用；不能單獨用。
直接看下方例子。

#### Arguments
1. `right` *(`Observable`)*: Observable sequence to match with the current sequence.

#### Returns
*(`Pattern`)*: Pattern object that matches when both observable sequences have an available value.

回傳的是Pattern object不是Observable喔。
#### Example
```js
// Choice of either plan, the first set of timers or second set
var source = Rx.Observable.when(
    Rx.Observable.timer(200).and(Rx.Observable.timer(300)).thenDo(function (x, y) { return 'first'; }),
    Rx.Observable.timer(400).and(Rx.Observable.timer(500)).thenDo(function (x, y) { return 'second'; })
);

當and左邊及右邊的operator都有送event的時候，才會去執行thenDo
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

// => Next: first
// => Next: second
// => Completed
```

注意這個例子
```js

const next = (msg) => console.log('[Nest] ' + msg);
const error = (msg) => console.log('[Error] ' + msg);
const complete = () => console.log('[Complete]');

const mapFirst = (event) => 'first';
const mapSecond = (event) => 'second';
const mapThird = (event) => 'third';
const mapFourth = (event) => 'fourth';
const mapFifth = (event) => 'fifth';
const mapSixth = (event) => 'sixth';

const timer1$ = Rx.Observable.timer(100);
const timer2$ = Rx.Observable.timer(200);
const timer3$ = Rx.Observable.timer(300);
const timer4$ = Rx.Observable.timer(400);

const amb$ = Rx.Observable.amb(
  timer2$.map(mapFirst),
  timer3$.map(mapSecond)
);

const and1$ = timer1$.and(timer2$).thenDo(mapFirst);
const and2$ = timer2$.and(timer3$).thenDo(mapSecond);
const and3$ = timer3$.and(timer4$).thenDo(mapThird);
const and4$ = timer1$.and(timer3$).thenDo(mapFourth);
const and5$ = timer1$.and(timer4$).thenDo(mapFifth);
const and6$ = timer2$.and(timer4$).thenDo(mapSixth);

const when$ = Rx.Observable.when(
  and1$, and2$,
  and3$, and4$,
  and5$, and6$);

const result$ = when$;
result$.subscribe(next, error, complete);

// 結果為：

/*
"[Nest] first"
"[Nest] third"
"[Complete]"
*/

// 如果把timer1$, timer2$, timer3$, timer4$都改成interval的話，結果如下
// 至於為什麼會這樣，尚待了解。

/*
"[Nest] first"
"[Nest] fourth"
"[Nest] first"
"[Nest] fifth"
"[Nest] first"
"[Nest] fourth"
"[Nest] first"
"[Nest] fifth"
*/
```
### Location

File:
- [/src/core/linq/observable/and.js](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/and.js)

Dist:
- [`rx.all.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.all.js)
- [`rx.all.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.all.compat.js)
- [`rx.joinpatterns.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.joinpatterns.js)

Prerequisites:
- [`rx.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.js) | [`rx.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.compat.js) | [`rx.lite.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.lite.js) | [`rx.lite.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.lite.compat.js)

NPM Packages:
- [`rx`](https://www.npmjs.org/package/rx)

NuGet Packages:
- [`RxJS-All`](http://www.nuget.org/packages/RxJS-All)
- [`RxJS-JoinPatterns`](http://www.nuget.org/packages/RxJS-JoinPatterns)

Unit Tests:
- [/tests/observable/when.js](https://github.com/Reactive-Extensions/RxJS/blob/master/tests/observable/when.js)
