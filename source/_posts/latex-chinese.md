---
title: LaTeX中文模版代码
date: 2019.12.21
tags:
- latex
- template
categories: maths
---

LaTeX - 中文（模板）

有些初使用LaTeX进行中文写作的作者，总是会遇到不能正确利用LaTeX编译中文文档的问题，这里总结出一个模板，来帮助这些作者方便进行LaTeX中文写作，具体步骤如下：

1. 添加如下代码
2. XeLaTeX编译执行

（注意：生成目录需要编译两次）

具体LaTeX代码如下：

<!--more-->

```latex
\documentclass{article}\usepackage{geometry}
\geometry{a4paper}
%\usepackage[UTF8, heading = false, scheme = plain]{ctex}%格式
\usepackage{ctex}
%\usepackage{authblk} %添加机构，需要安装preprint包
\usepackage{graphicx} %添加图片
\usepackage{amsthm}
\usepackage{amsmath}
\renewcommand{\vec}[1]{\boldsymbol{#1}} % 生产粗体向量，而不是带箭头的向量
\usepackage{amssymb}
\usepackage{booktabs} % excel导出的大表格%
\newtheorem{definition}{Definition} %英文
%\newtheorem{theorem}{Theorem}
\newtheorem{definition}{定义} %中文
\newtheorem{lemma}{引理}
\newtheorem{theorem}{定理}
%\newenvironment{proof}{{\noindent\it 证明}\quad}{\hfill $\square$\par}
\DeclareMathOperator{\Ima}{Im}%定义新符号
\DeclareMathOperator{\Rank}{rank}%定义求秩算子
\title{数学-计算机视觉}
\author{Jayzon Xu}
%\affil{MIT}
%date{2019年11月8日} %注释后显示为编译时日期
\begin{document}
\maketitle
\tableofcontents
\newpage
% 生成目录，请删除上面两行注释
%\listoffigures
%\newpage
% 生成图片列表，请删除上面两行注释
\section{数学}
\subsection{高等代数与最优化理论}
%\begin{figure}[ht] %htbp
%\centering
%\includegraphics[scale=0.6]{gradient.png}
%\caption{this is a figure demo}
%\label{fig:label}
%\end{figure}
A =
\begin{pmatrix}
    1 & 2 & 3 \\
    2 & 3 & 1 \\
    3 & 1 & 2
\end{pmatrix}
$\rho(A) = 6$.
\subsection{数学分析}
\begin{equation}\label{equ:sin}
\sin(z) = 2
\end{equation}
其中，$z\in \mathbb{C}$.
您能求解方程(\ref{equ:sin})吗？
\section{计算机视觉}
\subsection[Panoptic Segmentation]{全景分割}
\subsubsection{语义分割}
\subsubsection{实例分割}
\end{document}</code></pre>
```

<p>建议使用<a href="https://miktex.org/download">MikTex</a> + <a href="http://texstudio.sourceforge.net/">TeXstudio</a>作为编译器。</p>