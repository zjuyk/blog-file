---
title: "Latex环境构建与语法小结"
date: 2019-11-09T02:02:50+08:00
draft: true
tags: 
- latex
---


众所周知，`Latex`是科技类论文写作必须要学的语法，为了毕业论文足够好看，内容足够丰富，于是打算好好花时间再熟悉一下`Latex`的语法，并创建一个友好的撰写环境。

<!--more-->

### 1 环境准备

#### 1.1 我的环境

- ArchLinux
- KDE5 
- Texlive-most
- Texlive-langchinese
- Kile

#### 1.2 搭建环境

```bash
$ sudo pacman -S texlive-most # most of the packages on texlive
$ sudo pacman -S texlive-langchinese # chinese support
$ sudo pacman -S kile # the tex editor， choose the xelatex compiler
```

### 2 具体语法

#### 2.1 基础模板

```tex
\documentclass[UTF8]{ctexart}
\begin{document}
你好，世界！
\end{document}
```

#### 2.2 撰写标题

```tex
\title{Title}
\author{Author}
\date{}

\begin{document}
\maketitle
% 显示标题
\end{document}

% 想要设置标题格式可以这样做，设置字体大小
\usepackage{titling}
\pretitle{\begin{center}\fontsize{30pt}{30pt}\selectfont}
\posttitle{\end{center}}
```

#### 2.3 插入目录

```tex
% \maketitle
\tableofcontents % Set the TOC
```

#### 2.4 设置页边距

```tex
\usepackage{geometry}
\geometry{left=1in,right=1in,top=1in,bottom=1in} % 单位是英寸
```

#### 2.5 显示分页

```tex
\newpage
```

#### 2.6 撰写注释

```tex
% comment 单行注释

% 方法一
\usepackage{verbatim}
\begin{comment}
comments
\end{comment}

% 方法二
\iffalse
comments
\fi
```

#### 2.7 层次结构

```tex
% latex一共五级标题
\section{第一}
\subsection{第二}
\subsubsection{第三}
\paragraph{第四}
\subparagraph{第五}
```

#### 2.8 插入公式

```tex
\usepackage{amsmath} % use AMS-LaTex

% 行内公式
$ ... $
\[ ... \]

% 编号
\begin{equation}
...
\end{equation}

% 不编号
\begin{equation*}
...
\end{equation*}

\begin{displaymath}
...
\end{displaymath}
```

**Hint:** 详细介绍见官网

#### 2.9 原样输出

```tex
% 单行
\verb|text|
% 多行
\begin{verbatim}
\begin{test}
\end{verbatim}
```

#### 2.10 脚注、引言和超链接

```tex
% 脚注
\footnote{something}
% 引言
\begin{quote}
something
\end{quote}
% 超链接
\usepackage[colorlinks,linkcolor=blue]{hyperref} % 设置链接颜色
\url{http://www.baidu.com} % 显示超链接
\href{http://www.baidu.com}{百度} % 隐藏超链接
```

#### 2.11 撰写表格

```tex
% 基本格式
\begin{tabular}{cc} % 几个c代表几列,同时也代表了居中，l代表左对齐，r右对齐
    1&2\\  % "&"符号用来分隔列
    3&4\\
\end{tabular}

% 添加竖线和横线
\begin{tabular}{|c|c|} % 加|表示竖线
    \hline
    1&2\\  % "&"符号用来分隔列
    \hline
    3&4\\
    \hline
\end{tabular}

% 横线加粗
\usepackage{booktabs}
\begin{tabular}{|c|c|} % 加|表示竖线
    \toprule
    1&2\\  % "&"符号用来分隔列
    \midrule
    3&4\\
    \bottomrule
\end{tabular}

% 添加表标题和标签
% 放在table里面
\begin{table}[!htb] % 表参数，一般默认加就行
    \centering
    \caption{表标题}\label{tab:aStrangeTable}%添加标题 设置标签
    \begin{tabular}{cc}
    ...
    \end{tabular}
\end{table}
% !表示忽略美观，按后面参数顺序依次尝试排版表格
% h表示表格放在当前位置
% t表示表格放在下一页顶部
% b表示表格放在本页底部

% 单元格合并
\multicolumn{3}{|c|}{something} % 横向合并3列，居中显示文本

\multirow{4}*{something}&1&2&3&4&5&6\\ % 纵向合并四行
\cline{2-7} %为2-7列添加横线

% 斜线表头
\usepackage{diagbox} 
\diagbox{甲}{乙}{丙}...\\ % 添加斜线表头，参数个数自己调整，一般是两、三个
\diagbox[innerwidth=2cm] % 调整列宽度以适应嵌套表格
```

#### 2.12 插入图片

```tex
\documentclass{article}
\usepackage{graphicx}
\begin{document}
\includegraphics{a.jpg}
\end{document}
```

#### 2.13 强调

```tex
\underline{\textbf{something}} % 加黑加下划线
\emph{something} % 斜体
```

#### 2.14 伪代码

```tex
\usepackage{algorithm} 
\usepackage{algorithmic} % 命令全大写 
% \usepackage{algorithmicx} % 命令首字母大写 
\begin{algorithm}[!h] 
	\caption{PARTITION$(A,p,r)$} 
	\begin{algorithmic}[1] % 以下一行一个标行号 
		\STATE $i=p-1$ 
		\FOR{$j=p$ to $r$} 
		\IF{$A[j]<=0$} 
		\STATE $swap(A[i],A[j])$ 
		\STATE $i=i+1$ 
		\ENDIF 
		\ENDFOR 
		\end{algorithmic} 
		\end{algorithm} 
		
% algorithmic命令 
\STAT <text> % 普通赋值语句 
\IF{<condition>} \STATE{<text>} \ENDIF % if分支 
\FOR{<condition>} \STATE{<text>} \ENDFOR % for循环 
\FOR{<condition> \TO <condition> } \STATE{<text>} \ENDFOR 
\FORALL{<condition>} \STATE{<text>} \ENDFOR 
\WHILE{<condition>} \STATE{<text>} \ENDWHILE % while循环 
\REPEAT \STATE{<text>} \UNTIL{<condition>} 
\LOOP \STATE{<text>} \ENDLOOP 
\REQUIRE <text> 
\ENSURE <text> 
\RETURN <text> % 返回 
\PRINT <text> % 打印 
\COMMENT{<text>} % 注释 
\AND, \OR, \XOR, \NOT, \TO, \TRUE, \FALSE
```

### 3 详细参考

- [官方教程](http://mirrors.ustc.edu.cn/CTAN/info/lshort/chinese/lshort-zh-cn.pdf)
- [一份简单的入门教程](https://liam.page/2014/09/08/latex-introduction/)
