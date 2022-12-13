---
layout: post
title: "为什么要进行 URL 编码"
categories: ["技术"]
tags: ["URL"]
---

* 目录
{:toc .toc}
## 不编码 URL 引出的问题

我们都知道 HTTP 协议中参数的传输是 `key=value` 这种简直对形式的，如果要传多个参数就需要用 `&` 符号对键值对进行分割。如 `?name1=value1&name2=value2`，这样在服务端在收到这种字符串的时候，会用 `&` 分割出每一个参数，然后再用 `=` 来分割出参数值。[^1][^2]

针对 `name1=value1&name2=value2` 我们来说一下客户端到服务端的概念上解析过程: 

上述字符串在计算机中用 ASCII 码表示为： 
**6E616D6531 3D 76616C756531 26 6E616D6532 3D 76616C756532。**

**6E616D6531：name1**

**3D：=**

**76616C756531：value1**

**26：&**

**6E616D6532：name2**

**3D：=**

**76616C756532：value2**

服务端在接收到该数据后就可以遍历该字节流，首先一个字节一个字节的吃，当吃到 3D 这字节后，服务端就知道前面吃得字节表示一个 key，再向后吃，如果遇到 26，说明从刚才吃的 3D 到 26 字节之间的是上一个 key 的 value，以此类推就可以解析出客户端传过来的参数。

现在有这样一个问题，如果我的参数值中就包含 `=` 或 `&` 这种特殊字符的时候该怎么办。 

比如说 `name1=value1`,其中 value1 的值是 `va&lu=e1` 字符串，那么实际在传输过程中就会变成这样 `name1=va&lu=e1`。我们的本意是就只有一个键值对，但是服务端会解析成两个键值对，这样就产生了歧义。

## 解决方案

**如何解决上述问题带来的歧义呢？解决的办法就是对参数进行 URL 编码。**

URL 编码只是简单的在特殊字符的各个字节前加上 %，例如，我们对上述会产生奇异的字符进行 URL 编码后结果：`name1=va%26lu%3D`，这样服务端会把紧跟在 `%` 后的字节当成普通的字节，就是不会把它当成各个参数或键值对的分隔符。

另外一个问题，就是为什么我们要用 ASCII 传输，可不可以用别的编码？ 

当然可以用别的编码，你自己可以开发一套编码，然后自己解析。就像大部分国家都有自己的语言一样。那国家之间要交流，怎么办？ 用英语吧，英语的使用范围最广。

通常如果一样东西需要编码，说明这样东西并不适合传输。原因多种多样，如 Size 过大，包含隐私数据，对于 URL 来说，之所以要进行编码，是因为 URL 中有些字符会引起歧义。

例如，URL 参数字符串中使用 `key=value` 键值对这样的形式来传参，键值对之间以 `&` 符号分隔，如 `/s?q=abc&ie=utf-8`。如果你的 value 字符串中包含了 `=` 或者 `&`，那么势必会造成接收 URL 的服务器解析错误，因此必须将引起歧义的 `&` 和 `=` 符号进行转义，也就是对其进行编码。

又如，URL 的编码格式采用的是 ASCII 码，而不是 Unicode，这也就是说你不能在 URL 中包含任何非 ASCII 字符，例如中文。否则如果客户端浏览器和服务端浏览器支持的字符集不同的情况下，中文可能会造成问题。

URL 编码的原则就是使用安全的字符（没有特殊用途或者特殊意义的可打印字符）去表示那些不安全的字符。

预备知识：URI 是统一资源标识的意思，通常我们所说的 URL 只是 URI 的一种。典型 URL 的格式如下所示。下面提到的 URL 编码，实际上应该指的是 URI 编码。

```
foo://example.com:8042/over/there?name=ferret#nose
\_/  \______________/ \________/ \_________/  \__/
 |          |              |          |        |
scheme  authority         path      query   fragment
```

## 哪些字符需要编码

RFC3986 文档规定，URL 中只允许包含英文字母（a-zA-Z）、数字（0-9）、-_.~ 4个特殊字符以及所有保留字符。RFC3986 文档对 URL 的编解码问题做出了详细的建议，指出了哪些字符需要被编码才不会引起 URL 语义的转变，以及对为什么这些字符需要编码做出了相应的解释。

US-ASCII 字符集中没有对应的可打印字符：URL 中只允许使用可打印字符。US-ASCII 码中的 10-7F 字节全都表示控制字符，这些字符都不能直接出现在 URL 中。同时，对于 80-FF 字节（ISO-8859-1），由于已经超出了 US-ACII 定义的字节范围，因此也不可以放在 URL 中。

保留字符：URL 可以划分成若干个组件，协议、主机、路径等。有一些字符（:/?#[]@）是用作分隔不同组件的。例如：冒号用于分隔协议和主机，/ 用于分隔主机和路径，? 用于分隔路径和查询参数，等等。还有一些字符（!$&'()*+,;=）用于在每个组件中起到分隔作用的，如 = 用于表示查询参数中的键值对，& 符号用于分隔查询多个键值对。当组件中的普通数据包含这些特殊字符时，需要对其进行编码。

RFC3986 中指定了以下字符为保留字符：! * ' ( ) ; : @ & = + $ , / ? # [ ]

不安全字符：还有一些字符，当他们直接放在 URL 中的时候，可能会引起解析程序的歧义。这些字符被视为不安全字符，原因有很多。

- 空格：URL 在传输的过程，或者用户在排版的过程，或者文本处理程序在处理 URL 的过程，都有可能引入无关紧要的空格，或者将那些有意义的空格给去掉。
- 引号以及 <>：引号和尖括号通常用于在普通文本中起到分隔 URL 的作用。
- \#：通常用于表示书签或者锚点。
- %：百分号本身用作对不安全字符进行编码时使用的特殊字符，因此本身需要编码。
- {}|\^[]`~：某一些网关或者传输代理会篡改这些字符。

需要注意的是，对于 URL 中的合法字符，编码和不编码是等价的，但是对于上面提到的这些字符，如果不经过编码，那么它们有可能会造成 URL 语义的不同。因此对于 URL 而言，只有普通英文字符和数字，特殊字符 $-_.+!*'() 还有保留字符，才能出现在未经编码的 URL 之中。其他字符均需要经过编码之后才能出现在 URL 中。

但是由于历史原因，目前尚存在一些不标准的编码实现。例如对于 ~ 符号，虽然 RFC3986 文档规定，对于波浪符号 ~，不需要进行 URL 编码，但是还是有很多老的网关或者传输代理会进行编码。

## 如何对 URL 中的非法字符进行编码

URL 编码通常也被称为百分号编码（URL Encoding，also known as percent-encoding），是因为它的编码方式非常简单，使用 % 百分号加上两位的字符——0123456789ABCDEF —— 代表一个字节的十六进制形式。URL 编码默认使用的字符集是 US-ASCII。例如 a 在 US-ASCII 码中对应的字节是 0x61，那么 URL 编码之后得到的就是 %61，我们在地址栏上输入http://g.cn/search?q=%61%62%63，实际上就等同于在 google 上搜索 abc 了。又如 @ 符号在 ASCII 字符集中对应的字节为 0x40，经过 URL 编码之后得到的是 %40。

对于非 ASCII 字符，需要使用 ASCII 字符集的超集进行编码得到相应的字节，然后对每个字节执行百分号编码。对于 Unicode 字符，RFC 文档建议使用 utf-8 对其进行编码得到相应的字节，然后对每个字节执行百分号编码。如"中文"使用 UTF-8 字符集得到的字节为 0xE4 0xB8 0xAD 0xE6 0x96 0x87，经过 URL 编码之后得到"%E4%B8%AD%E6%96%87"。

如果某个字节对应着 ASCII 字符集中的某个非保留字符，则此字节无需使用百分号表示。例如"URL编码"，使用 UTF-8 编码得到的字节是 0x55 0x72 0x6C 0xE7 0xBC 0x96 0xE7 0xA0 0x81，由于前三个字节对应着 ASCII 中的非保留字符"URL"，因此这三个字节可以用非保留字符"URL"表示。最终的 URL 编码可以简化成"URL%E7%BC%96%E7%A0%81" ，当然，如果你用"%55%72%6C%E7%BC%96%E7%A0%81"也是可以的。

由于历史的原因，有一些 URL 编码实现并不完全遵循这样的原则，下面会提到。

## Javascript 中 escape, encodeURI 和 encodeURIComponent 区别

JavaScript 中提供了3对函数用来对 URL 编码以得到合法的 URL，它们分别是 escape / unescape, encodeURI / decodeURI，encodeURIComponent / decodeURIComponent。由于解码和编码的过程是可逆的，因此这里只解释编码的过程。

这三个编码的函数 ——escape，encodeURI，encodeURIComponent—— 都是用于将不安全不合法的 URL 字符转换为合法的 URL 字符表示，它们有以下几个不同点。

安全字符不同：

下面列出了这三个函数的安全字符（即函数不会对这些字符进行编码）

- escape（69个）：*/@+-._0-9a-zA-Z
- encodeURI（82个）：!#$&'()*+,/:;=?@-._~0-9a-zA-Z
- encodeURIComponent（71个）：!'()*-._~0-9a-zA-Z

兼容性不同：escape 函数是从 Javascript 1.0 的时候就存在了，其他两个函数是在 Javascript 1.5 才引入的。但是由于 Javascript 1.5 已经非常普及了，所以实际上使用 encodeURI 和 encodeURIComponent 并不会有什么兼容性问题。

对 Unicode 字符的编码方式不同：这三个函数对于 ASCII 字符的编码方式相同，均是使用百分号 + 两位十六进制字符来表示。但是对于 Unicode 字符，escape 的编码方式是 %uxxxx，其中的 xxxx 是用来表示 unicode 字符的4位十六进制字符。这种方式已经被 W3C 废弃了。但是在 ECMA-262 标准中仍然保留着 escape 的这种编码语法。encodeURI 和 encodeURIComponent 则使用 UTF-8 对非 ASCII 字符进行编码，然后再进行百分号编码。这是 RFC 推荐的。因此建议尽可能的使用这两个函数替代 escape 进行编码。

适用场合不同：encodeURI 被用作对一个完整的 URI 进行编码，而 encodeURIComponent 被用作对URI的一个组件进行编码。从上面提到的安全字符范围表格来看，我们会发现，encodeURIComponent 编码的字符范围要比 encodeURI 的大。我们上面提到过，保留字符一般是用来分隔 URI 组件（一个 URI 可以被切割成多个组件，参考预备知识一节）或者子组件（如 URI 中查询参数的分隔符），如：号用于分隔 scheme 和主机，?号用于分隔主机和路径。由于 encodeURI 操纵的对象是一个完整的的 URI，这些字符在 URI 中本来就有特殊用途，因此这些保留字符不会被 encodeURI 编码，否则意义就变了。

组件内部有自己的数据表示格式，但是这些数据内部不能包含有分隔组件的保留字符，否则就会导致整个 URI 中组件的分隔混乱。因此对于单个组件使用encodeURIComponent，需要编码的字符就更多了。

## 表单提交

当 HTML 的表单被提交时，每个表单域都会被 URL 编码之后才在被发送。由于历史的原因，表单使用的 URL 编码实现并不符合最新的标准。例如对于空格使用的编码并不是 %20，而是 + 号，如果表单使用的是 Post 方法提交的，我们可以在 HTTP 头中看到有一个 Content-Type 的 header，值为 application/x-www-form-URLencoded。大部分应用程序均能处理这种非标准实现的 URL 编码，但是在客户端 Javascript 中，并没有一个函数能够将 + 号解码成空格，只能自己写转换函数。还有，对于非 ASCII 字符，使用的编码字符集取决于当前文档使用的字符集。例如我们在 HTML 头部加上

```html
<meta http-equiv="Content-Type" content="text/html; charset=gb2312" />
```

这样浏览器就会使用 gb2312 去渲染此文档（注意，当 HTML 文档中没有设置此 meta 标签，则浏览器会根据当前用户喜好去自动选择字符集，用户也可以强制当前网站使用某个指定的字符集）。当提交表单时，URL 编码使用的字符集就是 gb2312。

之前在使用 Aptana（为什么专指 aptana 下面会提到）遇到一个很迷惑的问题，就是在使用 encodeURI 的时候，发现它编码得到的结果和我想的很不一样。下面是我的示例代码：


```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=gb2312" />
    </head>
    <body>
        <script type="text/javascript">
            document.write(encodeURI("中文"));
        </script>
    </body>
</html>
```

运行结果输出 %E6%B6%93%EE%85%9F%E6%9E%83。显然这并不是使用 UTF-8 字符集进行 URL 编码得到的结果（在 Google 上搜索"中文"，URL 中显示的是 %E4%B8%AD%E6%96%87）。

所以我当时就很质疑，难道 encodeURI 还跟页面编码有关，但是我发现，正常情况下，如果你使用 gb2312 进行 URL 编码也不会得到这个结果的才是。后来终于被我发现，原来是页面文件存储使用的字符集和 Meta 标签中指定的字符集不一致导致的问题。Aptana 的编辑器默认情况下使用 UTF-8 字符集。也就是说这个文件实际存储的时候使用的是 UTF-8 字符集。但是由于 Meta 标签中指定了 gb2312，这个时候，浏览器就会按照 gb2312 去解析这个文档，那么自然在"中文"这个字符串这里就会出错，因为"中文"字符串用 UTF-8 编码过后得到的字节是 0xE4 0xB8 0xAD 0xE6 0x96 0x87，这6个字节又被浏览器拿 gb2312 去解码，那么就会得到另外三个汉字"涓枃"（GBK 中一个汉字占两个字节），这三个汉字在传入 encodeURI 函数之后得到的结果就是 %E6%B6%93%EE%85%9F%E6%9E%83。因此，encodeURI 使用的还是 UTF-8，并不会受到页面字符集的影响。

对于包含中文的 URL 的处理问题，不同浏览器有不同的表现。例如对于 IE，如果你勾选了高级设置"总是以 UTF-8 发送 URL"，那么 URL 中的路径部分的中文会使用 UTF-8 进行 URL 编码之后发送给服务端，而查询参数中的中文部分使用系统默认字符集进行 URL 编码。为了保证最大互操作性，建议所有放到 URL 中的组件全部显式指定某个字符集进行 URL 编码，而不依赖于浏览器的默认实现。

另外，很多 HTTP 监视工具或者浏览器地址栏等在显示 URL 的时候会自动将 URL 进行一次解码（使用 UTF-8 字符集），这就是为什么当你在 Firefox 中访问Google 搜索中文的时候，地址栏显示的 URL 包含中文的缘故。但实际上发送给服务端的原始 URL 还是经过编码的。你可以在地址栏上使用 Javascript 访问location.href 就可以看出来了。在研究 URL 编解码的时候千万别被这些假象给迷惑了。




[^1]: [为什么要进行URL编码](https://www.cnblogs.com/jerrysion/p/5522673.html){:target="_blank"}

[^2]: [什么是 URL 编码以及工作原理](https://www.URLencoder.io/learn/){:target="_blank"}
