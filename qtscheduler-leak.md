
## Issue
Actually, for non-periodic scheduling, we keep reference to QTimer objects and remove them on dispose. This is a problem because the QTimer objects are accumulated in the set and never removed if `dispose` is not called.
It can be easily demonstrated in the timeflies examples. Just add a print statement to display the length of the set on mouse move:
```python
def on_next(info):
    label, (x, y), i = info
    label.move(x + i*12 + 15, y)
    print(len(scheduler._timers))
    label.show()
```
See full example
<script src="https://gist.github.com/jcafhe/9295d50e7f98b9b834d4b70c129caaa5.js"></script>

## Fix
In theory, there should be no need to keep reference to QTimer objects since a reference is captured anyway in the local `dispose` function. 

However, a problem arises when one use directly sheduling method (`shedule`, `schedule_relative`, ...) and doesn't keep a reference to the returned disposable, as seen in tests.

For whatever reason, the timer will never fire `timeout` signal and the action will not be invoked. E.g.:
- `>>> disp = schedule.shedule_relative(1.0, action, state)` would work
- `>>> schedule.shedule_relative(1.0, action, state)` wouldn't.

By using `QTimer.singleShot` static method, we don't need to manage QTimer lifetime for non-periodic scheduling. 

**drawback**: we can't stop the timer to cancel the action before it is invoked. Thus, the action will be cancelled at invocation time. If you think this is not acceptable, I've come to an other solution relying on manual destruction of QTimer object, but it's a big 'ugly'. Please feel free to share your thoughts about that.

For periodic scheduling, we still construct a Qtimer object and keep a reference in a set. Periodic timers will only be deleted on dispose. IMO it makes sense to tight periodic timer lifetime to the returned `disposable`.





