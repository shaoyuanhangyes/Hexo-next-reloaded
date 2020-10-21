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