Markdown Edit in Sublime
=====================
- *在sublime中配置Markdown editing与Markdown preview*
- *Markdown的格式*

---
### First Note
这算是第一个note了，先来Markdown，原因很简单，之后很多笔记都是要用这个的，还有Sublime，这两个是必须的。

### What is Markdown
本菜鸡也是偶尔看到人家写的README.md非常有意思，然后发现竟然有Markdown这样一个东西，表示颇为感兴趣。Markdown说白了就是一类似与html的标记语言，能使普通文本具有一定的格式显示在page上（实际上还是会将md文件转为html的），如下图，

![md2html](https://github.com/GrapeGun/GrapeNotes/raw/master/EditTools/MarkDown/images/md2html.png)

在使用Markdown preview在浏览器中浏览时，大概是使用了下面红色方框中的一个js，将md转成了html（图中的js是用来解析数学公式的，在这个之前还有一个js，但是太快，总是截不到图）。


### 在Sublime中编辑Markdown
这里就稍微提一下，安装Markdown editing 与Markdown preview即可，具体设置我会在Sublime的[note.md](https://github.com/GrapeGun/GrapeNotes/blob/master/EditTools/Sublime/note.md)再做笔记。
我刚刚接触sublime，感觉比vim好用不少，绝对的编辑神器

---

## 文本格式
这个文档里其实已经用到很多了。
比如 
* 斜体 *我是一个斜体* _我也是一个斜体_
* 粗体 **我是一个粗体** __我也是一个粗体__
* 粗斜体 ***我是一个粗斜体***
* 阴影 `我是一个阴影` 
* 表格 注意第一行第二行的对齐方式以及缩进要小于一个Tab的空格数             

   | col1 | col2 | col3 |
   |------|------|------|
   |aaa | bbb | ccc | 
   |ddd | eee | fff |
   |ggg | hhh | iii |



### 缩进
见[README.md](https://github.com/GrapeGun/GrapeNotes/blob/master/README.md),我用这个做了目录结构。那个符号是<kbd>></kbd>(即Shift+,)
> 缩进一
>> 缩进二

### 标题样式
这个不多说了，最开始就只记住了这个而已。
# Big title (h1)
## Middle title (h2)
### Smaller title (h3)
#### and so on (hX)
##### and so on (hX)
###### and so on (hX)

### 列表
这个也不多说了。

 1. list1
 2. list2
 3. list3

 - bullets can be `-`, `+`, or `*`
 - bullet list 1
 - bullet list 2
    - sub item 1
    - sub item 2

        with indented text inside

 - bullet list 3
 + bullet list 4
 * bullet list 5

### 链接
前面也有过,再如：[baidu with a title](http://www.baidu.com/ "title") ,[baidu without a title](http://www.baidu.com/)

另外，可以使用引用（引用一般放在文档末尾）：[reference emoji][emoji]

### 图片
上面有，不写了，多一点的就是图片的title的方式，类似与上面链接的方法。

### 代码段
这个功能很好，在gfm扩展后的代码高亮很赞。下面是java一个代码段

原始方式 增加缩进即可 高亮不太好使，要设置`enable_highlight`=true

    public `class` HelloWorld() { 
        public static void main(String[] args) {
            System.out.println("HelloWorld!");
        }
    }

GFM方式，可设置语言

```java
public class HelloWorld() {
    public static void main(String[] args) {
        System.out.println("HelloWorld!");
    }
}
```

### 数学公式
这个是必不可少的。(这个东西有点小复杂，明天再搞)

需要设置 `enable_mathjax`=true,

When `enable_mathjax` is `true`, inline math can be included \\(\frac{\pi}{2}\\) $\pi$

Alternatively, math can be written on its own line:

$$F(\omega) = \frac{1}{\sqrt{2\pi}} \int_{-\infty}^{\infty} f(t) \, e^{ - i \omega t}dt$$

\\[\int_0^1 f(t) \mathrm{d}t\\]

\\[\sum_j \gamma_j^2/d_j\\]

## About

感谢 [revolunet team][revolunet] 提供的文档帮助.

 [ref1]: http://revolunet.com
 [ref2]: http://revolunet.com "rich web apps"
 [MarkdownREF]: http://daringfireball.net/projects/markdown/basics
 [MarkdownPreview]: https://github.com/revolunet/sublimetext-markdown-preview
 [ST]: http://sublimetext.com
 [revolunet]: http://revolunet.com
 [revolunet-logo]: http://www.revolunet.com/static/parisjs8/img/logo-revolunet-carre.jpg "revolunet logo"
 [gfm]: http://github.github.com/github-flavored-markdown/
 [emoji]: http://www.emoji-cheat-sheet.com/
 [emoji with title]: http://www.emoji-cheat-sheet.com/

