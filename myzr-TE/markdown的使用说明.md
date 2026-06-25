输入[TOC]可以自动导出目录

[TOC]

# markdown的使用说明

## 一、标题

语法：#（一级标题）##（二级标题） ###（三级标题）

快捷键：

ctrl+数字1-6

ctrl+0 转换为普通文本

ctrl+ 加号或者减号

## 二、段落

### 1、换行

1）直接按键盘回车键

2）按shift+enter 小换行

3）Ctrl+Enter（仅限在代码块中）

4）Ctrl+] 增加缩进

### 2、分割线

---或者***（回车）

---

## 三、文字显示

### 1、字体（格式中可以看快捷方式）

粗体：用**包裹  

删除线：用~~包裹

下划线：用<u>标签包裹（Ctrl+U）

斜体：用*包裹（Ctrl+I）

高亮：用==包裹

tip：\转义\*

### 2、上下标

上标用^包裹

下标用~包裹

## 四、列表

### 1、无序列表

Ctrl+Shift+]

- 一级分类	
  - 二级分类  按tab键
    - 三级分类   按tab键

### 2、有序列表

Ctrl+Shift+[

1. 第一个
2. 第二个
	- 子内容1 
3. 连续按Enter键回到一级标题

### 3、任务列表

- [ ] 

> 代码：
>
> `- [空格] 空格`

> 效果：
>
> - [ ] 吃早餐

### 4、区块显示

>代码：
>
>`>+回车`或者Ctrl+Shfit+Q

## 五、代码显示

### 1、行内代码

> 代码：
>
> ```text
> `int a=0;`
> ```

### 2、代码块

> 代码：
>
> ```python
> printf"hello world!"
> ```

## 六、链接

> 代码：
>
> ```text
> 直接放网址
> [百度一下](放网址)
> ```

>效果：
>
>[ColorSpace - Color Palettes Generator and Color Gradient Tool](https://mycolor.space/)
>
>[百度一下]([ColorSpace - Color Palettes Generator and Color Gradient Tool](https://mycolor.space/))

点击跳转需ctrl+点击

[title](##一、标题)

[跳转](### 2、有序列表)



[click](##四、列表)

## 七、脚注

>```text
>[^文本]
>[^文本]：解释说明
>```

hello[^2]

[^2]:to greet.

> 效果：
>
> 这是一个技术[^1]
>
> [^1]:nihao<br>nihao 

## 八、图片插入

> 代码：
>
> ```text
> ![不显示的文字](图片路径"图片标题”)
> <img src="C:\Users\24012\Desktop\picture\0b86b95c0e37249fea57516d3fd1f11.jpg" alt="aaa" style="zoom:50%;" align="right"/>
> ```

> 效果：
>
> <img src="C:\Users\24012\Desktop\picture\0b86b95c0e37249fea57516d3fd1f11.jpg" alt="aaa" style="zoom:50%;" align="right"/>
>
> 

## 九、表格

使用快捷键CTRL+T

|            |      |      |      |      |
| :--------: | ---- | ---- | ---- | ---- |
| ji<br />ji |      |      |      |      |
|            |      |      |      |      |
|            |      |      |      |      |

shift+enter 在同一格中写不同行

## 十、流程图



## 十一、表情符号

> 在编辑中可以找到

Win+句号

## 十二、数学公式

>代码：
>
>$公式$

> ```text
> $formular$
> ```

$$
x^2+1=2*8
$$













