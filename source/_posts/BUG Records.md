---
title: Angualr BUG Records
date: 2018-10-23
tags: angular
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




