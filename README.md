
### Before

| ObservableBase  | AnonymousObservable | BlockingObservable  | GroupedObservable | ConnectableObservable | 
|---------------  |-------------------  |-------------------  |-----------------  |---------------------  | 
| \_\_await__     |                     |                     |                   |                       | 
| \_\_add__       |                     |                     |                   |                       | 
| \_\_iadd__      |                     |                     |                   |                       | 
| \_\_getitem__   |                     |                     |                   |                       | 
|                 |                     | \_\_iter__          |                   |                       | 
|                 |                     |                     |                   |                       | 
| _subscribe_core | _subscribe_core     |                     | _subscribe_core   | _subscribe_core       | 
| subscribe_      |                     | subscribe_          |                   |                       | 
| subscribe       |                     | subscribe           | subscribe         |                       | 
| pipe            |                     |                     |                   |                       | 
|                 |                     |                     |                   | connect               | 
|                 |                     |                     |                   | auto_connect          | 
|                 |                     | first               |                   |                       | 
|                 |                     | first_or_default    |                   |                       | 
|                 |                     | for_each            |                   |                       | 
|                 |                     | last                |                   |                       | 
|                 |                     | last_or_default     |                   |                       | 
|                 |                     | to_marbles_blocking |                   |                       | 
|                 |                     | to_iterable         |                   |                       | 

### After

| ObservableBase  | Observable  | AnonymousObservable | BlockingObservable | GroupedObservable | ConnectableObservable |  | 
|-----------------|-------------|---------------------|--------------------|-------------------|-----------------------|--| 
|                 | \_\_await__ |                     |                    |                   |                       |  | 
|                 | \_\_add__   |                     |                    |                   |                       |  | 
|                 | \_\_iadd__  |                     |                    |                   |                       |  | 
|                 | \_\_getitem__ |                   |                    |                   |                       |  | 
|                 |             |                     | \_\_iter__           |                   |                       |  | 
|                 |             |                     |                    |                   |                       |  | 
| _subscribe_core |             | _subscribe_core     |                    | _subscribe_core   | _subscribe_core       |  | 
| subscribe_      |             |                     | subscribe_         |                   |                       |  | 
| subscribe       |             |                     | subscribe          | subscribe         |                       |  | 
| pipe            |             |                     |                    |                   |                       |  | 
|                 |             |                     |                    |                   | connect               |  | 
|                 |             |                     |                    |                   | auto_connect          |  | 
