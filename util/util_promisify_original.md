<!-- YAML
added: v8.0.0
-->

* `original` {Function}
* Returns: {Function}

采用一个函数作为最后一个参数，并返回一个返回值为promise的版本。作为函数参数的函数遵循异常优先的回调风格，例如`(err,value) => {...}`。

例如：

```js
const util = require('util');
const fs = require('fs');

const stat = util.promisify(fs.stat);
stat('.').then((stats) => {
  // Do something with `stats`
}).catch((error) => {
  // Handle the error.
});
```

或者，使用`async function`获得等效的效果:

```js
const util = require('util');
const fs = require('fs');

const stat = util.promisify(fs.stat);

async function callStat() {
  const stats = await stat('.');
  console.log(`This directory is owned by ${stats.uid}`);
}
```

如果原本就有 `original[util.promisify.custom]` 属性, `promisify` 会返回它的值， 详见 [Custom promisified functions][].

`promisify()` 会假定在所有情况下 `original` 是一个函数且最后的参数是回调函数。如果`original`不是一个函数，那么`promisify()`将抛出一个错误。如果`original`是一个函数但其最后一个参数不是一个错误优先的回调函数，它仍将被传入一个错误优先的回调函数作为最后一个参数。 

