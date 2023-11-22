---
aliases: [DOM ]
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:54:58
tags: [learn/program/javascript]
title: DOM 
---
# DOM
## 基础知识

> 向军大叔每晚八点在 [抖音 (opens new window)](https://live.douyin.com/houdunren) 和 [bilibli (opens new window)](https://space.bilibili.com/282190994) 直播

操作文档 HTML 的 JS 处理方式为 DOM 即 Document Object Model 文档对象模型。如果对 HTML 很了解使用 DOM 并不复杂。

浏览器在加载页面是会生成 DOM 对象，以供我们使用 JS 控制页面元素。

### 文档渲染

浏览器会将 HTML 文本内容进行渲染，并生成相应的 JS 对象，同时会对不符规则的标签进行处理。

-   浏览器会将标签规范后渲染页面
-   目的一让页面可以正确呈现
-   目的二可以生成统一的 JS 可操作对象

#### 标签修复

在 html 中只有内容`houdunren.com` 而没有任何标签时，通过浏览器的 `检查>元素` 标签查看会自动修复成以下格式的内容

![image-20210830174605406](https://doc.houdunren.com/assets/img/image-20210830174605406.5e42351f.png)

下面 H1 标签结束错误并且属性也没有引号，浏览器在渲染中会进行修复

处理后的结果

#### 表格处理

表格 tabel 中不允许有内容，浏览器在渲染过程中会进行处理

渲染后会添加 tbody 标签并将 table 中的字符移出

#### 标签移动

所有内容要写在 BODY 标签中，下面的 SCRIPT 标签写在了 BODY 后面，浏览器渲染后也会进行处理

渲染后处理的结果

### 操作时机

需要保证浏览器已经渲染了内容才可以读取的节点对象，下例将无法读取到节点对象

不过我们可以将脚本通过事件放在页面渲染完执行

或使用定时器将脚本设置为异步执行

也可以放在文档加载后的事件处理函数中

或将脚本设置在外部文件并使用 defer 属性加载，defer 即会等到 DOM 解析后迟延执行

### 节点对象

JS 中操作 DOM 的内容称为节点对象（node)，即然是对象就包括操作 NODE 的属性和方法

-   包括 12 种类型的节点对象
-   常用了节点为 document、标签元素节点、文本节点、注释节点
-   节点均继承自 Node 类型，所以拥有相同的属性或方法
-   document 是 DOM 操作的起始节点

### 原型链

在浏览器渲染过程中会将文档内容生成为不同的对象，我伙来对下例中的 h1 标签进行讨论，其他节点情况相似

-   不同类型节点由专有的构造函数创建对象
-   使用 console.dir 可以打印出 DOM 节点对象结构
-   节点也是对象所以也具有 JS 对象的特征

最终得到的节点的原型链为

| 原型               | 说明                                                                              |
| ------------------ | --------------------------------------------------------------------------------- |
| Object             | 根对象，提供 hasOwnProperty 等基本对象操作支持                                    |
| EventTarget        | 提供 addEventListener、removeEventListener 等事件支持方法                         |
| Node               | 提供 firstChild、parentNode 等节点操作方法                                        |
| Element            | 提供 getElementsByTagName、querySelector 等方法                                   |
| HTMLElement        | 所有元素的基础类，提供 childNodes、nodeType、nodeName、className、nodeName 等方法 |
| HTMLHeadingElement | Head 标题元素类                                                                   |

我们将上面的方法优化一下，实现提取节点原型链的数组

下面为标题元素增加两个原型方法，改变颜色与隐藏元素

### 对象特征

即然 DOM 与我们其他 JS 创建的对象特征相仿，所以也可以为 DOM 对象添加属性或方法。

对于系统应用的属性，应该明确含义不应该随意使用，比如 ID 是用于标识元素唯一属性，不能用于其他目地

-   后面会讲到其他解决方案，来自定义属性，ID 属性可以直接修改但是不建议这么做

title 用于显示提示文档也不应该用于其他目地

下面是为对象合并属性的示例

使用对象特性更改样式属性

## 常用节点

JS 提供了访问常用节点的 api

| 方法                     | 说明                           |
| ------------------------ | ------------------------------ |
| document                 | document 是 DOM 操作的起始节点 |
| document.documentElement | 文档节点即 html 标签节点       |
| document.body            | body 标签节点                  |
| document.head            | head 标签节点                  |
| document.links           | 超链接集合                     |
| document.anchors         | 所有锚点集合                   |
| document.forms           | form 表单集合                  |
| document.images          | 图片集合                       |

### DOCUMENT

document 是 window 对象的属性，是由 HTMLDocument 类实现的实例。

-   document 包含 DocumentType（唯一）或 html 元素（唯一）或 comment 等元素

原型链中也包含 Node，所以可以使用有关节点操作的方法如 nodeType/NodeName 等

> 有关使用 Document 操作 cookie 与本地储存将会在相应章节中介绍

使用 title 获取和设置文档标题

获取当前 URL

获取域名

获取来源地址

系统针对特定标签提供了快速选择的方式

### ID

下面是直接使用 ID 获取元素（这是非标准操作，对浏览器有挑剔）

### Links

下面展示的是获取所有 a 标签

### Anchors

下例是获取锚点集合后能过 锚点 name 属性获取元素

### Images

下面是获取所有图片节点

## 节点属性

不同类型的节点拥有不同属性，下面是节点属性的说明与示例

### nodeType

nodeType 指以数值返回节点类型

| nodeType | 说明          |
| -------- | ------------- |
| 1        | 元素节点      |
| 2        | 属性节点      |
| 3        | 文本节点      |
| 8        | 注释节点      |
| 9        | document 对象 |

下面是节点 nodeType 的示例

下面是根据指定的 nodeType 递归获取节点元素的示例

-   可获取文本、注释、标签等节点元素

### Prototype

当然也可以使用对象的原型进行检测

-   section 、main、aslide 标签的原型对象为 HTMLElement
-   其他非系统标签的原型对象为 HTMLUnknownElement

下例是通过构建函数获取节点的示例

### nodeName

nodeName 指定节点的名称

-   获取值为大写形式

| nodeType | nodeName       |
| -------- | -------------- |
| 1        | 元素名称如 DIV |
| 2        | 属性名称       |
| 3        | `#text`          |
| 8        | #comment       |

下面来操作 nodeName

### tagName

nodeName 可以获取不限于元素的节点名，tagName 仅能用于获取标签节点的名称

-   tagName 存在于 Element 类的原型中
-   文本、注释节点值为 undefined
-   获取的值为大写的标签名

### nodeValue

使用 nodeValue 或 data 函数获取节点值，也可以使用节点的 data 属性获取节点内容

| nodeType | nodeValue |
| -------- | --------- |
| 1        | null      |
| 2        | 属性值    |
| 3        | 文本内容  |
| 8        | 注释内容  |

下面来看 nodeValue 的示例

使用 data 属性可以获取文本与注释内容

### 树状节点

下面获取标签树状结构即多级标签结构，来加深一下节点的使用

上例结果如下

## 节点集合

Nodelist 与 HTMLCollection 都是包含多个节点标签的集合，大部分功能也是相同的。

-   getElementsBy...等方法返回的是 HTMLCollection
-   querySelectorAll 返回的是 NodeList
-   NodeList 节点列表是动态的，即内容添加后会动态更新

### Length

Nodelist 与 HTMLCollection 包含 length 属性，记录了节点元素的数量

### Item

Nodelist 与 HTMLCollection 提供了 item()方法来根据索引获取元素

使用数组索引获取更方便

### namedItem

HTMLCollection 具有 namedItem 方法可以按 name 或 id 属性来获取元素

也可以使用数组或属性方式获取

数字索引时使用 item 方法，字符串索引时使用 namedItem 或 items 方法

## 动态与静态

通过 getElementsByTagname 等 getElementsBy... 函数获取的 Nodelist 与 HTMLCollection 集合是动态的，即有元素添加或移动操作将实时反映最新状态。

-   使用 getElement...返回的都是动态的集合
-   使用 querySelectorAll 返回的是静态集合

### 动态特性

下例中通过按钮动态添加元素后，获取的元素集合是动态的，而不是上次获取的固定快照。

document.querySelectorAll 获取的集合是静态的

### 使用静态

如果需要保存静态集合，则需要对集合进行复制

## 遍历节点

### forOf

Nodelist 与 HTMLCollection 是类数组的可迭代对象所以可以使用 for...of 进行遍历

### forEach

Nodelist 节点列表也可以使用 forEach 来进行遍历，但 HTMLCollection 则不可以

### call/apply

节点集合对象原型中不存在 map 方法，但可以借用 Array 的原型 map 方法实现遍历

当然也可以使用以下方式操作

### Array.from

Array.from 用于将类数组转为组件，并提供第二个迭代函数。所以可以借用 Array.from 实现遍历

### 展开语法

下面使用点语法转换节点为数组

## 节点关系

节点是父子级嵌套与前后兄弟关系，使用 DOM 提供的 API 可以获取这种关系的元素。

-   文本和注释也是节点，所以也在匹配结果中

### 基础知识

节点是根据 HTML 内容产生的，所以也存在父子、兄弟、祖先、后代等节点关系，下例中的代码就会产生这种多重关系

-   h1 与 ul 是兄弟关系
-   span 与 li 是父子关系
-   ul 与 span 是后代关系
-   span 与 ul 是祖先关系

下面是通过节点关系获取相应元素的方法

| 节点属性        | 说明           |
| --------------- | -------------- |
| childNodes      | 获取所有子节点 |
| parentNode      | 获取父节点     |
| firstChild      | 第一个子节点   |
| lastChild       | 最后一个子节点 |
| nextSibling     | 下一个兄弟节点 |
| previousSibling | 上一个兄弟节点 |

子节点集合与首、尾节点获取

-   文本也是 node 所以也会在匹配当中

下面通过示例操作节点关联

-   文本也是 node 所以也会在匹配当中

document 是顶级节点 html 标签的父节点是 document

### 父节点集合

下例是查找元素的所有父节点

使用递归获取所有父级节点

### 后代节点集合

获取所有的后代元素 SPAN 的内容

## 标签关系

使用 childNodes 等获取的节点包括文本与注释，但这不是我们常用的，为此系统也提供了只操作元素的关系方法。

### 基础知识

下面是处理标签关系的常用 API

| 节点属性               | 说明                                             |
| ---------------------- | ------------------------------------------------ |
| parentElement          | 获取父元素                                       |
| children               | 获取所有子元素                                   |
| childElementCount      | 子标签元素的数量                                 |
| firstElementChild      | 第一个子标签                                     |
| lastElementChild       | 最后一个子标签                                   |
| previousElementSibling | 上一个兄弟标签                                   |
| nextElementSibling     | 下一个兄弟标签                                   |
| contains               | 返回布尔值，判断传入的节点是否为该节点的后代节点 |

以下实例展示怎样通过元素关系获取元素

html 标签的父节点是 document，但父标签节点不存在

### 按类名获取标签

下例是按 className 来获取标签

## 标签获取

系统提供了丰富的选择节点（NODE）的操作方法，下面我们来一一说明。

### getElementById

使用 ID 选择是非常方便的选择具有 ID 值的节点元素，但注意 ID 应该是唯一的

> 只能通过 document 对象上使用

getElementById 只能通过 document 访问，不能通过元素读取拥有 ID 的子元素，下面的操作将产生错误

下面自定义函数来支持批量按 ID 选择元素

拥有 ID 的元素可做为 WINDOW 的属性进行访问

如果声明了变量这种访问方式将无效，所以并不建议使用这种方式访问对象

### getElementsByName

使用 getElementByName 获取设置了 name 属性的元素，虽然在 DIV 等元素上同样有效，但一般用来对表单元素进行操作时使用。

-   返回 NodeList 节点列表对象
-   NodeList 顺序为元素在文档中的顺序
-   需要在 document 对象上使用

### getElementsByTagName

使用 getElementsByTagName 用于按标签名获取元素

-   返回 HTMLCollection 节点列表对象
-   是不区分大小的获取

**通配符**

可以使用通配符 **\*** 获取所有元素

某个元素也可以使用通配置符 **\*** 获取后代元素，下面获取 id 为 houdunren 的所有后代元素

### getElementsByClassName

getElementsByClassName 用于按 class 样式属性值获取元素集合

-   设置多个值时顺序无关，指包含这些 class 属性的元素

下面我们来自己开发一个与 getElementsByClassName 相同的功能函数

## 样式选择器

在 CSS 中可以通过样式选择器修饰元素样式，在 DOM 操作中也可以使用这种方式查找元素。使用过 jQuery 库的朋友，应该对这种选择方式印象深刻。

使用 getElementsByTagName 等方式选择元素不够灵活，建议使用下面的样式选择器操作，更加方便灵活

### querySelectorAll

使用 querySelectorAll 根据 CSS 选择器获取 Nodelist 节点列表

-   获取的 NodeList 节点列表是静态的，添加或删除元素后不变

获取所有 div 元素

获取 id 为 app 元素的，class 为 houdunren 的后代元素

根据元素属性值获取元素集合

再来看一些通过样式选择器查找元素

### querySelector

querySelector 使用 CSS 选择器获取一个元素，下面是根据属性获取单个元素

### Matches

用于检测元素是否是指定的样式选择器匹配，下面过滤掉所有 name 属性的 LI 元素

### Closest

查找最近的符合选择器的祖先元素（包括自身），下例查找父级拥有 `.comment`类的元素

## 标准属性

元素的标准属性具有相对应的 DOM 对象属性

-   操作属性区分大小写
-   多个单词属性命名规则为第一个单词小写，其他单词大写
-   属性值是多类型并不全是字符串，也可能是对象等
-   事件处理程序属性值为函数
-   style 属性为 CSSStyleDeclaration 对象
-   DOM 对象不同生成的属性也不同

### 属性别名

有些属性名与 JS 关键词冲突，系统已经起了别名

| 属性  | 别名      |
| ----- | --------- |
| class | className |
| for   | htmlFor   |

### 操作属性

元素的标准属性可以直接进行操作，下面是直接设置元素的 className

下面设置图像元素的标准属性

使用 hidden 隐藏元素

### 多类型

大部分属性值是都是字符串，但并不是全部，下例中需要转换为数值后进行数据运算

下面表单 checked 属性值为 Boolean 类型

属性值并都与 HTML 定义的值一样，下面返回的 href 属性值是完整链接

## 元素特征

对于标准的属性可以使用 DOM 属性的方式进行操作，但对于标签的非标准的定制属性则不可以。但 JS 提供了方法来控制标准或非标准的属性

可以理解为元素的属性分两个地方保存，DOM 属性中记录标准属性，特征中记录标准和定制属性

-   使用特征操作时属性名称不区分大小写
-   特征值都为字符串类型

| 方法            | 说明     |
| --------------- | -------- |
| getAttribute    | 获取属性 |
| setAttribute    | 设置属性 |
| removeAttribute | 删除属性 |
| hasAttribute    | 属性检测 |

特征是可迭代对象，下面使用 for...of 来进行遍历操作

属性值都为字符串，所以数值类型需要进行转换

使用 removeAttribute 删除元素的 class 属性，并通过 hasAttribute 进行检测删除结果

特征值与 HTML 定义是一致的，这和属性值是不同的

### Attributes

元素提供了 attributes 属性可以只读的获取元素的属性

### 自定义特征

虽然可以随意定义特征并使用 getAttribute 等方法管理，但很容易造成与标签的现在或未来属性重名。建议使用以 data-为前缀的自定义特征处理，针对这种定义方式 JS 也提供了接口方便操作。

-   元素中以 data-为前缀的属性会添加到属性集中
-   使用元素的 dataset 可获取属性集中的属性
-   改变 dataset 的值也会影响到元素上

下面演示使用属性集设置 DIV 标签内容

多个单词的特征使用驼峰命名方式读取

改变 dataset 值也会影响到页面元素上

### 属性同步

特征和属性是记录元素属性的两个不同场所，大部分更改会进行同步操作。

下面使用属性更改了 className，会自动同步到了特征集中，反之亦然

下面对 input 值使用属性设置，但并没有同步到特征

但改变 input 的特征 value 会同步到 DOM 对象属性

## 创建节点

创建节点的就是构建出 DOM 对象，然后根据需要添加到其他节点中

### Append

append 也是用于添加元素，同时他也可以直接添加文本等内容。

### createTextNode

创建文本对象并添加到元素中

### createElement

使用 createElement 方法可以标签节点对象，创建 span 标签新节点并添加到 div#app

使用 PROMISE 结合节点操作来加载外部 JAVASCRIPT 文件

使用同样的逻辑来实现加载 CSS 文件

### cloneNode&importNode

使用 cloneNode 和 document.importNode 用于复制节点对象操作

-   cloneNode 是节点方法
-   cloneNode 参数为 true 时递归复制子节点即深拷贝
-   importNode 是 documet 对象方法

复制 div#app 节点并添加到 body 元素中

document.importNode 方法是部分 IE 浏览器不支持的，也是复制节点对象的方法

-   第一个参数为节点对象
-   第二个参数为 true 时递归复制

## 节点内容

### innerHTML

inneHTML 用于向标签中添加 html 内容，同时触发浏览器的解析器重绘 DOM。

下例使用 innerHTML 获取和设置 div 内容

-   innerHTML 中只解析 HTML 标签语法，所以其中的 script 不会做为 JS 处理

**重绘节点**

使用 innertHTML 操作会重绘元素，下面在点击第二次就没有效果了

-   因为对#app 内容进行了重绘，即删除原内容然后设置新内容
-   重绘后产生的 button 对象没有事件
-   重绘后又产生了新 img 对象，所以在控制台中可看到新图片在加载

### outerHTML

outerHTML 与 innerHTML 的区别是包含父标签

-   outerHTML 不会删除原来的旧元素
-   只是用新内容替换替换旧内容，旧内容（元素）依然存在

下面将 div#app 替换为新内容

使用 innerHTML 内容是被删除然后使用新内容

而使用 outerHTML 是保留旧内容，页面中使用新内容

### textContent 与 innerText

textContent 与 innerText 是访问或添加文本内容到元素中

-   textContentb 部分 IE 浏览器版本不支持
-   innerText 部分 FireFox 浏览器版本不支持
-   获取时忽略所有标签,只获取文本内容
-   设置时将内容中的标签当文本对待不进行标签解析

获取时忽略内容中的所有标签

设置时将标签当文本对待，即转为 HTML 实体内容

### outerText

与 innerText 差别是会影响所操作的标签

### insertAdjacentText

将文本插入到元素指定位置，不会对文本中的标签进行解析，包括以下位置

| 选项        | 说明         |
| ----------- | ------------ |
| beforebegin | 元素本身前面 |
| afterend    | 元素本身后面 |
| afterbegin  | 元素内部前面 |
| beforeend   | 元素内部后面 |

添加文本内容到 div#app 前面

## 节点管理

现在我们来讨论下节点元素的管理，包括添加、删除、替换等操作

### 推荐方法

| 方法        | 说明                       |
| ----------- | -------------------------- |
| append      | 节点尾部添加新节点或字符串 |
| prepend     | 节点开始添加新节点或字符串 |
| before      | 节点前面添加新节点或字符串 |
| after       | 节点后面添加新节点或字符串 |
| replaceWith | 将节点替换为新节点或字符串 |

在标签内容后面添加新内容

同时添加多个内容，包括字符串与元素标签

将标签替换为新内容

添加新元素 h1 到目标元素 div#app 里面

将 h2 移动到 h1 之前

使用 remove 方法可以删除节点

### insertAdjacentHTML

将 html 文本插入到元素指定位置，浏览器会对文本进行标签解析，包括以下位置

| 选项        | 说明         |
| ----------- | ------------ |
| beforebegin | 元素本身前面 |
| afterend    | 元素本身后面 |
| afterbegin  | 元素内部前面 |
| beforeend   | 元素内部后面 |

在 div#app 前添加 HTML 文本

### insertAdjacentElement

insertAdjacentElement() 方法将指定元素插入到元素的指定位置，包括以下位置

-   第一个参数是位置
-   第二个参数为新元素节点

| 选项        | 说明         |
| ----------- | ------------ |
| beforebegin | 元素本身前面 |
| afterend    | 元素本身后面 |
| afterbegin  | 元素内部前面 |
| beforeend   | 元素内部后面 |

在 div#app 前插入 span 标签

### 古老方法

下面列表过去使用的操作节点的方法，现在不建议使用了。但在阅读老代码时可来此查看语法

| 方法         | 说明                           |
| ------------ | ------------------------------ |
| appendChild  | 添加节点                       |
| insertBefore | 用于插入元素到另一个元素的前面 |
| removeChild  | 删除节点                       |
| replaceChild | 进行节点的替换操作             |

### DocumentFragment

当对节点进行添加、删除等操作时，都会引起页面回流来重新渲染页面,即重新渲染颜色，尺寸，大小、位置等等。所以会带来对性能的影响。

**解决以上问题可以使用以下几种方式**

1.  可以将 DOM 写成 html 字符串，然后使用 innerHTML 添加到页面中，但这种操作会比较麻烦，且不方便使用节点操作的相关方法。
    
2.  使用 createDocumentFragment 来管理节点时，此时节点都在内存中，而不是 DOM 树中。对节点的操作不会引发页面回流,带来比较好的性能体验。
    

**DocumentFragment 特点**

-   createDocumentFragment 父节点为 null
-   继承自 node 所以可以使用 NODE 的属性和方法
-   createDocumentFragment 创建的是文档碎片，节点类型 nodeType 为 11。因为不在 DOM 树中所以只能通过 JS 进行操作
-   添加 createDocumentFragment 添加到 DOM 后,就不可以再操作 createDocumentFragment 元素了,这与 DOM 操作是不同的
-   将文档 DOM 添加到 createDocumentFragment 时,会移除文档中的 DOM 元素
-   createDocumentFragment 创建的节点添加到其他节点上时，会将子节点一并添加
-   createDocumentFragment 是虚拟节点对象，不直接操作 DOM 所以性能更好
-   在排序/移动等大量 DOM 操作时建议使用 createDocumentFragment

## 表单控制

表单是高频操作的元素，下面来掌握表单项的 DOM 操作

### 表单查找

JS 为表单的操作提供了单独的集合控制

-   使用 document.forms 获取表单集合
-   使用 form 的 name 属性获取指定 form 元素
-   根据表单项的 name 属性使用 form.elements.title 获取表单项，
-   也可以直接写成 form.name 形式，不需要 form.elements.title
-   针对 radio/checkbox 获取的表单项是一个集合

通过表单项可以反向查找 FORM

## 样式管理

通过 DOM 修改样式可以通过更改元素的 class 属性或通过 style 对象设置行样式来完成。

-   建议使用 class 控制样式，将任务交给 CSS 处理，更简单高效

### 批量设置

使用 JS 的 className 可以批量设置样式

也可以通过特征的方式来更改

### classList

如果对类单独进行控制使用 classList 属性操作

| 方法                    | 说明     |
| ----------------------- | -------- |
| node.classList.add      | 添加类名 |
| node.classList.remove   | 删除类名 |
| node.classList.toggle   | 切换类名 |
| node.classList.contains | 类名检测 |

在元素的原有 class 上添加新 class

使用 classList 也可以移除 class 列表中的部分 class

使用 toggle 切换类，即类已经存在时删除，不存在时添加

使用 contains 检查 class 是否存在

### 设置行样式

使用 style 对象可以对样式属性单独设置，使用 cssText 可以批量设置行样式

**样式属性设置**

使用节点的 style 对象来设置行样式

-   多个单词的属性使用驼峰进行命名

**批量设置行样式**

使用 cssText 属性可以批量设置行样式，属性名和写 CSS 一样不需要考虑驼峰命名

也可以通过 setAttribute 改变 style 特征来批量设置样式

### 获取样式

可以通过 style 对象，window.window.getComputedStyle 对象获取样式属性，下面进行说明

**style**

可以使用 DOM 对象的 style 属性读取行样式

-   style 对象不能获取行样式外定义的样式

**getComputedStyle**

使用 window.getComputedStyle 可获取所有应用在元素上的样式属性

-   函数第一个参数为元素
-   第二个参数为伪类
-   这是计算后的样式属性，所以取得的单位和定义时的可能会有不同
