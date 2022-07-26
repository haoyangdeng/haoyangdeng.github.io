# Markdown学习


# markdown使用规则
参考资料[Markdown 官方教程](https://markdown.com.cn/basic-syntax/)
## 标题
使用符号井号`#`后面接标题字段，注意`#`和标题间要有空格,多个#代表几级标题，如 `## 标题`就是上面标题的效果。
## 段落
使用空白行将段落进行区分。
## 换行
在一行的末尾添加两个或多个空格，然后按回车键。或者在句子的结尾使用`<br>`来进行换行。
## 强调
使用星号`*`或者下划线`_`来实现字体的加粗和斜体。   
|写法|效果|
|----------|---------|
|`**word**`|**word**|  
|`*word*`|*word*|  
|`***word***`|***word***|
## 块引用
使用大于号`>`来对整句或者整段引用内容加以区分，多个大于号可以进行嵌套
>引用内容
>>引用内容
## 代码引用
使用\`和\`或者\``和\``对内容中引用代码加以区分
## 分隔线
`***`或者`__`或者`---`单独写一行，且前后均添加空白行  
## 链接
```
[超链接显示名](超链接地址 "超链接title")
```  
使用`<>`可以直接将地址内容变为可点击内容  
强调链接, 在链接语法前后增加星号。 要将链接表示为代码，请在方括号中添加反引号。
```
I love supporting the **[EFF](https://eff.org)**.  
This is the *[Markdown Guide](https://www.markdownguide.org)*.  
See the section on [`code`](#code).  
```
渲染如下：
I love supporting the **[EFF](https://eff.org)**.  
This is the *[Markdown Guide](https://www.markdownguide.org)*.  
See the section on [`code`](#code).  
链接中的空格使用`%20`代替以适用不同的Markdown应用程序
## 图片链接
添加图像
```
![这是图片](/assets/img/philly-magic-garden.jpg "Magic Gardens")
```
添加图像链接
```
[![沙漠中的岩石图片](/assets/img/shiprock.jpg "Shiprock")](https://markdown.com.cn)
```
## 转义字符
除了使用`\`来使用转义字符


