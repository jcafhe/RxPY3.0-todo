## Current implementation
### VirtualTimeScheduler [conccurency.virtualtimescheduler](https://github.com/ReactiveX/RxPY/blob/master/rx/concurrency/virtualtimescheduler.py)
- **clock**: initial clock value and absolute time comparer

### ReactiveTest [testing.reactivetest](https://github.com/ReactiveX/RxPY/blob/master/rx/testing/reactivetest.py)
- **created**: 200
- **subscribed** = 200
- **disposed** = 1000

### TestScheduler [testing.testcheduler](https://github.com/ReactiveX/RxPY/blob/master/rx/testing/testscheduler.py)

based on ReactiveTest created, subscribed, disposed.

# New API
### MarbleScheduler
- accept MarbleObservable with subscription/unsubcribe informations.
