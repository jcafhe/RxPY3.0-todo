
# Marbles syntax
## breaking changes
- each character will be interpreted as a marble (except for special characters). E.g. `-abc-` will be interpreted as 3 consecutive marbles `a`, `b`, `c`, instead of one marble `abc`.

- each character advance time by one tick (except for space), e.g. `-ab-c--` will produce `a` at 1 tick, `b` at 2 tick, **`c` at 4 tick** instead of `ab` at 1 tick, **`c` at 3 tick**.

- change error symbol from `x` or `X` to `#`

## new features

- add support for grouping marbles e.g. `--(bc)-d-`. Every marble in a group share the same timestamp defined by the first `(`. Each character (even parentheses) advance time by one tick. `b` and `c` will be produced at 2 ticks, `d` at 7 ticks.

- add optional *lookup* dictionnary to convert a marble to the specified value. E.g. `from_marbles('a-b--', lookup={a:11, b:22}` will produce `11` at 0 and `22` at 2 * timespan.

- add optional *error* parameter to specify the exception raised by `#` marble.

- add `^` as the subscription symbol. If subscription time is 0, each marble that has been declared before subscription will have negative relative time. E.g. `a-^--b--` will produce `a` at -2 ticks, `b` at 3 ticks. This should only be used in `hot()` function (see below).


# New functions
## `parse(string, [timespan, lookup, error, time_shift]) -> List[Recorded]`
Convert a string to a list of records (Recorded). The optional *time_shift* parameter allows to set the subscription time. This will set the time of the `^` marble if specified, otherwise of the first character in the diagram (except for space). 

## `test_context(timespan)`
Setup a TestScheduler and returns the functions below as a namedtuple. This unsures that everything will be called with the same values for the following parameters: subscribed, created, disposed, timespan. Parameters are set to:
- created = 100
- subscribed = 190
- disposed = 1000

Regarding `hot()` and `exp()` functions, the timings are shifted by +10 to unsure that the first character will not be skipped by the scheduler (a bit of a hack). So the first character will have a timestamp of 190 + 10 = 200.

### `cold(string, [lookup, error]) -> Observable`
Parse a marbles string and return a cold observable by calling `TestScheduler.create_cold_observable()`.

### `hot(string, [lookup, error]) -> Observable`
Parse a marbles string and return a hot observable by calling `TestScheduler.create_hot_observable()`.

### `start(create | observable) -> List[Recorded]`
Wrapper around `TestScheduler.start()`. Returns a list of records instead of a MockObserver (i.e. mockobserver.messages).

### `exp(string, [lookup, error]) -> List[Recorded]`
Parse a marbles string and return a list of records.

