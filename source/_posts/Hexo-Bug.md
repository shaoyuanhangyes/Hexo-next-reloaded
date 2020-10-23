---
title: Hexo Bug
date: 2020-10-23 10:00:01
categories: 
- Hexo配置
tags:
- Hexo
---

在书写markdown数学公式的时候 习惯性的使用`｛｝`中同一类型运算下的公式括起来 导致有两个同侧括号在一起 导致Hexo生成HTML的时候无法解析`｝` 
生成的bug提示如下
<!-- more --> 
```bash
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Template render error: (unknown path) [Line 8, Column 322]
  parseAggregate: expected comma after expression
    at Object._prettifyError (D:\Code\My Code\blog\node_modules\nunjucks\src\lib.js:36:11)
    at Template.render (D:\Code\My Code\blog\node_modules\nunjucks\src\environment.js:526:21)
    at Environment.renderString (D:\Code\My Code\blog\node_modules\nunjucks\src\environment.js:364:17)
    at D:\Code\My Code\blog\node_modules\hexo\lib\extend\tag.js:62:48
    at tryCatcher (D:\Code\My Code\blog\node_modules\bluebird\js\release\util.js:16:23)
    at Function.Promise.fromNode.Promise.fromCallback (D:\Code\My Code\blog\node_modules\bluebird\js\release\promise.js:180:30)
    at Tag.render (D:\Code\My Code\blog\node_modules\hexo\lib\extend\tag.js:62:18)
    at Object.onRenderEnd (D:\Code\My Code\blog\node_modules\hexo\lib\hexo\post.js:282:20)
    at D:\Code\My Code\blog\node_modules\hexo\lib\hexo\render.js:65:19
    at tryCatcher (D:\Code\My Code\blog\node_modules\bluebird\js\release\util.js:16:23)
    at Promise._settlePromiseFromHandler (D:\Code\My Code\blog\node_modules\bluebird\js\release\promise.js:512:31)
    at Promise._settlePromise (D:\Code\My Code\blog\node_modules\bluebird\js\release\promise.js:569:18)
    at Promise._settlePromise0 (D:\Code\My Code\blog\node_modules\bluebird\js\release\promise.js:614:10)
    at Promise._settlePromises (D:\Code\My Code\blog\node_modules\bluebird\js\release\promise.js:694:18)
    at _drainQueueStep (D:\Code\My Code\blog\node_modules\bluebird\js\release\async.js:138:12)
    at _drainQueue (D:\Code\My Code\blog\node_modules\bluebird\js\release\async.js:131:9)
    at Async._drainQueues (D:\Code\My Code\blog\node_modules\bluebird\js\release\async.js:147:5)
    at Immediate.Async.drainQueues (D:\Code\My Code\blog\node_modules\bluebird\js\release\async.js:17:14)
    at processImmediate (internal/timers.js:439:21)
```
