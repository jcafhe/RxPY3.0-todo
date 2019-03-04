
## Issue
Actually, we keep references to QTimer objects in a set and remove them on dispose. This is a problem for non-periodic scheduling because the QTimer objects are accumulated in the set and never removed if `dispose` is not called.
It can be easily demonstrated with the timeflies examples. Just add a print statement to display the length of the set on mouse move:
```python
def on_next(info):
    label, (x, y), i = info
    label.move(x + i*12 + 15, y)
    print(len(scheduler._timers))
    label.show()
```
See full example [here](https://gist.github.com/jcafhe/9295d50e7f98b9b834d4b70c129caaa5)

## Fix
In theory, there should be no need to keep reference to QTimer objects since a reference is captured anyway in the local `dispose` function. 

However, a problem arises when one uses directly the scheduler and sheduling method (`shedule`, `schedule_relative`, ...) and doesn't keep a reference to the returned disposable, as seen in tests.

For whatever reason, the timer will never fire `timeout` signal and the action will not be invoked. E.g.:
- `>>> disp = schedule.shedule_relative(1.0, action, state)` would work
- `>>> schedule.shedule_relative(1.0, action, state)` wouldn't.

By using `QTimer.singleShot` static method, we don't need to manage QTimer lifetime for non-periodic scheduling. 

**drawback**: we can't stop the timer to cancel the action before it is invoked. Thus, the action will be cancelled at invocation time. If you think this is not acceptable, I've come to an other solution relying on manual destruction of QTimer object, but it's a big 'ugly' (we keep a python object wrapping a C++ timer which has potentially already been deleted). Please feel free to share your thoughts about that.

For periodic scheduling, we still construct a Qtimer object and keep a reference in a set. Periodic timers will only be deleted on dispose. IMO it makes sense to tight periodic timer lifetime to the returned `disposable`.

