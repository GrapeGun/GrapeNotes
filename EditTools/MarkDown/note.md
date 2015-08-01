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

在使用Markdown preview在浏览器中浏览时，大概是使用了下面红色方框中的一个js，将md转成了html。


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



## GitHub Flavored Markdown

If you use the Github parser, you can use some of [Github Flavored Markdown][gfm] syntax :

 * User/Project@SHA: revolunet/sublimetext-markdown-preview@7da61badeda468b5019869d11000307e07e07401
 * User/Project#Issue: revolunet/sublimetext-markdown-preview#1
 * User : @revolunet

Some Python code :

```python
import random

class CardGame(object):
    """ a sample python class """
    NB_CARDS = 32
    def __init__(self, cards=5):
        self.cards = random.sample(range(self.NB_CARDS), 5)
        print 'ready to play'
```

Some Javascript code :

```js
var config = {
    duration: 5,
    comment: 'WTF'
}
// callbacks beauty un action
async_call('/path/to/api', function(json) {
    another_call(json, function(result2) {
        another_another_call(result2, function(result3) {
            another_another_another_call(result3, function(result4) {
                alert('And if all went well, i got my result :)');
            });
        });
    });
})
```

The Github Markdown also brings some [nice Emoji support][emoji] : :+1: :heart: :beer:

[^note-id]: This is the text of the note. 

## Parsers and Extensions

Markdown Preview comes with **Python-Markdown** preloaded.

### *Python-Markdown*

The [Python-Markdown Parser][] provides support for several extensions.

[Python-Markdown Parser]: https://github.com/waylan/Python-Markdown

#### Extra Extensions

* `abbr` -- [Abbreviations][]
* `attr_list` -- [Attribute Lists][]
* `def_list` -- [Definition Lists][]
* `fenced_code` -- [Fenced Code Blocks][]
* `footnotes` -- [Footnotes][]
* `tables` -- [Tables][]
* `smart_strong` -- [Smart Strong][]

[Abbreviations]: http://pythonhosted.org/Markdown/extensions/abbreviations.html
[Attribute Lists]: http://pythonhosted.org/Markdown/extensions/attr_list.html
[Definition Lists]: http://pythonhosted.org/Markdown/extensions/definition_lists.html
[Fenced Code Blocks]: http://pythonhosted.org/Markdown/extensions/fenced_code_blocks.html
[Footnotes]: http://pythonhosted.org/Markdown/extensions/footnotes.html
[Tables]: http://pythonhosted.org/Markdown/extensions/tables.html
[Smart Strong]: http://pythonhosted.org/Markdown/extensions/smart_strong.html


You can enable them all at once using the `extra` keyword.

    extensions: [ 'extra' ]

If you want all the extras plus the `toc` extension,
your settings would look like this:

    {
        ...
        parser: 'markdown',
        extensions: ['extra', 'toc'],
        ...
    }


#### Other Extensions

There are also some extensions that are not included in Markdown Extra
but come in the standard Python-Markdown library.

* `code-hilite` -- [CodeHilite][]
* `html-tidy` -- [HTML Tidy][]
* `header-id` -- [HeaderId][]
* `meta_data` -- [Meta-Data][]
* `nl2br` -- [New Line to Break][]
* `rss` -- [RSS][]
* `sane_lists` -- [Sane Lists][]
* `smarty` -- [Smarty][]
* `toc` -- [Table of Contents][]
* `wikilinks` -- [WikiLinks][]

[CodeHilite]:  http://pythonhosted.org/Markdown/extensions/code_hilite.html
[HTML Tidy]:  http://pythonhosted.org/Markdown/extensions/html_tidy.html
[HeaderId]:  http://pythonhosted.org/Markdown/extensions/header_id.html
[Meta-Data]:  http://pythonhosted.org/Markdown/extensions/meta_data.html
[New Line to Break]:  http://pythonhosted.org/Markdown/extensions/nl2br.html
[RSS]:  http://pythonhosted.org/Markdown/extensions/rss.html
[Sane Lists]:  http://pythonhosted.org/Markdown/extensions/sane_lists.html
[Table of Contents]:  http://pythonhosted.org/Markdown/extensions/toc.html
[WikiLinks]:  http://pythonhosted.org/Markdown/extensions/wikilinks.html
[Smarty]: https://pythonhosted.org/Markdown/extensions/smarty.html

#### 3rd Party Extensions

*Python-Markdown* is designed to be extended.

Some included ones are:

* `delete` -- github style delte support via `~~word~~`
* `githubemoji` --  github emoji support
* `tasklist` -- github style tasklists
* `magiclink` -- github style auto link conversion of http|ftp links
* `headeranchor` -- github style header anchor links
* `github` -- Adds the above extensions in one shot
* `b64` -- convert and embed local images to base64.  Setup by adding this `b64(base_path=${BASE_PATH})`

There are also a number of others available:

Just fork this repo and add your extensions inside the `.../Packages/Markdown Preview/markdown/extensions/` folder.

Check out the list of [3rd Party extensions](
https://github.com/waylan/Python-Markdown/wiki/Third-Party-Extensions).


#### Default Extensions

The default extensions are:

* `footnotes` -- [Footnotes]
* `toc` -- [Table of Contents]
* `fenced_code` -- [Fenced Code Blocks] 
* `tables` -- [Tables]

Use the `default` keyword, to select them all.
If you want all the defaults plus the `definition_lists` extension,
your settings would look like this:

    {
        ...
        parser: 'markdown',
        extensions: ['default', 'definition_lists'],
        ...
    }

## Examples

### Tables

The `tables` extension of the *Python-Markdown* parser is activated by default,
but is currently **not** available in *Markdown2*.

The syntax was adopted from the [php markdown project](http://michelf.ca/projects/php-markdown/extra/#table),
and is also used in github flavoured markdown.

| Year | Temperature (low) | Temperature (high) |  
| ---- | ----------------- | -------------------|  
| 1900 |               -10 |                 25 |  
| 1910 |               -15 |                 30 |  
| 1920 |               -10 |                 32 |  


### Wiki Tables

If you are using *Markdown2* with the `wiki-tables` extra activated you should see a table below:

|| *Year* || *Temperature (low)* || *Temperature (high)* ||  
||   1900 ||                 -10 ||                   25 ||  
||   1910 ||                 -15 ||                   30 ||  
||   1920 ||                 -10 ||                   32 ||  


### Definition Lists

This example requires *Python Markdown*'s `def_list` extension.

Apple
:   Pomaceous fruit of plants of the genus Malus in 
    the family Rosaceae.

Orange
:   The fruit of an evergreen tree of the genus Citrus.


## About

This plugin and this sample file is proudly brought to you by the [revolunet team][revolunet]

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

