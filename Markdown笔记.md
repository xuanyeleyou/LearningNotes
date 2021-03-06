#  Markdown笔记

##  1、基础语法

###  1.1 字体

各级标题使用# + 空格 + 标题内容的形式，几个#代表几级标题    

粗体使用两个星号包裹，斜体使用一个星号包裹，例如**粗体**，*斜体*

###  1.2 段落与换行

行与行之间有空行，视为换行；若段内换行，则上一行末尾插入两个以上空格，然后按回车键。

**有序列表**  

使用”数字+英文句号+空格+列表内容“  

1. 如果列表每项都没有换行，使用一个空格
2. 如果列表有换行，有序列表使用两个空格，无序列表使用三个空格
3. 如果列表项都只有一行，列表项之间不要使用空行
4. 如果列表项中有换行，那个列表项之间空一行，列表前后也空一行，易于区分

####  无序列表

使用”- + 空格 + 列表内容“ ，以减号标识。

+ 嵌套列表
  + 嵌套列表
    + 嵌套列表

**分隔线**

使用三个以上的*/-/_来标记。

***

----

____

###  1.3 图片

插入图片语法：![图片替代文字](图片地址)

- 图片替代文字为图片无法显示时出现的文字，可以省略
- 图片地址为图片的本地地址或者网络地址



### 1.4 链接

1.   文字链接  

   [文字内容]（链接地址）

2.   引用链接

   [链接文字][连接标记]

   [链接标记]：链接地址。一般此行放在文件末尾

3.   网址链接

   <网址>，<>包裹网址会转换为超链接

4. 链接标题

   链接标题要清晰明了，网址尽量用<>包裹，转换为超链接

   

### 1.5 行内代码和代码块

- 行内代码使用`代码`包裹
- 代码块使用tab键或四个空格开头
- 使用转义或者强调某部分，可以使用`包裹



### 1.6 引用

引用使用< + 内容来标记，多行引用可以在每一行开头添加<，引用内可以使用markdown语法

引用中不要添加空行



### 1.7 转义

\\特殊符号，这样可以实现插入标记符号而不被渲染



## 2 扩展语法GFM

### 2.1 删除线

/~~ 删除的文字/~~，使用两个

~~删除的文字~~，可以实现这种效果

### 2.2 表情符号

:表情符号：，加入表情代码即可，例如:smile:

更多表情符号，可以参考<http://www.webpagefx.com/tools/emoji-cheat-sheet/>

### 2.3 自动链接

使用GFM语法，可以不适用<>包裹网址，就可以被识别，如果不想使用自动链接，可以使用`包裹。

### 2.4 表格

- 表格前后各空一行
- 每一行最前和最后均使用|
- 表格对齐方式：默认左对齐，
  - 右对齐：表头与其他行之间的---，-：表示右对齐
  - 居中对齐：:-:表示

| 序号 | 姓名 |
| :--: | :--: |
|  1   | 张三 |
|  2   | 李四 |

### 2.5 任务列表

- [x] 玩乐
- [ ] 吃喝
- [x] 准备

使用- [ ]标记未选中，- [x]标记选中

### 2.6 围栏代码块

在扩展语法中，代码使用~~~或者```包围，前后建议空一行，可以加上语言名称，支持语法高亮

~~~C++
#including <studio.h>
using namespace std;

int main ():{
    cout<< "good morning!";
    return 0;
}
~~~

### 2.7 锚点/书签

语法：[锚点说明][#锚点名]，锚点名使用字母或数字，不加空格，锚点可以跳转到指定地点。

## 3 排版技巧

1. 中文标点符号和后面的内容不加空格；
2. 数字和百分号之间不加空格；
3. 数字和单位符号之间不加空格；
4. 货币符号后不加空格；
5. 负号后不加空格；
6. 全角：占两个字节，中文标点符号用全角；
7. 半角：占一个字节，英文标点符号用半角；



## 4 Typora使用指南











