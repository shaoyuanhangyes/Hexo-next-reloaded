---
title: Markdown书写数学公式
date: 2019-01-16 22:47:36
categories:
- Markdown
- Math
tags:
- Markdown
- Math
mathjax: true
---

## 前提
theme/next下_config.yml中` mathjax: enable: true`要开启
## 插入数学公式
### 行内公式
**行内公式**是可以让公式在文中与文字或其他东西混编，不独占一行。` $数学公式$`
+ 示例
```
质能方程 $E = mc^2$
```
+ 显示
质能方程 $E = mc^2$
<!-- more --> 
### 独立公式
**独立公式**使公式单独占一行，不与文中其他文字等混编。 ` $$数学公式$$`
+ 示例
```
质能方程 $$E = mc^2$$
```
+ 显示
质能方程 $$E = mc^2$$

## 普通数学公式
### 加减乘除
与平时书写习惯一样
* 示例
```
$$x = 25*y+18-z/2+10%3$$
```
* 显示
$$x = 25*y+18-z/2+10%3$$

### 上下标
使用`^`来表示上标 `_`来表示下标 同时如果上下标的内容多于一个字符 可以使用`{}`来将这些内容括起来当做一个整体 上下标是可以嵌套的。
* 示例
```
$$x = a_{1}^n+a_{2}^n+a_{3}^n$$
```
* 显示
$$x = a_{1}^n+a_{2}^n+a_{3}^n$$
如果希望左右两边都能有上下标，可以使用\sideset语法
* 示例
```
$$\sideset{^1_2}{^3_4}\bigotimes$$
```
* 显示
$$\sideset{^1_2}{^3_4}\bigotimes$$

### 括号
`()` `[]` 和`|`都表示它们自己，但是`{}`因为有特殊作用因此当需要显示大括号时一般使用`\lbrace \rbrace`来表示。
* 示例
```
$$f(x,y) = 25*\lbrace x+(y-1)/2 \rbrace$$
```
* 显示
$$f(x,y) = 25*\lbrace x+(y-1)/2 \rbrace$$

### 分数
分数使用`\frac{分母}{分子}`这样的语法 不过推荐使用`\cfrac`来代替`\frac`显示公式不会太挤。
* 示例
```
$$\frac{1}{3} 与 \cfrac{1}{3}$$
```
* 显示

$$\frac{1}{3} 与 \cfrac{1}{3}$$

### 根号开方
开方使用`\sqrt[次数]{被开方数}`这样的语法
* 示例
```
$$\sqrt[2]{X}$$
$$\sqrt[3]{Y}$$
$$\sqrt{5-x}$$
```
* 显示
$$\sqrt[2]{X}$$
$$\sqrt[3]{Y}$$
$$\sqrt{5-x}$$

## 特殊字符
### 希腊字母

|代码|大写|代码|小写|
|:------:|:------:|:------:|:------:|
|`A`|$A$|`\alpha`|$\alpha$|
|`B`|$B$|`\beta`|$\beta$|
|`\Gamma`|$\Gamma$|`\gamma`|$\gamma$|
|`\Delta`|$\Delta$|`\delta`|$\delta$|
|`E`|$E$|`\epsilon`|$\epsilon$|
|`Z`|$Z$|`\zeta`|$\zeta$|
|`H`|$H$|`\eta`|$\eta$|
|`\Theta`|$\Theta$|`\theta`|$\theta$|
|`I`|$I$|`\iota`|$\iota$|
|`K`|$K$|`\kappa`|$\kappa$|
|`\Lambda`|$\Lambda$|`\lambda`|$\lambda$|
|`M`|$M$|`\mu`|$\mu$|
|`N`|$N$|`\nu`|$\nu$|
|`\Xi`|$\Xi$|`\xi`|$\xi$|
|`O`|$O$|`\omicron`|$\omicron$|
|`\Pi`|$\Pi$|`\pi`|$\pi$|
|`P`|$P$|`\rho`|$\rho$|
|`\Sigma`|$\Sigma$|`\sigma`|$\sigma$|
|`T`|$T$|`\tau`|$\tau$|
|`\Upsilon`|$\Upsilon$|`\upsilon`|$\upsilon$|
|`\Phi`|$\Phi$|`\phi`|$\phi$|
|`X`|$X$|`\chi`|$\chi$|
|`\Psi`|$\Psi$|`\psi`|$\psi$|
|`\Omega`|$\Omega$|`\omega`|$\omega$|

### 关系运算符

|符号|代码|符号|代码|
|:------:|:------:|:------:|:------:|
|$\pm$|`\pm`|$\times$|`\times`|
|$\div$|`\div`|$\mid$|`\mid`|
|$\cdot$|`\cdot`|$\ast$|`\ast`|
|$\cdot$|`\cdot`|$\ast$|`\ast`|
|$\bigodot$|`\bigodot`|$\bigotimes$|`\bigotimes`|
|$\bigoplus$|`\bigoplus`|$\circ$|`\circ`|
|$\leq$|`\leq`|$\geq$|`\geq`|
|$\neq$|`\neq`|$\approx$|`\approx`|
|$\equiv$|`\equiv`|$\sum$|`\sum`|
|$\prod$|`\prod`|$\coprod$|`\coprod`|
|$\bot$|`\bot`|$\angle$|`\angle`|
|$\lceil n\rceil$|`\lceil n\rceil`|$\lfloor n\rfloor$|`\lfloor n\rfloor`|

### 三角函数

|符号|代码|符号|代码|
|:------:|:------:|:------:|:------:|
|$\sin$|`\sin`|$\cos$|`\cos`|
|$\tan$|`\tan`|$\cot$|`\cot`|
|$\sec$|`\sec`|$\csc$|`\csc`|
|$\arcsin$|`\arcsin`|$\arccos$|`\arccos`|
|$\arctan$|`\arctan`|

### 对数函数

|符号|代码|符号|代码|符号|代码|
|:------:|:------:|:------:|:------:|:------:|:------:|
|$\log$|`\log`|$\lg$|`\lg`|$\ln$|`\ln`|

### 微积分

|符号|代码|符号|代码|
|:------:|:------:|:------:|:------:|
|$\lim$|`\lim`|$\prime$|`\prime`|
|$\infty$|`\infty`|$\mathrm{d}$|`\mathrm{d}`|
|$\int$|`\int`|$\oint$|`\oint`|
|$\iint$|`\iint`|$\iiint$|`iiint`|
|$\partial$|`$\partial$`|

### 省略号(Martrix)

|符号|代码|符号|代码|符号|代码|
|:------:|:------:|:------:|:------:|:------:|:------:|
|$$\cdots$$|`$$\cdots$$`|$$\ddots$$|`$$\ddots$$`|$$\vdots$$|`$$\vdots$$`|

### 矩阵
在开头使用 `\begin{matrix}` 在结尾使用 `\end{matrix}` 在中间插入矩阵元素 每个元素之间插入`&` 并在每行结尾处使用 `\\` **(更新最新版本后必须要`\\\`才能起到换行 可能是新版本转义的问题 老版本不存在)**
使用矩阵时必须声明`$`或`$$`符号。

|符号|代码|
|:------:|------|
|$$\begin{matrix}a_{11} & a_{12} & a_{13} \\\a_{21} & a_{22} & a_{23} \\\a_{31} & a_{32} & a_{33} \end{matrix}$$|`$$\begin{matrix} a_{11} & a_{12} & a_{13} \\\ a_{21} & a_{22} & a_{23} \\\ a_{31} & a_{32} & a_{33} \end{matrix}$$`|
|$$\begin{pmatrix}a_{11} & a_{12} & a_{13} \\\a_{21} & a_{22} & a_{23} \\\a_{31} & a_{32} & a_{33} \end{pmatrix}$$|`$$\begin{pmatrix} a_{11} & a_{12} & a_{13} \\\ a_{21} & a_{22} & a_{23} \\\ a_{31} & a_{32} & a_{33} \end{pmatrix}$$`|
|$$\begin{vmatrix}a_{11} & a_{12} & a_{13} \\\a_{21} & a_{22} & a_{23} \\\a_{31} & a_{32} & a_{33} \end{vmatrix}$$|`$$\begin{vmatrix} a_{11} & a_{12} & a_{13} \\\ a_{21} & a_{22} & a_{23} \\\ a_{31} & a_{32} & a_{33} \end{vmatrix}$$`|
|$$\begin{bmatrix}a_{11} & a_{12} & a_{13} \\\a_{21} & a_{22} & a_{23} \\\a_{31} & a_{32} & a_{33} \end{bmatrix}$$|`$$\begin{bmatrix} a_{11} & a_{12} & a_{13} \\\ a_{21} & a_{22} & a_{23} \\\ a_{31} & a_{32} & a_{33} \end{bmatrix}$$`|
|$$\begin{Bmatrix}a_{11} & a_{12} & a_{13} \\\a_{21} & a_{22} & a_{23} \\\a_{31} & a_{32} & a_{33} \end{Bmatrix}$$|`$$\begin{Bmatrix} a_{11} & a_{12} & a_{13} \\\ a_{21} & a_{22} & a_{23} \\\ a_{31} & a_{32} & a_{33} \end{Bmatrix}$$`|
|$$\begin{Vmatrix}a_{11} & a_{12} & a_{13} \\\a_{21} & a_{22} & a_{23} \\\a_{31} & a_{32} & a_{33} \end{Vmatrix}$$|`$$\begin{Vmatrix} a_{11} & a_{12} & a_{13} \\\ a_{21} & a_{22} & a_{23} \\\ a_{31} & a_{32} & a_{33} \end{Vmatrix}$$`|
|$$\begin{pmatrix}a_{11} & a_{12} & \cdots & a_{1n} \\\a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{n1} & a_{n2} & \cdots & a_{nn} \end{pmatrix}_{n\times n}$$|`$$\begin{pmatrix}a_{11} & a_{12} & \cdots & a_{1n} \\\a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{n1} & a_{n2} & \cdots & a_{nn} \end{pmatrix}_{n\times n}$$`|

## 综合案例示例

|符号|代码|
|:------:|:------:|
|$$\pi=16\arctan(\frac{1}{5})-4\arctan(\frac{1}{239})$$|`$$\pi=16\arctan(\frac{1}{5})-4\arctan(\frac{1}{239})$$`|
|$$(\sin{x})^{\prime}=\cos{x}$$|`$$(\sin{x})\prime=\cos{x}$$`|
|$$\lim_{x\to+\infty}\cfrac{sin{x}}{x}=0$$|`$$\lim_{x\rightarrow+\infty}\cfrac{sin{x}}{x}=0$$`|
|$$\int_{a}^{b}f(x)\mathrm{d}{x}=f(\xi)(b-a)$$|`$$\int_{a}^{b}f(x)\mathrm{d}{x}=f(\xi)(b-a)$$`|
|$$e^{i\theta}=\cos\theta+i\sin\theta$$|`$$e^{i\theta}=\cos\theta+i\sin\theta$$`|
|$$\lim_{n\to+\infty}\sum_{i=1}^{n}f[a+\frac{i}{n}(b-a)]\frac{b-a}{n}=\int_{a}^{b}f(x)\mathrm{d}{x}$$|`$$\lim_{n\to+\infty}\sum_{i=1}^{n}f[a+\frac{i}{n}(b-a)]\frac{b-a}{n}=<br>\int_{a}^{b}f(x)\mathrm{d}{x}$$`|
|$$f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-{\frac{(x-\mu)^2}{2\sigma^2}}} -\infty< x <+\infty$$|`$$f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{\frac{(x-\mu)^2}{2\sigma^2}} -\infty< x <+\infty$$`|

