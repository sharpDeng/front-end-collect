
# 构建前端性能监控系统

> 在前端开发工作中，除了项目开发保质保量上线以外，项目的数据监控也应该配套起来，确保线上的正常运转。如上报 pv 监控项目是否正常运转；测速上报反应项目质量；脚本错误监控作为监控中重要一环，当页面发生报错的时候，通过上报错误信息，能及时发现存在问题，修复优化、减少损失。

[本文转载自](https://github.com/joeyguo/blog/issues/13)

本文基于在手Q家校群前端脚本错误量优化的方案，致力于打造极致的脚本错误优化。

作为首篇，主要讲解基础的脚本错误监控和上报方式，以及常会遇到的 Script error. 的产生原因和处理方法。

## 监控上报
脚本错误主要有两类：语法错误、运行时错误。
监控的方式主要有两种：try-catch、window.onerror。

### 监控方法

#### 示例 · try-catch

```javascript
try {
    test  // <- throw error
} catch(e){
    console.log('运行时错误信息 ↙');
    console.log(e);
}
```

![捕捉错误](https://cloud.githubusercontent.com/assets/10385585/23819866/b46e35bc-0647-11e7-955d-b5c947c10349.png)

通过给代码块进行 try-catch 包装，当代码块出错时 catch 将能捕获到错误信息，页面也将继续执行。

当发生语法错误或异步错误时，则无法正常捕捉。

#### 示例 · try-catch (语法报错)

```javascript
try {
    function empty()   // <-  throw error 语法错误
} catch(e){
    console.log('语法错误信息 ↙');
    console.log(e);
}
```

![语法报错无法捕捉](https://cloud.githubusercontent.com/assets/10385585/23819873/eded7078-0647-11e7-9762-0cce1524bfc2.png)

#### 示例 · try-catch (异步错误)

```javascript
try {
    setTimeout(function() {
        test // <- throw error 异步错误
    },0)
} catch(e){
    console.log('异步错误信息 ↙');
    console.log(e);
}
```

无法捕捉错误

![无法捕捉错误](https://cloud.githubusercontent.com/assets/10385585/23819887/2ab8b9ae-0648-11e7-8c52-e436726b19a6.png)

语法错误无法在 try-catch 中进行捕抓、而异步报错则可以通过为异步函数块再包装一层 try-catch，增加标识信息来配合定位，可以用工具来进行处理，这里不展开。

#### 示例 · window.onerror

```javascript
/**
 * @param {String}  msg    错误信息
 * @param {String}  url    出错文件
 * @param {Number}  row    行号
 * @param {Number}  col    列号
 * @param {Object}  error  错误详细信息
 */
window.onerror = function (msg, url, row, col, error) {
    console.log('onerror 错误信息 ↙');
    console.log({
        msg,  url,  row, col, error
    })
};

test // <-  throw error
```

![onerror](https://cloud.githubusercontent.com/assets/10385585/23819935/1c99afbc-0649-11e7-8073-cda0ff1208a5.png)

window.onerror 能捕捉到当前页面的语法错误或运行时报错，是十分强大的。
那么try-catch 是否不再需要呢？其实并不是。

在使用过程中的体会：onerror 主要用来捕获预料之外的错误，而 try-catch 则可以用在预知情况下监控特定错误，两种形式结合使用更加高效。

### 上报方式

监控错误拿到了报错信息，接下来则是将捕抓的错误信息发送到信息收集平台上，发送的形式主要有两种：

1. 通过Ajax发送数据
2. 动态创建 img 标签的形式

示例 · 动态创建 img 标签进行上报

```javascript
function report(msg, level) {
    var reportUrl = "http://localhost:8055/report";
    new Image().src = reportUrl + '?msg=' + msg;
}
```

### 监控上报整体流程

监控报错，并将捕捉到的错误信息上报给数据收集平台，如下图

![流程图](https://cloud.githubusercontent.com/assets/10385585/23819962/80913706-0649-11e7-9ffb-85ce40efad69.png)

## 错误信息分析 · Script error

有了监控了后，就可以在收集平台上进行查看脚本错误量的日志统计。

![统计](https://cloud.githubusercontent.com/assets/10385585/23838826/702b42ee-07d4-11e7-9853-2c135550e00e.png)

发现占据榜首的错误信息 “Script error.” 具有非常高的比例，没有无具体的错误信息，无法定位问题，而这是怎么产生的呢？

### 产生 Script error 的原因

翻看在 [webkit 的源码](http://trac.webkit.org/browser/trunk/Source/WebCore/dom/ScriptExecutionContext.cpp)可以看到 “Script error.” 是浏览器在同源策略限制下所产生的。浏览器出于安全上的考虑，当页面引用的非同域的外部脚本中抛出了异常，此时本页面无权限获得这个异常详情， 将输出 Script error 的错误信息。

![script error 原因](https://cloud.githubusercontent.com/assets/10385585/23820174/bb73b3e0-064d-11e7-9bdb-5e382fb549d6.png)

### 优化 Script error

Script error 来自同源策略的影响，那么解决的方案之一是进行资源的同源化，另外也可以利用跨源资源共享机制( CORS )。

#### 方案一：同源化

1. 将js代码内联到html文件中
2. 将js文件与html文件放到同一域名下
以上两种方式能够简单直接地解决问题，但也可能带来其他影响，如内联资源不好利用文件缓存，同域无法充分利用cdn优势等等。

#### 方案二：跨源资源共享机制( CORS )

跨源资源共享 ( CORS )机制让Web应用服务器能支持跨站访问控制，从而能够安全地跨站数据传输。主要是通过给请求带上特定头信息，服务器实现了CORS接口，就可以跨源通信，从而能够看到具体报错信息。

 1. 为页面上script标签添加crossorigin属性。

```javascript
<script src="http://127.0.0.1:8077/main.js" crossorigin></script>
```

增加 crossorigin 属性后，浏览器将自动在请求头中添加一个 Origin 字段，发起一个 跨来源资源共享 请求。Origin 向服务端表明了请求来源，服务端将根据来源判断是否正常响应。

![](https://cloud.githubusercontent.com/assets/10385585/23839391/b15066b0-07d8-11e7-81eb-f2e0b63ec02b.png)

2. 响应头中增加 Access-Control-Allow-Origin 来支持跨域资源共享。

![](https://cloud.githubusercontent.com/assets/10385585/23839417/eb2cde86-07d8-11e7-887f-4824f0dcafa8.png)

Access-Control-Allow-Origin: * 表示通过该跨域请求，且该资源可以被任意站点跨站访问。而当该资源仅允许来自 http://127.0.0.1:8066 的跨站请求，其它站点都不能跨站访问时，将可以返回：

```java
Access-Control-Allow-Origin:http://127.0.0.1:8066
```

3. 指定域名的 Access-Control-Allow-Origin 的响应头中需带上Vary:Origin。

Vary 字段的作用在于为缓存服务器提供缓存规则及缓存筛选的依据。当增加 Vary:Origin 响应头后，缓存服务器将会按照 Origin 字段的内容，缓存不同版本，在请求响应时根据请求头中的 Origin 决定是否能够使用缓存响应。

![](https://cloud.githubusercontent.com/assets/10385585/23840619/aef5528c-07e1-11e7-8131-bc0b595ac2cc.png)

举例 · 不加 Vary 将存在错误命中缓存的问题

![](https://cloud.githubusercontent.com/assets/10385585/23839839/aa8fb4ea-07db-11e7-8e9f-6258f6d610cd.png)

上图中，第一个请求（Origin: 127.0.0.1:8066）响应被浏览器缓存了，当第二个请求（Origin: 127.0.0.1:8888）发起，被错误命中了前一个请求的缓存，收到了 Access-Control-Allow-Origin:http://127.0.0.1:8066 的响应时，将导致资源加载失败。
所以当 Access-Control-Allow-Origin 不是返回为 * 时，需要加上 Vary 返回头来避免引缓存导致的权限问题。

跨域脚本报错产生 Script error. 通过以上方式进行处理后将能够捕获到具体的报错信息了。在 NodeJS 的实现中主要通过添加以下代码：

```JavaScript
app.use(function *(next){
    // 拿到请求头中的 Origin
    var requestOrigin = this.get('Origin'); 
    if (!requestOrigin) { // 不存在则忽略
      return yield next;
    }

    // 设置 Access-Control-Allow-Origin: Origin
    this.set('Access-Control-Allow-Origin', requestOrigin);

    // 设置 Vary: Origin
    this.vary('Origin');
    return yield next;
});
```

