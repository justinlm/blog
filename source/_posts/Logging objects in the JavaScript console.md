---
title: JavaScript 控制台输出引用对象
date: 2019/08/24
tag: JavaScript
---
浏览器控制台输出引用对象内容要注意使用```console.log(JSON.parse(JSON.stringify(obj)))```
如下是摘自developer.mozilla的说明：

## [Logging objects](https://developer.mozilla.org/en-US/docs/Web/API/Console/log#Logging_objects)
Don't use console.log(obj), use ```console.log(JSON.parse(JSON.stringify(obj)))```.


This way you are sure you are seeing the value of obj at the moment you log it. Otherwise, many browsers provide a live view that constantly updates as values change. This may not be what you want.