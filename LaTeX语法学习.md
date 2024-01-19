# <center>LaTeX学习</center>
## 基础格式

\documentclass[[a4paper](#options),11pt]{[article](#article)}  
`使用article 格式、11 磅大小的字体、A4 纸大小`  
中文短文这样表示：\documentclass[UTF8]{ctexart}

\\[usepackage](#package)[utf8]{inputenc}  
`字体编码，用来嵌入字体（中文）`  
% define the title  
\author{FanL}  
\title{Instructions}

\begin{document}  
\maketitle 生成标题（必须有）  
\tableofcontents 输出目录  
\section{Section1}  
Well, and here begins my lovely article.  
\section{Section2}  
Here it ends.  
\end{document}


### Article

| 可选种类 | 详解 |  
| :--------: | :----: |  
|    article   | 排版科学期刊、演示文档、短报告、程序文档、邀请函 |  
| proc |  一个基于 article 的会议文集类。 |  
| minimal | 非常小的文档类。只设置了页面尺寸和基本字体。主要用来查错 |  
| report | 排版多章节长报告、短篇书籍、博士论文 |    
| slides | 排版幻灯片|  


### Options

| 项目 | 详解             | 备注 |
| :----: | :----------------: | :----: |
| 字体 | 10pt, 11pt, 12pt |默认使用10pt|
|纸张尺寸|a4paper, letterpaper为美国A4|a5paper, b5paper, executivepaper,legalpaper|
|对齐|fleqn 设置行间公式为左对齐；leqno 设置行间公式的编号为左对齐|
|排版|onecolumn, twocolumn|
|打印|twoside；oneside |article 和report 类为单面(single sided) 格式，book 类缺省为双面(double sided) 格式。|
|纸张方向|默认为portrait（纵向）| landscape为横屏|
|新章开始|默认每章都从奇数页开始openright|openany从新的一页开始，不分奇偶：仅对书籍有效|

### package
| 名称 | 用途 |
| :----: | :----: |
| doc |排版LATEX 的说明文档|
|exscale |提供了按比例伸缩的数学扩展字体|
|fontenc |指明LATEX 字体编码(font encoding)|
|ifthen |提供如下形式的命令‘if . . . then do . . . otherwise do . . . .’|
|latexsym| 提供LATEX 符号字体。|
|makeidx |提供排版索引的命令。|
|syntonly| 编译文档而不生成 dvi 文件（常用于查错）。
|inputenc |指明使用哪种输入编码，如 ASCII, ISO Latin-1, ISO Latin-2, 437/850 IBM code pages, Apple Macintosh, Next, ANSI-Windows 或用户自定义编码。|


## 基础语法


| [列表](#列表) | [表格](#表格) | [图片](#图片) | [引用](#引用) | [参考文献](#参考文献) |
| ---- | ---- | ---- | ---- | ----- |
|      |      |      |      | [页面样式](#页面样式) |




- %为注释符号
- 脚注：\footnote{脚注内容}
- 强调（似乎是斜体）：\emph{强调内容} 
- 引用  
`\begin{quote}`  
`引用的内容` 要改变字体的话需要反斜杠\  
`\end{quote}`

- 摘要：需要begin也需要end
- 导入文件  
`\input{文件名(不用加后缀)}`
- 符号
```
引号：`' 
双引号：``''
```
>破折号：- - -  
省略号：\dots 或是 \ldots `句末需要加. 句中格式如下：$\ldots$`  
全角半角挤压`\punctstyle{quanjiao} \puncstyle{banjiao}`  





- 空格
`~` 表示不可打断的空格 Question~1  
`\phantom{这是用来占位置的参数}`    
`\hphantom{这是用来占位置的参数}` 水平方向占位，和上文样式一样  
`\vphantom{这是用来占位置的参数}` 看不出怎么垂直占位的  

- 分段  
`\par`空一格  
`\newline`不缩进  

- 跨行不分段  
`\linebreak`  
`\\`顶格  

- 字体
斜体：`\textit{}  \textsl{文字}`  
加粗：`\textbf{}`   
下划线：`\underline`  
首字放大
```
\usepackage{lettrine}
...

\lettrine[lines=2]{\textbf{需要放大的字/字母}}
```

### 页面样式  
`\pagestyle{格式}`  
[点击参看](https://zhuanlan.zhihu.com/p/652713952)

| 格式       | 样式  |
| ---------- | ------------- |
| empty      | 空白页眉、页脚                                 |
| plain      | 空白页眉，但是在页脚的中央输出页码             |
| headings   | 空白页脚，但是在页眉输出页码、章节编号以及标题 |
| myheadings | 自定义   |

```
\usepackage{fancyhdr} 插入页脚的宏包
修改页眉页脚参数，页眉页脚参数的修改是按左中右进行的。在正文中输入如下命令：
\pagestyle{fancy}
\lhead{}
\chead{}
\rhead{}
\lfoot{}
\cfoot{}
\rfoot{}  空格即表示空白
\renewcommand{\headrulewidth}{0.4pt}
\renewcommand{\footrulewidth}{0.4pt} 设置页眉页脚分割线的宽度，如果为0pt,则不显示线条
```

### 列表
- 无序列表
```
\begin{itemize}
  \item The individual entries are indicated with a black dot, a so-called bullet.
  \item The text in the entries may be of any length.
\end{itemize}
```

- 有序列表
```
\begin{enumerate}
  \item This is the first entry in our list
  \item The list numbers increase with each entry we add
\end{enumerate}
```

- 都没有
```
\begin{description}
\item 城关小学
\item 城关中学
\end{description}
```

### 表格
[在线工具](https://www.tablesgenerator.com/)

\begin{center} 居中  
\begin{tabular}{ c c c } 3个c表示居中，有3列  
 cell1 & cell2 & cell3 \\换行命令  
 cell4 & cell5 & cell6 \\  
 cell7 & cell8 & cell9  
\end{tabular}  
\end{center}  

`水平线命令 \hline 和垂直线参数 | 来添加边框`
`\begin{tabular}{||c c c||}`   
 
`\begin{tabular}{|rrr|}`3个r表示表格有三列，右对齐  
\centering 也可以这样表格居中  
`\hline` 横直线，可以认为是分割线  
`直角边 $a$ & 直角边 $a$ & 斜边 $a$ \\`  
`\hline`  
`    3 &         4 &        5\\`  
`    5 &        12 &        13\\`  
`\hline`  
`end{tabular}`  
在第2-4列画水平线 `\cline{2-4}`  
- 表格添加标题，需要在table环境中  
```  
\begin{table}  
\centering 表格居中  
\begin{tabular}{|c| c| c|}

\end{tabular}
\caption{表格标题}
\end{table}
```
- 插入长表格
```
\usepackage{longtable} %插入长表格需要一个长表格的宏包
\begin{longtable}
长表格内容
\end{longtable}
```

### 图片

- 插图：插图由外部包提供，所以需要先导包  
`\usepackage{graphic}`  
`\includegraphics[width=宽度]{文件名.格式}`  
`sale=放缩因子；height=高度`  

### 引用

\ref   
\pageref
```
\begin{figure}[h]
    \centering
    \includegraphics[width=0.25\textwidth]{mesh}
    \caption{a nice plot}
    \label{fig:mesh1}
\end{figure}

As you can see in the figure \ref{fig:mesh1}, the
function grows near 0. Also, in the page \pageref{fig:mesh1}
is the same example.
```

### 参考文献

- 方法一
```
\begin{thebibliography}{99} 
{99}表示添加参考文献数量的宽度，任意两位数字的参考文献就可以输入99
\bibitem{01}参考文献
\bibitem{02}
\bibitem{03}
\bibitem{04}
\bibitem{05}
\bibitem{06}
\end{thebibliography}
正文中引用到参考文献的地方\cite{01} \cite{02}
这种参考文献的方式认为给定了顺序，引用也只能引用人为给定的顺序
```

- 方法二
参考文献引用格式选取Bibtex,将所需参考文献的bibtex格式放入一个text文件里,命名为reference。
```
\bibliographystyle{plain}  参考文献的排版格式
\bibliographystyle{reference.bib}  参考文献的位置
再选择File，open，打开reference.text文件，再save as reference.bib
正文引用文献的地方\cite{参考文献的名称}
再加上宏包：\usepackage{hyperref} %正文里点击引用可直接跳转至参考文献部分
这种方式不用在意参考文献顺序，自动排版
```
