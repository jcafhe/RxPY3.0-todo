# move core to internal. Circular dependencies:
## `rx.internal.basic.noop`
test case `import rx`
- rx 
- rx.internal.init[**Observable**]
- rx.internal.observable.init[**Observable**]
- rx.internal.observable.observable.py
- rx.disposable.init[**Disposable**]
- rx.disposable.disposable.py
- rx.internal.init[**noop**] --> import error if noop is imported after Observable in rx.internal.init 

## `rx.internal.priorityqueue.PriorityQueue`
- rx 
- rx.internal.init[**Observable**]
- rx.internal.observable.init[**Observable**]
- rx.internal.observable.observable.py
- rx.concurrency.init[**CurrentThreadScheduler**]
- rx.concurrency.currentthreadScheduler.py
- rx.internal.init[**PriorityQueue**] --> import error if PriorityQueue is imported after Observable in rx.internal.init 
