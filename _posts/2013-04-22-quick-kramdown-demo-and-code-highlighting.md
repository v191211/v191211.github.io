---
layout: post
title: "Kramdown使用示例"
categories: 技术
tags: Jekyll
---

> 2017年5月24日更新

这篇文章用于测试 [Simple Texture][Simple Texture] 主题的 [kramdown][kramdown] 格式。

* Kramdown 使用案例目录
{:toc .toc}

## 常规使用

普通段落。

[一个链接](https://empvalley.com)，链接到我的主页。

带有标题（title）的[链接](https://empvalley.com/blog "晨霜's Blog")。

***斜体并加粗。***

**加粗。**

*斜体。*

**文本加粗。**

脚注[^1]。

## 引用

> ruby -v
>
> tsc -v

### 嵌套引用

> 引用块中的段落。
>
> > 嵌套引用。

### 列表引用

> 无序列表
>
> * 列表一
> * 列表二
> * 列表三
>
> 有序列表
>
> 1. 列表一
> 2. 列表二
> 3. 列表三

### 长文本引用

> Jekyll 是一个用于个人、项目或组织站点，简单的、博客相关的静态站点生成器。
>
> 可以理解为是一个 CMS （内容管理系统），但是没有那么复杂。Jekyll 获取内容，渲染 Markdown 和模板，然后输出完整的、可托管在 Apache、Nginx 或其它网站服务器上的静态网站。
>
> Jekyll 是 GitHub Pages 的引擎，可以托管 GitHub 仓库中的网站。

## 列表

* 列表一 项目一
  * 内嵌列表 项目一
  * 内嵌列表 项目二
  * 内嵌列表 项目三 包含引用

> ruby -v
>
> tsc -v

* 列表一 项目二
* 列表一 项目三

## 表格

* 表格一

    |-----------------+------------+-----------------+----------------|
    | Default aligned |Left aligned| Center aligned  | Right aligned  |
    |-----------------|:-----------|:---------------:|---------------:|
    | First body part |Second cell | Third cell      | fourth cell    |
    | Second line     |foo         | **strong**      | baz            |
    | Third line      |quux        | baz             | bar            |
    | Footer row      |            |                 |                |
    |-----------------+------------+-----------------+----------------|

* 表格二

    |---
    | Default aligned | Left aligned | Center aligned | Right aligned
    |-|:-|:-:|-:
    | First body part | Second cell | Third cell | fourth cell
    | Second line |foo | **strong** | baz
    | Third line |quux | baz | bar
    | Footer row

## 水平线

* * *

---

_  _  _  _

---------------

## 图片

图片来了！

![smiley](https://kramdown.gettalong.org/overview.png)



## 代码

### 代码段

行内代码块  `C:/Ruby23-x64` or `SELECT  "offices".* FROM "offices" `

反引号 `` ` ``
``  `一些`  `` 文本 (注意：使用两个空格，以便能够输出一个空格！)。

Ruby 代码块 `x = Class.new`{:.language-ruby}

### 围栏式代码块

~~~~~~~~~~~~

~~~~~~~
带有波浪符的代码
~~~~~~~~

~~~~~~~~~~~~~~~~~~

### 长代码块

    function myFunction() {
        alert("Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.");
    }

### 代码块语言

~~~ ruby
def what?
  42
end
~~~

## 高亮

### 外部引用

<script src="https://gist.github.com/yizeng/9b871ad619e6dcdcc0545cac3101f361.js"></script>
### 常规高亮

{% highlight ruby %}
def foo
  puts 'foo'
end
{% endhighlight %}

### 长文本高亮

{% highlight c# %}
public class Hello {
    public static void Main() {
        Console.WriteLine("Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.");
    }
}
{% endhighlight %}

### 带有行号的长文本高亮

{% highlight javascript linenos=table %}
function myFunction() {
    alert("Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.");
}
{% endhighlight %}

[^1]: 脚注。

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/simple-texture-jekyll-theme

