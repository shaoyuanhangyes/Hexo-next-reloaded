---
title: Mathematical Formulas
date: 2020-10-21 17:31:13
categories:
- Markdown
- Math
tags:
- Markdown
- Math
mathjax: true
---

theme/next下_config.yml中` mathjax: enable: true`要开启

## 自记常考数学公式

### 均值不等式

<b>均方根QM</b>:  $\sqrt{\cfrac{x_{1}^2+\cdot\cdot\cdot+x_{n}^2}{n}}$

<b>算术平均AM</b>: $\cfrac{x_1 + \cdot\cdot\cdot + x_n}{n}$

<b>几何平均GM</b>: $\sqrt[n]{x_1 \cdot\cdot\cdot x_n}$

<b>调和平均HM</b>: $\cfrac{n}{\cfrac{1}{x_1}+\cdot\cdot\cdot+\cfrac{1}{x_n}}$
<!-- more --> 

存在 均方根QM $\geq$ 算术平均AM $\geq$ 几何平均GM $\geq$ 调和平均HM

$$ \sqrt{\cfrac{x_{1}^2+\cdot\cdot\cdot+x_{n}^2}{n}} \geq \cfrac{x_1 + \cdot\cdot\cdot + x_n}{n} \geq \sqrt[n]{x_1 \cdot\cdot\cdot x_n} \geq \cfrac{n}{\cfrac{1}{x_1}+\cdot\cdot\cdot+\cfrac{1}{x_n}}$$
当且仅当$x_1=x_2= \cdot\cdot\cdot =x_n$时  等号成立

### 三角不等式

#### 一般形式

$n$维向量$x=(x_1,\cdot\cdot\cdot,x_n) y=(y_1,\cdot\cdot\cdot,y_n)$ 因为两边之和一定大于第三边 所以成立
$||x|-|y|| \leq |x \pm y| \leq |x|+|y|$ 当且仅当 $x||y$ (xy平行)的时候才成立
即: $$\sqrt{(x_1 \pm y_1)^2+\cdot\cdot\cdot+(x_n \pm y_n)^2} \leq \sqrt{x_{1}^2+\cdot\cdot\cdot+x_{n}^2}+\sqrt{y_{1}^2+\cdot\cdot\cdot+y_{n}^2}$$
$$\sqrt{(x_1 \pm y_1)^2+\cdot\cdot\cdot+(x_n \pm y_n)^2} \geq |\sqrt{x_{1}^2+\cdot\cdot\cdot+x_{n}^2}-\sqrt{y_{1}^2+\cdot\cdot\cdot+y_{n}^2}|$$

#### 1维特别形式
当$n$维向量的维数n=1时 有
$$||a|-|b|| \leq |a+b| \leq |a|+|b|$$ 左处等号当且仅当$ab \leq 0$时成立 右处等号当且仅当$ab \geq 0$时成立

$$||a|-|b|| \leq |a-b| \leq |a|+|b|$$ 左处等号当且仅当$ab \geq 0$时成立 右处等号当且仅当$ab \leq 0$时成立

### 基本初等函数不等式

$\cfrac{x}{1+x} \leq \ln(1+x) \leq x  \forall x>-1$

$\sin{x} \leq x \leq \tan{x}$

$e^x \geq x+1$

### 柯西不等式 

#### 离散形式

$$
(\sum_{i=1}^{n}a_ib_i)^2 \leq (\sum_{i=1}^{n}a_i^{2})(\sum_{i=1}^{n}b_i^{2})
$$
当且仅当$a_i=kb_i$时等号成立

#### 连续形式

$$
(\int_{a}^{b} f(x)g(x)\mathrm{d}x)^2 \leq \int_{a}^{b}f^2(x)\mathrm{d}x\int_{a}^{b}g^2(x)\mathrm{d}x
$$

当且仅当存在常数k使得$f(x)=kg(x)$成立

#### 多元形式

$$
(\iint_{D} f(x,y)g(x,y)\mathrm{d}\sigma)^2 \leq (\iint_{D} f^2(x,y)\mathrm{d}\sigma)(\iint_{D} g^2(x,y)\mathrm{d}\sigma)
$$

### 切比雪夫不等式

$$
对于x_1 \geq x_2 \geq \cdot\cdot\cdot \geq x_n 和 y_1 \geq y_2 \geq \cdot\cdot\cdot \geq y_n 存在不等式:
$$

顺序和 $\geq$ 乱序和 $\geq$ 逆序和

$$
x_1y_1+x_2y_2+\cdot\cdot\cdot+x_ny_n \geq \cfrac{1}{n}(x_1+x_2+\cdot\cdot\cdot+x_n)(y_1+y_2+\cdot\cdot\cdot+y_n) \geq x1y_n+x_2y_{n-1}+\cdot\cdot\cdot+x_ny_1
$$