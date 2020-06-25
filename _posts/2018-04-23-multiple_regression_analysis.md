---
toc: true
layout: post
description:
author: 山拓
comments: true
categories: [statistics]
title: 重回帰分析
---

## 重回帰モデル

$n$個のデータ $(y _ 1,x _ {11}, \cdots, x _ {1p}),\cdots (y _ n,x _ {n1},\cdots,x _ {np})$ を線形モデル 

$$
y=\beta _ 0+\beta _ 1x _ 1+\cdots+\beta _ px _ p=\beta _ 0+\sum _ {j=1} ^ p \beta _ jx _ j
$$

で説明することを考えます。これは単回帰において説明変数を1つから $p$ 個に増やしたものです。最小二乗法の考え方に基づき、 $\displaystyle\sum _ {i=1} ^ n\left[y _ i-\left(\beta _ 0+\sum _ {j=1} ^ p \beta _ jx _ {ij}\right)\right] ^ 2$ を最小化することを目標とするわけですが、データをまとめて扱うために行列とベクトルで表記することを考えましょう。 ここで、 

$$
\renewcommand{\arraystretch}{1.5} \boldsymbol{y}= \left[ \begin{array}{c} y _ 1\\ y _ 2\\ \vdots \\ y _ n \end{array} \right] ,\ \ \ X=\left[ \begin{array}{ccccc} 1 & x _ {11}& x _ {12} &\cdots
& x _ {1p} \\ 1& x _ {21}& x _ {22}&\cdots & x _ {2p}\\ \vdots & \vdots& \vdots& & \vdots \\1 &x _ {n1} & x _ {n2} &\cdots & x _ {np} \end{array} \right] ,\ \ \
\boldsymbol{\beta}= \left[ \begin{array}{c} \beta _ 0\\ \beta _ 1\\ \vdots \\ \beta _ p \end{array} \right]
$$

とすると、重回帰モデルは 

$$
\boldsymbol{y}=X\boldsymbol{\beta}+\underbrace{\boldsymbol{\varepsilon}} _ {誤差項}
$$

と書けるので、最小二乗法により、 $$ \boldsymbol{\hat{y}}=X\boldsymbol{\hat{\beta}} $$ を推定します。

目的関数$S(\boldsymbol{\beta})$は 

$$
S(\boldsymbol{\beta})=\sum _ {i=1} ^ n\left[y _ i-\left(\beta _ 0+\sum _ {j=1} ^ p
\beta _ jx _ {ij}\right)\right] ^ 2={\|\boldsymbol{y}-X\boldsymbol{\beta}\|} ^ 2={\|X\boldsymbol{\beta}-\boldsymbol{y}\|} ^ 2
$$

となり、 $S(\boldsymbol{\beta})$を最小化する$\boldsymbol{\beta}$, つまり 

$$
\hat {\boldsymbol
{\beta }}={\underset {\boldsymbol {\beta }}{\operatorname {arg min} }}\,S({\boldsymbol{\beta}})
$$

を求めます。

## 正規方程式
さて、最小二乗法により最適な解を求めたいわけですが、条件に基づいて目的関数$S(\boldsymbol{\beta})$を微分すると次のような方程式が得られます。 

$$
X ^ TX\boldsymbol{\hat\beta}=X ^ T\boldsymbol{y}
$$

これを**正規方程式**と言います。正規方程式の導出については[こちら]([https://omedstu.jimdofree.com/2018/04/21/%E6%AD%A3%E8%A6%8F%E6%96%B9%E7%A8%8B%E5%BC%8F/](https://omedstu.jimdofree.com/2018/04/21/正規方程式/))をお読みください。この正規方程式より、係数の推定値は

$$
\boldsymbol{\hat\beta}={(X ^ TX)} ^ {-1}X ^ T\boldsymbol{y}
$$

という式で得られます。

### 正規方程式の解の覚え方
正規方程式は覚えるべき式です。そこで覚え方について説明します。もし、次の方程式に解があったらどうでしょうか。

$$
\boldsymbol{y}=X\boldsymbol{\beta} 
$$

これは得られたデータが誤差なく完全に直線（平面）上にある場合のみ解が存在します。しかし、実際にはそのようなデータはなく、必ず誤差を含むので、この方程式の解は存在しません。ところで、この方程式に左から$X ^ {T}$をかけると、

$$
X ^ T\boldsymbol{y}=X ^ TX\boldsymbol{\beta} 
$$

つまり、 

$$
X ^ TX\boldsymbol{\hat\beta}=X ^ T\boldsymbol{y} 
$$

となり、これは正規方程式です。なので、「正規方程式は$\boldsymbol{y}=X\boldsymbol{\beta}$の左から$X ^ {T}$をかける」と覚えると良いでしょう。

### 正規方程式で解が求められない場合
正規方程式 

$$
X ^ TX\boldsymbol{\hat\beta}=X ^ T\boldsymbol{y} 
$$

の解を求めるには${(X ^ TX)} ^ {-1}$が計算できなくてはなりません。そのためには$X ^ TX$が正則であることが条件です。大概が正則になりますが、データによっては正則にならない場合 (線形代数の言葉で「rank落ち」する場合)も存在します。その原因として次の2つが挙げられます。

#### 1. 説明変数の数 $p$ がサンプルサイズ $n$よりも多いとき ($n<p$)
要は連立方程式を解いているわけなので、求める未知数の数が式の数よりも多いと解は求まりません。そんな場合あるの？と思うでしょうが、例えば遺伝子を取り扱う場合、遺伝子数（説明変数の数）が数千単位なのに被験者数（サンプルサイズ）が数十ということはよく起こります。

#### 2. $n>p$ だとしても、ある説明変数の値が他の変数の線形結合で表現できる場合（多重共線性がある場合）
例えば、降雨量を予想する際に$x _ 1$が摂氏、$x _ 2$が華氏となっている場合や家の価格を求めるときに$x _ 1$がメートルによる値、$x _ 2$がフィートによる値となっている場合、などが当てはまります。要は冗長な説明変数が入っている場合とも言えます。

### 解決策
#### 1. サンプルサイズを増やす
増やすことが可能であれば解決にはなります。

#### 2. 説明変数の数を減らす
要は適切なモデル選択をするということです。$p$ 値や自由度調節済み決定係数、情報量基準を見て変数を減らしたり、L1正則化 (Lasso)などによりスパース推定することも良いでしょう。多重共線性を発見するためにはVIF (variance inflation factor; 分散拡大係数)を見てやるとよいです。

#### 3. L2正則化 (ridge)する
#### 4. 一般化逆行列（疑逆行列）を使う
できれば1～3を用いて解決した方がよいです。「頑張って変数を削ったけど、特殊なケースだからサンプルサイズを変数の数より増やせない」などのやむを得ないときに使いましょう。一般化逆行列の良いところは数値が安定することです。

## 係数の最小二乗推定値は不偏推定量か
$$
\boldsymbol{\hat\beta}={(X ^ TX)} ^ {-1}X ^ T\boldsymbol{y} 
$$
によって推定した値ですが、これは不偏推定量になっているのでしょうか。答えから言えば誤差項について、$E[\boldsymbol{\varepsilon}]=0$が成り立っていれば、これは不偏推定量になります。

不偏推定量ってなんだっけと思っている人がいるかもしれないですが、簡単に言えば、**平均が真の値になる推定量のこと**を不偏推定量といいます。不偏推定量の条件は推定値$\hat \theta$について 

$$
E[\hat \theta]=\theta 
$$

が成り立つことです。なので、今回は$\color{red}{E[\boldsymbol{\hat\beta}]=\boldsymbol{\beta}}$を示せば目標達成できます。

それではこれを示してみましょう。重回帰モデルは 

$$
\boldsymbol{y}=X \boldsymbol{\beta}+\boldsymbol{\varepsilon} 
$$

と表されることを思い出すと、 

$$
\begin{aligned}
\boldsymbol{\hat\beta}&={(X ^ TX)} ^ {-1}X ^ T\boldsymbol{y}\\
&={(X ^ TX)} ^ {-1}X ^ T(X\boldsymbol{\beta}+\boldsymbol{\varepsilon})\\ &=\boldsymbol{\beta}+{(X ^ TX)} ^ {-1}X ^ T\boldsymbol{\varepsilon} 
\end{aligned}
$$

となります。よって、 

$$
\begin{aligned}
E[\boldsymbol{\hat\beta}]&=E[\boldsymbol{\beta}+{(X ^ TX)} ^ {-1}X ^ T\boldsymbol{\varepsilon}]\\ &=\boldsymbol{\beta}+{(X ^ TX)} ^ {-1}X ^ T\underbrace{E[\boldsymbol{\varepsilon}]} _ {=0}\\
&=\boldsymbol{\beta} 
\end{aligned}
$$

が成り立つので、$\boldsymbol{\hat\beta}$は$\boldsymbol{\beta}$の不偏推定量です。

## 係数の推定値の標準誤差
まず仮定として重回帰モデル 

$$
\boldsymbol{y}=X \boldsymbol{\beta}+\boldsymbol{\varepsilon} 
$$

の誤差項 $\boldsymbol{\varepsilon}$は多変量正規分布 $\mathcal{N}(0, \sigma ^ 2
I _ n)$に従うとします。ただし、$\sigma$は未知の定数、$I _ n$は$n$次の単位行列です。分散共分散行列が$\sigma ^ 2 I _ n$となっている意味ですが、それぞれのサンプルにおける誤差は「独立だが同じ分布」から発生しているものと考えるためです。そのため、共分散は全て0になっています。

係数の推定値の標準誤差を求めましょう。まずは係数の推定値の分散$\textrm{Var}[\boldsymbol{\hat\beta}]$を計算します。分散共分散行列の定義より、 

$$
\textrm{Var}[\boldsymbol{\hat\beta}]=E\left[\left(\boldsymbol{\hat\beta}-E[\boldsymbol{\hat\beta}]\right)\left(\boldsymbol{\hat\beta}-E[\boldsymbol{\hat\beta}]\right) ^ {T}\right] 
$$

となります。ここで、
$$
\begin{align*} \boldsymbol{\hat\beta}-E[\boldsymbol{\hat\beta}]&={(X ^ TX)} ^ {-1}X ^ T\boldsymbol{y}-\boldsymbol{\beta}\ \ \ (\because\ E[\boldsymbol{\hat\beta}]=\boldsymbol{\beta})\\
&={(X ^ TX)} ^ {-1}X ^ T(X \boldsymbol{\beta}+\boldsymbol{\varepsilon})-\boldsymbol{\beta}\\ &={(X ^ TX)} ^ {-1}X ^ T\boldsymbol{\varepsilon} \end{align*}
$$

が成り立つので、 

$$
\begin{align*}
\textrm{Var}[\boldsymbol{\hat\beta}]&=E[({(X ^ TX)} ^ {-1}X ^ T\boldsymbol{\varepsilon})({(X ^ TX)} ^ {-1}X ^ T\boldsymbol{\varepsilon}) ^ T]\\
&=E[({(X ^ TX)} ^ {-1}X ^ T\boldsymbol{\varepsilon})(\boldsymbol{\varepsilon} ^ TX{(X ^ TX)} ^ {-1})]\ \ \ (\because {(AB)} ^ T=B ^ TA ^ T)\\
&={(X ^ TX)} ^ {-1}X ^ TE[\boldsymbol{\varepsilon}\boldsymbol{\varepsilon} ^ T]X{(X ^ TX)} ^ {-1}\\
&={(X ^ TX)} ^ {-1}X ^ T\sigma ^ 2I _ n X{(X ^ TX)} ^ {-1}\\ &=\sigma ^ 2{(X ^ TX)} ^ {-1}X ^ TX{(X ^ TX)} ^ {-1}\\
&=\sigma ^ 2{(X ^ TX)} ^ {-1} \end{align*}
$$

となります。

さて、$\sigma ^ 2$ですが、これは未知の定数です。なので、これの推定値を計算する必要があります。残差平方和 $S _ e=\|\boldsymbol{y}-X\boldsymbol{\beta}\|$を計算し、 

$$
{\hat\sigma} ^ 2=\frac{S _ e}{n-p-1}
$$

とします。これより、 

$$
\textrm{Var}[\boldsymbol{\hat\beta}]\approx\frac{S _ e}{n-p-1}{(X ^ TX)} ^ {-1} 
$$

となり、各係数$\hat\beta _ j$について 

$$
\textrm{Var}[\hat\beta _ j]=(\textrm{Var}[\boldsymbol{\hat\beta}]) _ {jj} 
$$

が成り立つので、標準誤差 $\textrm{SE}(\boldsymbol{\hat\beta})$は 

$$
\textrm{SE}(\boldsymbol{\hat\beta})=\left(\textrm{diag}\left[\textrm{Var}[\boldsymbol{\hat\beta}]\right]\right) ^ {\frac{1}{2}}
$$

と表せます。

## $t$ 値の求め方
t値は回帰係数がゼロと差があるかを検定するための統計量です。$t$値は 

$$
T(\hat\beta _ j)=\frac{\hat\beta _ j}{\textrm{SE}(\hat\beta _ j)} 
$$

と定義されます。アダマール除算(要素ごとの割り算; $A\oslash B:=[A _ {ij}/B _ {ij}]$)を用いて表記すると、 

$$
T(\boldsymbol{\hat\beta})=\boldsymbol{\hat\beta}\oslash\textrm{SE}(\boldsymbol{\hat\beta})
$$

となります。