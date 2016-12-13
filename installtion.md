##ES6 via npm
```
npm install rxjs-es
```
导入核心功能块
```
import Rx from 'rxjs/Rx';

Rx.Observable.of(1,2,3)
```

仅作为补充部分导入你所需要的(通常这种做法有利于减小文件大小)
```
import {Observable} from 'rxjs/Observable';
import 'rxjs/add/operator/map';

Observable.of(1,2,3).map(x => x + '!!!'); // etc
```

要导入您需要的并且与提出的绑定操作符一起使用它
```
import { Observable } from 'rxjs/Observable';
import { of } from 'rxjs/observable/of';
import { map } from 'rxjs/operator/map';

Observable::of(1,2,3)::map(x => x + '!!!'); // etc
```
