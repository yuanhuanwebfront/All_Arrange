## 1. Doctype的作用

> Doctype表示文档的类型，必须是html文档的第一行
> 它的作用是告诉浏览器以何种方式解析文档
> 指定后就会按照标准模式解析，否则就会使用兼容模式解析



## 2. 标准模式和兼容模式有什么区别？

> 标准模式下的渲染方式和js解析方式都是按照浏览器支持的最高标准运行。
> 
> 兼容模式中，页面以宽松的向后兼容的方式显示，模拟老式浏览器行为以防止站点无法工作。



## 3. 什么是DTD?

> DTD(文档类型定义)是一组机器可读的规则。
> 定义XML和HTML的特定版本中所有允许元素和它们的属性



## 4. html为什么只需要写《!DOCTYPE HTML》 而不需引入DTD

> HTMl5 不基于SGML，因此不需要对DTD进行引用。



## 5. SGML、HTML、XML、XHTML的区别
> SGML 是标准通用标记语言，是一种定义电子文档结构和描述内容的国际标准语言。
> HTML 是超文本标记语言，主要是用于规定如何显示网页。
> XML 也是标记语言，XML的标签可以自由创建，数量无限。
> XHTML 与HTML差不多，区别在于XHTML更严格，比如标签小写，标签必须闭合。

## 6. 行内元素和块级元素的区别

> 1. 页面布局上，行内元素在一条直线上水平排列；块级元素默认占据一行，垂直方向排列
> 2. 行内元素只能包含文本元素和行内元素；块级元素可以包含块级和行级元素
> 3. 行内元素设置`width`,  `height`, `margin`, `padding` 上下无效。  

## 7. link 和 @import 有什么区别

> 1. @import 是css提供的语法规则，只有导入样式的作用；link是html提供的标签，不仅可以加载css文件，还可以定义其他属性。
> 2. link引入的css同时被加载；@import引入的css将在页面加载完成后加载。
> 3. @import是一种语法存在兼容问题，link是html标签，不存在兼容问题

## 8. 浏览器渲染流程

> 1. 浏览器打开一个网页时，都会去请求对应的 `html` 文件，浏览器接收到 `html` 文件后，会将字符串转换为 `标记` ，在完成标记后会转换成 `Node` 节点，然后组合成为 `DOM` 树。
> 2. 对 `CSS` 文档进行解析，生成  `CSSOM` 树
> 3. 当 `DOM` 树和 `CSSOM` 树构建完成后，每个渲染对象是由一个 **节点** 和该节点的 **css盒模型** 组成的。不可见的DOM元素不会出现在渲染树中。渲染对象并没有位置和大小，设置这些值的过程叫做 `回流` 。采用递归的方式，从根元素开始，为每个元素计算大小和位置。从而形成 `render tree`。
> 3. 之后会进行 `render tree` 的遍历，并将内容显示在屏幕上。

## 9. 渲染过程遇到js怎么处理？

> + js的`加载`、`解析`、`执行` 都会阻塞文档的解析
> + js脚本建议放在文档底部，防止阻塞渲染。
> + 可以添加`async`和`defer`来异步加载js文件。
>   + `defer`表示延迟执行脚本，脚本加载的同时`html`也在进行解析，执行是在文档解析完成后执行的。
>   + `async`是表示异步执行脚本，如果脚本已经加载完成，那么脚本就会执行，依然有可能阻塞文档解析。

## 10. 什么是文档的预解析 ？

> + 传统的浏览器中，`HTML解析器`运行于主线程之中，并且遇到脚本后会被阻塞。`预解析`会从主线程中分离，能够预先加载`脚本`、`样式表`、`图片`。

## 11. CSS如何阻塞文档解析 ？

> + 理论上，样式表不会改变DOM树，就没有必要停止解析文档等待它们。但是，脚本执行的时候可能会请求样式信息，如果此时样式还没有加载，就会得到错误的值。
> + 因此，如果浏览器没有完成样式表的下载和构建，此时想运行脚本，那么浏览器会延迟`脚本的执行`和`文档解析`,  直到完成`CSSOM`的下载和构建。也就是说，这种情况下，浏览器会`先下载和构建 CSSOM`，`再执行Javascript` ，最后在进行`文档的解析`。  

## 12. 渲染页面时会有什么不良现象？

> + FOUC：这种现象被称为`文档样式短暂失效`，这是因为在`引用样式表`的时候，引用方法不对或者文档位置不对。导致先加载了无样式的文档，之后再突然显示有样式的文档。
> + 白屏：在某些浏览器下，由于需要先构建`DOM`和`CSSOM`，构建完成后在进行渲染，如果CSS放在HTML底部，由于`CSS`未加载完成，导致页面出现白屏；也有可能是因为将`Javascript`文件放在了头部，脚本解析的时候会`阻塞文档解析和渲染`。

## 13. 如何优化关键渲染路径？

> + 浏览器将`HTML`、`CSS`、`Javascript`转化为屏幕上所显示的实际像素，这期间所经历的一系列步骤，被称为`关键渲染路径`。[详细](https://github.com/berwin/Blog/issues/29)
>+ 具体步骤：
> 
>  1. 获取`HTML`并开始构建`DOM`
> 
>  2. 获取`CSS`并开始构建`CSSOM`
>   3. 结合`DOM`和`CSSOM`形成`渲染树`。
>   4. 根据样式确定每个节点的位置和大小，进行布局。
>   5. 绘制像素。
> + 优化方式  [详细](https://zhuanlan.zhihu.com/p/57752042)
>
>   1. **关键资源的数量优化**
>     + 关键资源是指那些可以阻塞页面首次渲染的资源，比如JS、CSS文件。这些文件的数量越少，渲染速度越快
>   2. **关键路径的长度优化**
>      + 关键路径长度是指浏览器和服务器之间的往返次数；例如页面有一个JS资源，那么关键路径就为2，因为先加载一次`HTML`，再加载一次`JS`。如果一个页面有一个`CSS`和一个`JS`，那么关键路径长度也为2，因为这两个文件可以同时加载。
>   3. **关键字节的数量优化**
>      + 关键字节的数量指的是关键资源的字节大小，浏览器要下载的资源字节越小，那么下载和加载的速度都会加快。

## 重绘和回流

> + 回流： 当渲染树因为元素的尺寸、布局等改变影响布局的操作。（width、height、position）
> + 重绘： 当渲染树中的一些元素需要更新属性，这些属性只影响元素的外观、风格不影响布局。(color)
> + `回流`一定会引起`重绘`    `重绘`不一定引起`回流`。
> + 减少回流可以使用以下方法
>   + 使用`transform`代替`top`
>   + 不使用`table`布局
>   + 减少直接修改DOM样式，改为通过修改类名修改样式。

## DOMContentLoaded和Load事件的区别？

> + `DOMContentLoaded`是指文档被完全加载和解析完成之后触发
> + `Load`事件当所有资源加载完成后触发。

## HTML语义化

> 1. 用正确的标签去描述正确的事情
> 2. 语义化让页面内容结构化，更清晰，便于浏览器、搜索引擎解析。
> 3. 搜索引擎的爬虫也依赖于`HTML`标记来确定上下文和关键字的权重，有利于`SEO`。
> 4. 页面更容易分块，便于阅读和维护。（`header`、`footer`）

## 前端SEO注意事项

> + 合理的`title`、`description`、`keywords`，三个选项的权重依次降低。
> + 语义化的HTML代码，符合W3C规范，可以让搜索引擎更容易理解网页。
> + 重要内容放在页面头部，抓取顺序是由上到下。
> + 重要内容不要使用`JS`输出，搜索引擎不会抓取JS中的内容。
> + 少用`iframe`
> + 非装饰性图片必须加`alt`。

## HTML5的离线缓存怎么使用，工作原理

> + 在用户没有网络连接时，可以正常访问站点和应用，在用户正常连接网络时，更新用户机器上的缓存文件。
>
> + **原理：** 离线存储是基于一个新建的`.appcache`文件的缓存机制。通过这个文件上的解析清单离线存储资源，这些资源就会像`Cookie`一样被存储了下来。之后当网络处于离线状态时，浏览器会通过被离线存储的数据进行页面展示。
>
> + **如何使用？**
>
>   + 创建一个和html同名的`manifest`文件，在页面头部加入下面一个`manifest`属性
>
>      ```html
>      <html lang="en" manifest="index.manifest">
>      ```
>
>   + 在如下 `cache.manifest`文件的编写离线存储的资源。
>
>     ```javascript
>     CACHE MANIFEST
>     #V0.11
>     CACHE: 
>     js/app.js
>     css/style.css
>     NETWORK:
>     resource/logo.png
>     FALLBACK:
>     / /offline.html
>
>     ```
>
>     > **CACHE: **表示需要离线存储的资源列表，由于`manifest`文件的页面将被自动离线存储，所以不需要把自身也列出来
>     >
>     > 
>     >
>     > **NETWORK**: 表示在它下面列出来的资源只有在线的状态下才能访问，不会被离线存储，因此离线情况下无法使用。
>     >
>     > 
>     >
>     > **FALLBACK:** 表示如果访问第一个资源失败，那么就会使用第二个资源来替换他，比如这个配置访问根目录下的任何一个资源失败了，那么就去访问`offline.html`。
>
>   + 如何更新缓存
>
>     > + 更新`manifest`文件 
>     > + 通过 JS 操作
>     > + 清除浏览器缓存
>
> + 注意事项
>
>   + 浏览器对缓存数据的容量限制可能不太一样。
>   + 如果`manifest`文件或者配置中的某个文件不能正常下载，那么将会使用历史缓存。
>   + 引用`manifest`的html必须与mainfest文件同源，在一个域下面。
>   
> + 运行原理
>
>   + 在线的情况下，浏览器发现`html`头部存在`manifest`	文件。如果第一次访问`app`，那么浏览器会根据`manifest`文件的内容下载相应的资源并进行离线存储。如果已经访问过并且资源已经离线存储了，那么浏览器就会使用离线的资源加载文件，然后浏览器会对比新旧`manifest`文件，如果文件发生改变，那么就会重新下载资源并存储。	
>   
>

## Cookies、sessionStorage和localstorage的区别？

> + `cookie`开始是为了服务端记录用户状态的一种方式，由服务端设置，在客户端存储，然后每次发器同源请求时，发送给服务器端。`cookoe`最多可以存储`4KB`数据，它的有效时间由`expires`属性指定，并且`cookie`只能被同源的页面访问共享。
> + `sessionStorage`时`HTML5`提供的一种浏览器本地存储的方法，它借鉴了服务端`session`的概念，代表了一次会话中所保存的数据。一般能存储`5MB`或者更大的数据。它在窗口关闭后就会失效，并且只能被同一个窗口的同源页面访问共享。
> + `localStorage`一般也能够存储`5MB`或者更大的数据，与`sessionStorage`不同的是，除非手动删除，否则它不会失效。

## iframe有哪些缺点？

> + 会阻塞主页面的`onload`事件，`onload`事件会在`iframe`加载完毕后触发。
> + 搜索引擎无法解析这种页面，不利于`SEO`。
> + `iframe`和主页面共享连接池，会影响页面并行加载。
> + 后退按钮失效

## 浏览器内多个标签页之间的通信如何实现

> + 使用`WebSocket`，通信的标签页连接同一个服务器，发送消息到服务器后，服务器推送消息给连接的客户端。
> + 使用`SharedWorker`，两个页面共享一个线程，通过线程发送数据和接收数据来实现标签之间的通信。
> + 可以调用`localStorage`、`Cookies`等本地存储方式，`localStorage`另一个浏览上下文里被添加、修改和删除时，它都会触发一个`storage`事件，我们通过`storage`事件，控制它的值进行页面信息通信。
> + 如果我们能获得对应标签页的引用，可以通过`postMessage`方法实现多个标签页通信。

## 性能优化

> + **页面**
>
>   1. 文件合并、CSS雪碧图、base64转码、iconfont减少`HTTP`请求数，避免请求造成等待的情况。
>
>   2. 使用DNS缓存和预加载减少DNS查询次数
>
>   3. 设置缓存策略，对常用不变的资源进行缓存。
>
>   4. 使用延迟加载的方式，减少页面首屏加载时需要请求的资源。（图片懒加载、script的异步加载）
>
>   5. 部分资源使用预加载，提高访问速度。
>
>      1. Preload用于预加载当前页的资源，浏览器会优先加载它们。不阻塞页面的onload的情况下请求资源。
>
>         ```html
>         <link rel="preload" href="地址">
>         ```
>
>      2. Prefech告诉浏览器这个资源将来可能需要，什么时候加载由浏览器来决定。
>
> + **服务端**
>
>   1. 使用`CDN`服务，来提高用户对于资源请求时的响应速度。
>   2. 服务端启用`Gzip`、`Deflate`等方式对于资源进行压缩，减小文件体积。
>   3. 尽可能减小`Cookie`大小。
>
> + **CSS、Javascript**
>
>   1. 样式表文件放在head中、脚本文件放在页面底部
>   2. 避免使用@import标签
