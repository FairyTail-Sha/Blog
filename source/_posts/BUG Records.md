# BUG Records

标签（空格分隔）： bug angular

---
*2018/10/23*

## bug-ts-1
TypeError: Observable_1.Observable.from is not a function
```
import 'rxjs/Observable';
```
替换为
```
import { Observable } from 'rxjs';
import 'rxjs/add/observable/from';
```




