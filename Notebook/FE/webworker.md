
在 vite 环境中使用 web worker 时，如果遇到生产环境中 worker.js 文件的 MIME 类型被识别为 text/html，导致报错无法运行的情况时，可以参考以下两种方法，原理都是避免编译时产出单独的 worker.js 文件。

#### 方法一

worker文件不需要包装，引入时后缀增加 ?worker&inline，使用时直接 new ImportedWorker();
```javascript
// worker.js
self.addEventListener(
  'message',
  function (e) {
    const {
      data: { type, payload },
    } = e;
  }
);

```javascript
import ImportedWorker from './worker?worker&inline';
const worker = new ImportedWorker();

worker.addEventListener ...
```

#### 方法二

worker导出为函数；需要一个转换函数把 worker 转换为URL；
```javascript
// worker.js
export function hilitorWorker() {
  self.addEventListener(
    'message',
    function (e) {
      const {
        data: { type, payload },
      } = e;
    });
}
```

```javascript
export function createWorker(fn: any) {
	return new Worker(URL.createObjectURL(new Blob([`(${fn})()`], { type: 'text/javascript' })));
}
```

```javascript
// 使用
const worker = createWorker(hilitorWorker);
```

## worker 改造成async/await 服务调用方式

调用方：
```javascript
var a = {};
```

接收方：
