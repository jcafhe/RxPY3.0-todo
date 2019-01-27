## Current implementation
### VirtualTimeScheduler [conccurency.virtualtimescheduler](https://github.com/ReactiveX/RxPY/blob/master/rx/concurrency/virtualtimescheduler.py)
- **clock**: initial clock value and absolute time comparer

### ReactiveTest [testing.reactivetest](https://github.com/ReactiveX/RxPY/blob/master/rx/testing/reactivetest.py)
- **created**: 200
- **subscribed** = 200
- **disposed** = 1000

### TestScheduler [testing.testcheduler](https://github.com/ReactiveX/RxPY/blob/master/rx/testing/testscheduler.py)

based on ReactiveTest created, subscribed, disposed.


# Marble update
## breaking changes
- each character is considered as an item and advance time by one tick (except for space) e.g. `-ab-c--` will produce `a` at 1 tick, `b` at 2 tick, `c` at 4 tick instead of `ab` at 1 tick, `c` at 3 ticks.

- change error symbol from `x` or `X` to `#`

## new features

- add group of items e.g. `--(bc)-d-`. Every items in a group share the same time defined by `(`. Each character (even parenthesis) advance time by one tick. `b` and `c` will be produced at 2 ticks, `d` at 7 ticks.

- add optional *lookup* dictionnary to convert an ASCII item to a specified value. `from_marbles('a-b--', lookup={a:1, b:2}` will produce `-1-2--`

- add optional *error* parameter to specified the exception raised by `#`.

- add `^` as the subscription symbol. Each items that have been declared before subscription will have negative time. `a-^--b--` will produce `a` at -2 ticks, `b` at 3 ticks.



