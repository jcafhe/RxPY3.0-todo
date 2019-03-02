# Explored fixs
## remove `self._timers`
In theory, there should be no need to keep reference to QTimer object since a reference is captured anyway in the local `dispose` function. 
However, a problem arises when user doesn't hold a reference to the disposable returned by `subscribe` method, as seen in tests.
For whatever reason, the timer will never fire `timeout` signal and the action will not be invoked for non-periodic scheduling.
E.g. `>>> disp = schedule.shedule_relative(1.0, action, state)` would work, but `>>> schedule.shedule_relative(1.0, action, state)` wouldn't.

## use static method `QTimer.singleShot` for non periodic scheduling
This is the cleaner fix since we don't need to manage QTimer lifetime. We still keep reference to periodic QTimers because it make sense to tight QTimer lifetime to the `disposable` returned by `subscribe` method.
However, `QTimer.singleShot(int:msec, Functor:functor)` overload is only available in QT5.4 according to Qt documentation.
- QT5: https://doc.qt.io/qt-5/qtimer.html#singleShot-4
- QT4.8: https://doc.qt.io/archives/qt-4.8/qtimer.html#singleShot

