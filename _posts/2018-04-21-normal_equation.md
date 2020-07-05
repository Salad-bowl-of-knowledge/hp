---
toc: true
layout: post
description:
author: 山拓
comments: true
categories: [statistics]
title: 正規方程式
---

最小二乗法で役立つ正規方程式(normal equation)の導出をします。後半にL2正則化項をつけたときの正規方程式も説明します。

## 正規方程式とは？
初めに正規方程式に関する定理を紹介します。

---

**定理**

$X$を行列、$\boldsymbol{\beta}, \boldsymbol{y}$を縦ベクトルとし、$\|\cdot\|$をノルムとする。また$X ^ T$を$X$の転置行列とする。$X ^ TX$が正則なとき, $S(\boldsymbol{\beta})=\|X\boldsymbol{\beta}-\boldsymbol{y}\| ^ 2$つまり$\|X\boldsymbol{\beta}-\boldsymbol{y}\|$を最小にする$\boldsymbol{\beta}$はただ1つ存在し、それは、正規方程式：

$$
X ^ TX\boldsymbol{\beta}=X ^ T\boldsymbol{y}
$$

の解である。

---

$\|X\boldsymbol{\beta}-\boldsymbol{y}\|=0$, すなわち$X\boldsymbol{\beta}=\boldsymbol{y}$となる解が見つかればよい (→補足1) のですが、見つからないときでもこの定理により、$\|X\boldsymbol{\beta}-\boldsymbol{y}\|$を最小にするような$\boldsymbol{\beta}$を見つけられます。

正規方程式は$X\boldsymbol{\beta}=\boldsymbol{y}$の両辺に左から$X ^ {T}$を掛けたものですが、このことを用いて正規方程式を覚えるのがオススメです。

なお、$S(\boldsymbol{\beta})$を最小化する$\boldsymbol{\beta}$を 

$$
\hat {\boldsymbol {\beta }}={\underset {\boldsymbol {\beta }}{\operatorname {arg min} }}\,S({\boldsymbol{\beta}})
$$

と表記することができます。

### 補足1 :
$n$変数の連立1次方程式$A\boldsymbol{x}=\boldsymbol{b}$に解がただ一つ存在する必要十分条件は 

$$
{\rm rank}(A)=n
$$

## 正規方程式の導出
目的関数 $S(\boldsymbol{\beta})=\|X\boldsymbol{\beta}-\boldsymbol{y}\| ^ 2$ を式展開すると、 

$$
\begin{aligned}
\|X\boldsymbol{\beta}-\boldsymbol{y}\| ^ 2&=(X\boldsymbol{\beta}-\boldsymbol{y}) ^ T(X\boldsymbol{\beta}-\boldsymbol{y})\quad \small{(\because 補足2)}\\
&=(\boldsymbol{\beta} ^ TX ^ T-\boldsymbol{y} ^ T)(X\boldsymbol{\beta}-\boldsymbol{y})\\
&=\boldsymbol{\beta} ^ TX ^ TX\boldsymbol{\beta}-\boldsymbol{\beta} ^ TX ^ T\boldsymbol{y}-\boldsymbol{y} ^ TX\boldsymbol{\beta}+\boldsymbol{y} ^ T\boldsymbol{y}\\
&=\boldsymbol{\beta} ^ TX ^ TX\boldsymbol{\beta}-2\boldsymbol{\beta} ^ TX ^ T\boldsymbol{y}+\boldsymbol{y} ^ T\boldsymbol{y}\quad \small{(\because 補足3)} \end{aligned}
$$

となります。これを$\boldsymbol{\beta}$で微分します。これは、各要素$\beta _ j$で偏微分する、つまり勾配(gradient)を求めるということです(→補足4）。微分すると、

$$
\frac{\partial \|X\boldsymbol{\beta}-\boldsymbol{y}\| ^ 2}{\partial
\boldsymbol{\beta}}=2X ^ TX\boldsymbol{\beta}-2X ^ T\boldsymbol{y}
$$

となります。よって、 $S(\boldsymbol{\beta})=\|X\boldsymbol{\beta}-\boldsymbol{y}\| ^ 2$ が最小となる条件として、

$$
\frac{\partial
\|X\boldsymbol{\beta}-\boldsymbol{y}\| ^ 2}{\partial \boldsymbol{\beta}}=0\ \Leftrightarrow\ X ^ TX\boldsymbol{\beta}=X ^ T\boldsymbol{y}
$$

が得られます。$X ^ TX$が正則なとき、$\boldsymbol{\beta}=(X ^ TX) ^ {-1}X ^ T\boldsymbol{y}$ が唯一の解であり、この $\boldsymbol{\beta}$ が目的関数の最小値を与えます。

### 補足2 :
$\mathbb{R} ^ n$のベクトル $\displaystyle \boldsymbol{a}=  \left[ \begin{array}{c} a _ 1 \\ \vdots \\ a _ n \end{array} \right], \ \boldsymbol{b}= \left[ \begin{array}{c} b _ 1 \\ \vdots \\ b _ n
\end{array} \right] $に対して 
$$
\langle \boldsymbol{a}, \boldsymbol{b}\rangle=\boldsymbol{a} ^ T\boldsymbol{b}=a _ 1b _ 1+\cdots+a _ nb _ n
$$

を$\mathbb{R} ^ n$の標準内積とし、ベクトル$\boldsymbol{a}$のノルムを

$$
\|\boldsymbol{a}\|=\sqrt{\langle\boldsymbol{a},\boldsymbol{a}\rangle} 
$$

とします。

### 補足3 :
$\boldsymbol{y} ^ TX\boldsymbol{\beta}=\langle\boldsymbol{y},X\boldsymbol{\beta}\rangle$はスカラーなので、転置を取っても同じです。よって,

$$
\boldsymbol{y} ^ TX\boldsymbol{\beta}=\left(\boldsymbol{y} ^ TX\boldsymbol{\beta}\right) ^ T=\boldsymbol{\beta} ^ TX ^ T\boldsymbol{y} 
$$

### 補足4 :
多変数関数$f(x _ 1,x _ 2,\cdots,x _ n)$に対して、 各変数による偏微分を並べたベクトルを勾配ベクトルといいます。例えば、$f(x _ 1,x _ 2,x _ 3)=x _ 1+x _ 2 ^ 2+x _ 3 ^ 3$のとき、勾配ベクトルは$(1,2x _ 2,3x _ 3 ^ 2)$となります。この勾配ベクトルは、

$$
\frac{df}{d\boldsymbol{x}}={\rm grad}f=\nabla
f=\left(\frac{\partial f}{\partial x _ 1}, \cdots, \frac{\partial f}{\partial x _ n}\right)
$$

などと表記されます。ただし縦ベクトルか横ベクトルかは$\boldsymbol{x}$次第です。

対称行列$X'(=X ^ TX)$(→補足5)に関する二次形式 

$$
\boldsymbol{\beta} ^ TX'\boldsymbol{\beta}=\sum _ {i=1} ^ n\sum _ {j=1} ^ nx' _ {ij}\beta _ i\beta _ j\ \ \ \left(X':=\left[x' _ {ij}\right] _ {n\times n}\right)
$$

を$\beta _ k$で偏微分すると、

$$
\begin{aligned} \frac{\partial \boldsymbol{\beta} ^ TX'\boldsymbol{\beta}}{\partial \beta _ k}
&=\frac{\partial}{\partial \beta _ k}\sum _ {i=1} ^ n\sum _ {j=1} ^ nx' _ {ij}\beta _ i\beta _ j\\
&=\frac{\partial}{\partial \beta _ k}\left(\sum _ {j (\neq k)}x' _ {kj}\beta _ k\beta _ j+\sum _ {i (\neq k)}x' _ {ik}\beta _ i\beta _ k+x' _ {kk}\beta _ k ^ 2\right)\\ 
&=\sum _ {j (\neq k)}x' _ {kj}\beta _ j+\sum _ {i
(\neq k)}x' _ {ik}\beta _ i+2x' _ {kk}\beta _ k\\ 
&=2\sum _ {i=1} ^ nx' _ {ki}\beta _ i\quad (\because X'\text{は対称行列なので、} x' _ {ik}=x' _ {ki}) 
\end{aligned}
$$

となるので、勾配ベクトルは 

$$
\frac{\partial
\left(\boldsymbol{\beta} ^ TX'\boldsymbol{\beta}\right)}{\partial \boldsymbol{\beta}}=\left[ \begin{array}{c} 2\sum _ {i=1} ^ nx' _ {1i}\beta _ i \\ \vdots \\ 2\sum _ {i=1} ^ nx' _ {ni}\beta _ i \end{array}
\right]=2\left[ \begin{array}{ccc} x' _ {11} &\cdots&x' _ {1n}\\ \vdots&\ddots&\vdots \\ x' _ {n1} &\cdots&x' _ {nn} \end{array} \right]\left[ \begin{array}{c} \beta _ 1 \\ \vdots \\
\beta _ n \end{array} \right]=2X'\boldsymbol{\beta}
$$

となります。 よって

$$
\frac{\partial \left(\boldsymbol{\beta} ^ TX ^ TX\boldsymbol{\beta}\right)}{\partial \boldsymbol{\beta}}=2X ^ TX\boldsymbol{\beta}
$$

が成り立ちます。

また、線形関数 

$$
\begin{aligned} 
\boldsymbol{\beta} ^ TX ^ T\boldsymbol{y}&=\langle X\boldsymbol{\beta}, \boldsymbol{y}\rangle\\
&=\left(\sum _ {j=1} ^ m x _ {1j}\beta _ j\right)y _ 1+\cdots+\left(\sum _ {j=1} ^ m x _ {nj}\beta _ j\right)y _ n\\
&=\sum _ {i=1} ^ n\sum _ {j=1} ^ m x _ {ij}\beta _ jy _ i 
\end{aligned}
$$

を$\beta _ k$で偏微分すると、

$$
\begin{aligned} 
\frac{\partial \left(\boldsymbol{\beta} ^ TX ^ T\boldsymbol{y}\right)}{\partial
\beta _ k}&=\frac{\partial}{\partial \beta _ k}\sum _ {i=1} ^ n\sum _ {j=1} ^ m x _ {ij}\beta _ jy _ i\\ 
&=\frac{\partial}{\partial \beta _ k}\sum _ {j=1} ^ m\left(\sum _ {i=1} ^ n x _ {ij}y _ i\right)\beta _ j\\
&=\sum _ {i=1} ^ n x _ {ik}y _ i 
\end{aligned} 
$$

となるので、勾配ベクトルは 

$$
\frac{\partial \left(\boldsymbol{\beta} ^ TX ^ T\boldsymbol{y}\right)}{\partial \boldsymbol{\beta}}=\left[ \begin{array}{c} \sum _ {i=1} ^ n
x _ {i1}y _ i \\ \vdots \\ \sum _ {i=1} ^ n x _ {im}y _ i \end{array} \right]=\left[ \begin{array}{ccc}x _ {11} &\cdots& x _ {n1}\\ \vdots&\ddots&\vdots \\ x _ {1m} &\cdots&x _ {nm} \end{array}
\right]\left[ \begin{array}{c} y _ 1 \\ \vdots \\ y _ n \end{array} \right]=X ^ T\boldsymbol{y}
$$

となります。

### 補足5 :  $X ^ T X$ は対称行列となる
**[証明]** 行列$A$が対称行列ならば、$A ^ T=A$である。

$$
\left(X ^ TX\right) ^ T=X ^ T\left(X ^ T\right) ^ T=X ^ TX
$$

が成り立つので、$X ^ TX$は対称行列である。

## L2正則化 (ridge)の正規方程式
L2正則化(ridge)をかけた場合の目的関数$S _ \lambda(\boldsymbol{\beta})$は 

$$
S _ \lambda(\boldsymbol{\beta})=\frac{1}{n}\|\boldsymbol{y}-X\boldsymbol{\beta}\| ^ 2+\lambda\|\boldsymbol{\beta}\| ^ 2 
$$

となります。

第1項は正則化が無い場合と同じなので、第2項について考えます。 $\|\boldsymbol{\beta}\| ^ 2$を$\beta _ k$で偏微分すると、 

$$
\begin{aligned} \frac{\partial \|\boldsymbol{\beta}\| ^ 2}{\partial \beta _ k} 
&=\frac{\partial}{\partial
\beta _ k}\sum _ {i=1} ^ n\sum _ {j=1} ^ n \beta _ i\beta _ j\\ 
&=2\beta _ k 
\end{aligned}
$$

ゆえに 

$$
\frac{\partial \|\boldsymbol{\beta}\| ^ 2}{\partial \boldsymbol{\beta}}=2\boldsymbol{\beta} 
$$

となります。

第1項と合わせると、目的関数 $S _ \lambda(\boldsymbol{\beta})$を最小化する条件は 

$$
\begin{aligned} 
\frac{1}{n}\cdot2(X  ^ {T}X \boldsymbol{\beta}-X  ^ {T}\boldsymbol{y})+2\lambda \boldsymbol{\beta}&=0\\ X  ^ {T}X
\boldsymbol{\beta}-X  ^ {T}\boldsymbol{y}+\lambda n \boldsymbol{\beta}
&=0\\ (X  ^ {T}X +\lambda nI )\boldsymbol{\beta}
&=X  ^ {T}\boldsymbol{y}
\end{aligned}
$$

となります。ただし、$I$は単位行列です。よって、L2正則化をつけた場合の正規方程式は 

$$
\boldsymbol{\beta}=(X ^ T X +\lambda nI ) ^ {-1}X ^ T y
$$

となります。

これの良いところは$X  ^ {T}X$が正則でなくとも、$X  ^ {T}X +\lambda nI$が正則となることで、逆行列を求めることができるという点です。また、これが**正則化**という名の所以です。

## 最急降下法とのメリット・デメリットの比較
最適なパラメータを求めるのに、正規方程式を使う方法とは別に機械学習における最急降下法（勾配降下法）を用いる方法があります。

| 最急降下法(gradient descent) | 正規方程式(normal equation)  |
| :--: | :--: |
| 学習率 (learning rate) $\alpha$ の設定が必要 | 学習率を設定する必要なし |
| 計算を繰り返す(ループ処理をする）必要がある | ループ処理の必要なし |
| 計算量は $O (kp ^ 2)$ | $X ^ T X$ の逆行列を計算するため、計算量は $O(p ^ 3)$     |
| 説明変数の値域をそろえる（feature scalingする）必要がある | feature scalingの必要はない |
| $p$ が大きくても動作する | $p$ が大きくなると遅い |

なお、説明変数の数$p$が大きいというのは、おおよそ$p > 10 ^ 4$ぐらいからです。$p$が小さいときは正規方程式を用いるのがよいでしょう。