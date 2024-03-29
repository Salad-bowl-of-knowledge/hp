---
toc: true
layout: post
description:
author: 優曇華院
comments: true
categories: [machine-learning]
title: Boltzmann Generatorsの解説
---

[Boltzmann Generatorsの論文](https://arxiv.org/abs/1812.01729)についての解説，というかざっくりしたメモ．機械学習とかニューラルネットワークとかは完全にエアプなんで間違ってたらゴメンね．RealNVPとかGANの話知ってると楽かも．

Boltzmann分布と正規分布を対応させる写像を，機械学習で作る．ただし，Boltzmann分布での低エネルギー状態が正規分布で原点にあるとする．

例えばタンパク質がopenとcloseの２形態を取ったとする．Boltzmann分布からサンプリングしようとすれば，普通はある安定な状態（今回はopenとする）から始める．openの状態から，タンパク質の側鎖の角度や長さなどに微妙な摂動を加えていって，エネルギーが低下するようなものをどんどん選んでいく．この時，openとcloseの間に準安定な領域が存在すれば，そこから抜け出せなくなって，サンプリングが詰む．

そこで，Boltzmann分布と結ばれた正規分布を考える．この正規分布では，低エネルギー状態は原点付近に存在する．正規分布の方でopenの点（原点付近）からスタートして，摂動を加えてサンプリング（原点付近）する．close（原点付近）が見つかるまでサンプリングできる．

## Boltzmann Generatorとは
ある系のconfigrationが$\boldsymbol{x}$で，その時のエネルギーが$U(\boldsymbol{x})$で表されるとすると，系がconfigration $\boldsymbol{x}$を取る確率はBoltzmann分布に従い，

$$
\begin{aligned}
  \exp\left(-\frac{U(\boldsymbol{x})}{kT_0}\right)=\exp(-u(\boldsymbol{x}))
\end{aligned}\tag{1}
$$

に比例する．ただ，全ての状態を数えるのは非現実的なので，平衡状態からニューラルネットワークを使って，one-shotでサンプリングすることを目標とする．latent spaceとしては，latent variable$\boldsymbol{z}$の正規分布$p_Z(\boldsymbol{z})$を使う．与えられたBoltzmann分布に近いconfigurationの確率分布$p_X(\boldsymbol{x})$と，$p_Z(\boldsymbol{z})$と$p_X(\boldsymbol{x})$の間の全単射$F_{zx}: \boldsymbol{z}\mapsto\boldsymbol{x}$を求めることが目標となる．

実際にBoltzmann分布でのサンプルを得るには，$p_X(\boldsymbol{x})$を重みづけすることが必要である．この場合，

$$
\begin{aligned}
  w(\boldsymbol{x})=\frac{e^{-u(\boldsymbol{x})}}{p_X(\boldsymbol{x})}
\end{aligned}\tag{2}
$$

で重みづけすればよい．

## 写像の構成
configration variable $\boldsymbol{x}$とlatent variable $z$の間には，次のような関係があるとする：

$$
\begin{aligned}
    \boldsymbol{z} &= \boldsymbol{F} _ {xz}(\boldsymbol{x}; \theta)\\
    \boldsymbol{x} &= \boldsymbol{F} _ {zx}(\boldsymbol{z}; \theta).
\end{aligned}\tag{3}
$$

もちろん， $\boldsymbol{F} _ {xz}=\boldsymbol{F} _ {zx}{}^{-1}$ ．これらの変換のJacobi行列を求めれば，

$$
\begin{aligned}
    J_{zx}(\boldsymbol{z}; \theta) &= \frac{\partial\boldsymbol{x}}{\partial\boldsymbol{z}} = \left(\frac{\partial \boldsymbol{F} _ {zx}}{\partial z_1},\dots,\frac{\partial \boldsymbol{F} _ {zx}}{\partial z_n}\right)(\boldsymbol{z}; \theta)\\
    J_{xz}(\boldsymbol{x}; \theta) &= \frac{\partial\boldsymbol{z}}{\partial\boldsymbol{x}} = \left(\frac{\partial \boldsymbol{F} _ {xz}}{\partial x_1},\dots,\frac{\partial \boldsymbol{F} _ {xz}}{\partial x_n}\right)(\boldsymbol{x}; \theta)
\end{aligned}\tag{4}
$$

となり，さらにJacobianを

$$
\begin{aligned}
    R_{xz}(\boldsymbol{x}; \theta) &= \left|\det J_{xz}(\boldsymbol{x}; \theta)\right|\\
    R_{zx}(\boldsymbol{x}; \theta) &= \left|\det J_{zx}(\boldsymbol{z}; \theta)\right|
\end{aligned}\tag{5}
$$

とする．

写像を$\boldsymbol{x}$から$\boldsymbol{z}$への体積非保存の「流れ」として考えれば，$p_X(\boldsymbol{x})\,dx=p_Z(\boldsymbol{z})\,dz$が成立するので，

$$
\begin{aligned}
    p_X(\boldsymbol{x};\theta) &= p_Z(\boldsymbol{z})\left|\frac{\partial\boldsymbol{z}}{\partial{x}}\right|\\
    &= p_Z(\boldsymbol{F} _ {xz}(\boldsymbol{x};\theta))R_{xz}(\boldsymbol{x};\theta)\\
    p_Z(\boldsymbol{z};\theta) &= p_X(\boldsymbol{x})\left|\frac{\partial\boldsymbol{x}}{\partial{z}}\right|\\
    &= p_X(\boldsymbol{F} _ {zx}(\boldsymbol{z};\theta))R_{zx}(\boldsymbol{z};\theta)
\end{aligned} \tag{6}
$$

となる．右辺の確率分布は$\theta$に依存しない（系がはじめから有している確率分布）が，左辺の計算結果は$\theta$に依存する（Boltzmann Generatorによって得られた確率分布）．

## RealNVPによるニューラルネットワーク構成
$F_{zx}$を直接求めることは困難なので，アフィンカップリングレイヤ（入出力の一部が比較的簡単な関係にある全単射）を考える．具体的には，$\boldsymbol{x}$を$(\boldsymbol{x} _ 1,\boldsymbol{x} _ 2)$，$\boldsymbol{z}$を$(\boldsymbol{z} _ 1,\boldsymbol{z} _ 2)$に分ける．これらについて，非線形変換を次のように定義する：

$$
\begin{aligned}
  & \boldsymbol{f} _ {xz}(\boldsymbol{x_1},\boldsymbol{x_2})
  \quad:
  \begin{cases}
    \boldsymbol{z} _ 1 = \boldsymbol{x} _ 1\\
    \boldsymbol{z} _ 2 = \boldsymbol{x} _ 2\odot\exp(\boldsymbol{S}(\boldsymbol{x} _ 1;\theta))+\boldsymbol{T}(\boldsymbol{x} _ 1;\theta)
  \end{cases};\\
  & \log R_{xz} = \sum_iS_i(\boldsymbol{x} _ 1;\theta).
\end{aligned}\tag{7}
$$

さらに，アフィンカップリングレイヤの逆写像は次の様になる：

$$
\begin{aligned}
  & \boldsymbol{f} _ {zx}(\boldsymbol{z_1},\boldsymbol{z_2})
  \quad:
  \begin{cases}
    \boldsymbol{x} _ 1 = \boldsymbol{z} _ 1\\
    \boldsymbol{x} _ 2 = (\boldsymbol{z} _ 2-\boldsymbol{T}(\boldsymbol{x_1};\theta))\odot\exp(-\boldsymbol{S}(\boldsymbol{z} _ 1;\theta))
  \end{cases};\\
  & \log R_{zx} = -\sum_iS_i(\boldsymbol{z} _ 1;\theta).
\end{aligned}\tag{8}
$$

このニューラルネットワークによって，合成写像$F_{zx}$が得られる．

## 機械学習のtraining
Boltzmann Generatorでは，主に２つの機械学習：training by energyとtraining by exampleを使う．

### training by energyの場合
1. latent spaceから正規分布$p_Z(\boldsymbol{z})$に従って適当に$\boldsymbol{z}$を選ぶ
2. $\boldsymbol{F} _ {zx}:\boldsymbol{z}\mapsto\boldsymbol{x}$ を使って$p_X(x)$を計算する
3. 生成された確率分布$p_X(x)$と目標のBoltzmann分布$e^{-u(\boldsymbol{x})}$との差を次のロスで評価する．

$$
\begin{aligned}
J_\text{KL}=E_{\boldsymbol{z}}[u(\boldsymbol{F} _ {zx}(\boldsymbol{z}))-\log R_{zx}(\boldsymbol{z})]
\end{aligned} \tag{9}
$$

$J_\text{KL}$を小さくすることが目標となる．

### training by exampleの場合
1. シミュレーションや観測結果から，実際に得られた$\boldsymbol{x}$を用意する．つまり，$e^{-u(\boldsymbol{x})}$が大きい．
2. $F_{xz}:\boldsymbol{x}\mapsto\boldsymbol{z}$で，これを$\boldsymbol{z}$に変換する．この$\boldsymbol{z}$が正規分布$p_Z(\boldsymbol{z})$から選ばれやすければよい．それは，次のロス

$$
\begin{aligned}
J_\text{ML}=E_{\boldsymbol{x}}\left[\frac{1}{2}\|\boldsymbol{F} _ {xz}(\boldsymbol{x})\|^2-\log R_{xz}(\boldsymbol{x})\right]
\end{aligned} \tag{10}
$$

を最小化することに等しい．$J_\text{ML}$の第１項は正規分布に対応する調和振動子のエネルギーを表す．

### 一般の場合
一般の場合にはBoltzmann Generatorでは合計のロス

$$
\begin{aligned}
J=w_\text{ML}J_\text{ML}+w_\text{KL}J_\text{KL}+w_\text{ML}J_\text{ML}
\end{aligned} \tag{11}
$$

を最小とすることが目標になる．

## KLロス関数
以下では，真の確率分布と学習によって得られた確率分布を区別する．すなわち，$\boldsymbol{x}$の真の確率分布$\mu_X(\boldsymbol{x})$は分配関数を$Z_X$とするBoltzmann分布である：

$$
\begin{aligned}
  \mu_X(\boldsymbol{x})=\frac{1}{Z_X}e^{-u(\boldsymbol{x})}.
\end{aligned}\tag{12}
$$

また，$\boldsymbol{z}$の真の確率分布$\mu_Z(\boldsymbol{z})$は正規分布である：

$$
\begin{aligned}
  \mu_Z(\boldsymbol{z})=\frac{1}{Z_Z}\exp\left(-\frac{\boldsymbol{z}^2}{2\sigma^2}\right).
\end{aligned}\tag{13}
$$

ただし，１つの温度しか考えない場合は$\sigma=1$とする．また，

$$
\begin{aligned}
  u_Z(\boldsymbol{z})=-\log\mu_Z(\boldsymbol{z})=\frac{1}{2\sigma^2}\boldsymbol{z}^2+C.
\end{aligned}\tag{14}
$$

とする．学習による$\boldsymbol{x}$，$\boldsymbol{z}$の確率分布をそれぞれ$q_X(\boldsymbol{x})$，$q_Z(\boldsymbol{z})$とする．これらについては，(6)式から，

$$
\begin{aligned}
    q_Z(\boldsymbol{z};\theta) &= \mu_X(\boldsymbol{F} _ {zx}(\boldsymbol{z};\theta))R_{zx}(\boldsymbol{z})\\
    q_X(\boldsymbol{x};\theta) &= \mu_Z(\boldsymbol{F} _ {xz}(\boldsymbol{x};\theta))R_{xz}(\boldsymbol{x})
\end{aligned}\tag{15}
$$

が成り立つ．

確率分布$p$，$q$に対して，Kullback-Leibler divergenceは次の式で定義される：

$$
\begin{aligned}
  \text{KL}(q \| p) &= \int q(\boldsymbol{x})[\log q(\boldsymbol{x})-\log p(\boldsymbol{x})]\,d\boldsymbol{x}\\
                           &= -H_q(\boldsymbol{x})-\int q(\boldsymbol{x})\log p(\boldsymbol{x})\,d\boldsymbol{x}.
\end{aligned}\tag{16}
$$

よって，$\mu_Z$と$q_Z$のKullback-Leibler divergenceは

$$
\begin{aligned}
  &\text{KL} _ \theta(\mu_Z \| q_Z) = -H_Z-\int\mu_Z(\boldsymbol{z})\log q(\boldsymbol{z})\,d\boldsymbol{z}\\
  &= -H_Z-\int\mu_Z(\boldsymbol{z})[\log\mu_X(\boldsymbol{F} _ {zx}(\boldsymbol{z};\theta)\\
  &\qquad\qquad+\log R_{zx}(\boldsymbol{z};\theta))]\,d\boldsymbol{z}\\
  &= -H_Z-\int\mu_Z(\boldsymbol{z})\biggl[\log\frac{\exp(-u(\boldsymbol{F} _ {zx}(\boldsymbol{z};\theta)))}{Z_X}\\
  &\qquad\qquad+\log R_{zx}(\boldsymbol{z};\theta))\biggr]\,d\boldsymbol{z}\\
  &= -H_Z+\log Z_X\\
  &\qquad+E_{\boldsymbol{z}\sim\mu_Z(\boldsymbol{Z})}[u(F_{zx}(\boldsymbol{z};\theta))-\log R_{zx}(\boldsymbol{z};\theta)]
\end{aligned}\tag{17}
$$

この第３項が，(9)式の$J_\text{KL}$である．また，

$$
\begin{aligned}
  H_X &= -\int q_X(\boldsymbol{x};\theta)\log q_X(\boldsymbol{x};\theta)\,d\boldsymbol{x}\\
  &= -\int\mu_Z(F_{xz}(\boldsymbol{x};\theta))R_{xz}(\boldsymbol{x})\\
  &\qquad\qquad\times\log(\mu_Z(F_{xz}(\boldsymbol{x};\theta))R_{xz}(\boldsymbol{x}))\,d\boldsymbol{x}\\
  &= -\int\mu_Z(F_{xz}(\boldsymbol{x};\theta)) \left|\frac{\partial\boldsymbol{z}}{\partial\boldsymbol{x}} \right|\\
  &\qquad\qquad\times\log(\mu_Z(F_{xz}(\boldsymbol{x};\theta))R_{xz}(\boldsymbol{x}))\,d\boldsymbol{x}\\
  &= -\int\mu_Z(\boldsymbol{z})\log(\mu_Z(\boldsymbol{z})R_{zx}{}^{-1}(\boldsymbol{z}))\,d\boldsymbol{z}\\
  &= -\int\mu_Z(\boldsymbol{z})\log\mu_Z(\boldsymbol{z})\,d\boldsymbol{z}+E_{\boldsymbol{z}\sim\mu_Z(\boldsymbol{z})}
\end{aligned}\tag{18}
$$

なので，

$$
\begin{aligned}
  &\text{KL} _ \theta(\mu_Z \| q_Z) \\
  &= -H_Z+\log Z_X\\
  &\qquad+E_{\boldsymbol{z}\sim\mu_Z(\boldsymbol{Z})}[u(F_{zx}(\boldsymbol{z};\theta))-\log R_{zx}(\boldsymbol{z};\theta)]\\
  &= -H_X+\log Z_X+E_{\boldsymbol{z}\sim\mu_Z(\boldsymbol{z})}[u(\boldsymbol{F} _ {zx}(\boldsymbol{z};\theta))]\\
  &= -H_X+\log Z_X+E_{\boldsymbol{x}\sim\mu_X(\boldsymbol{x})}[u(\boldsymbol{x})].
\end{aligned}\tag{19}
$$

同様に，

$$
\begin{aligned}
  &\text{KL} _ \theta(q_X \| \mu_X) \\
  &= -H_X-\int q_X(\boldsymbol{x};\theta)\log\mu_X(\boldsymbol{x})\,d\boldsymbol{x}\\
  &= -H_X-\int\mu_Z(\boldsymbol{F} _ {xz}(\boldsymbol{x};\theta))R_{xz}(\boldsymbol{x})\log\mu_X(\boldsymbol{x})\,d\boldsymbol{x}\\
  &= -H_X+\log Z_X+E_{\boldsymbol{z}\sim\mu_Z(\boldsymbol{z})}[u(\boldsymbol{F} _ {zx}(\boldsymbol{z};\theta))]\\
  &= -H_X+\log Z_X+E_{\boldsymbol{x}\sim\mu_X(\boldsymbol{x})}[u(\boldsymbol{x})].
\end{aligned}\tag{20}
$$

以上から，

$$
\begin{aligned}
  \text{KL} _ \theta(\mu_Z \| q_Z)=\text{KL} _ \theta(q_X\| \mu_X).
\end{aligned}\tag{21}
$$

さらに，$U=E_{\boldsymbol{x}\sim\mu_X(\boldsymbol{x})}[u(\boldsymbol{x})]$として，

$$
\begin{aligned}
  J_\text{KL}=U-H_X+H_Z
\end{aligned}\tag{22}
$$

が得られる．この式を参考にすれば，(9)式で定義した$J_\text{KL}$の表式において，第１項は系の内部エネルギーを，第２項は自由エネルギーに対するエントロピーの寄与を表すことが分かる．

(2)式でも述べたように，重み付けは，

$$
\begin{aligned}
  w(\boldsymbol{x}) &= \frac{\mu_X(\boldsymbol{x})}{q_X(\boldsymbol{x})}=\frac{q_Z(\boldsymbol{z})}{\mu_Z(\boldsymbol{z})}\\
  &= \frac{Z_X\exp(-u(\boldsymbol{x}))}{\mu_Z(\boldsymbol{z})R_{xz}(\boldsymbol{x})}\\
  &\propto \exp(-u(\boldsymbol{F} _ {zx}(\boldsymbol{x};\theta))+u_Z(\boldsymbol{z})+\log R_{zx}(\boldsymbol{z}))
\end{aligned}\tag{23}
$$

となる．これによって，平衡状態でのある量$A(\boldsymbol{x})$の期待値は

$$
\begin{aligned}
  E(A)\approx\frac{\sum_{i=1}^Nw(\boldsymbol{x}A(\boldsymbol{x}))}{\sum_{i=1}^Nw(\boldsymbol{w})}
\end{aligned}\tag{24}
$$

で与えられる．さらに，

$$
\begin{aligned}
  &\min\text{KL} _ \theta(\mu_Z\| q_Z)\\
  &= \min E_{\boldsymbol{z}\sim\mu_Z(\boldsymbol{Z})}[u(\boldsymbol{F} _ {zx}(\boldsymbol{z};\theta))-\log R_{zx}(\boldsymbol{z};\theta)]\\
  &= \min E_{\boldsymbol{z}\sim\mu_Z(\boldsymbol{Z})}[u(\boldsymbol{F} _ {zx}(\boldsymbol{x};\theta))-u_Z(\boldsymbol{z})-\log R_{zx}(\boldsymbol{z})]\\
  &= \min E_{\boldsymbol{z}\sim\mu_Z(\boldsymbol{Z})}[\log\mu_Z(\boldsymbol{z})-\log q_Z(\boldsymbol{z};\theta)]\\
  &= \max E_{\boldsymbol{z}\sim\mu_Z(\boldsymbol{Z})}[\log w(\boldsymbol{x})].
\end{aligned}\tag{25}
$$

## MLロス関数
$\text{KL} _ \theta(\mu_Z \| q_Z)=\text{KL} _ \theta(q_X\| \mu_X)$であったが，次のKLロスを考える：

$$
\begin{aligned}
  &\text{KL} _ \theta(\mu_X \| q_X) = -H_X-\int\mu_X(\boldsymbol{x})\log q_X(\boldsymbol{x};\theta)\,d\boldsymbol{x}\\
  &= -H_X-\int\mu_X(\boldsymbol{x})[\log\mu_Z(\boldsymbol{F} _ {xz}(\boldsymbol{x};\theta))+\log R_{xz}(\boldsymbol{x})]\,d\boldsymbol{x}\\
  &= -H_X+\log Z_Z\\
  &\qquad+E_{\boldsymbol{x}\sim\mu_X(\boldsymbol{x})}\left[\frac{1}{2\sigma^2}\|\boldsymbol{F} _ {xz}(\boldsymbol{x};\theta)\|^2-\log R_{xz}(\boldsymbol{x})\right].
\end{aligned}\tag{26}
$$

training by exampleを実行する場合，そのexampleが$\mu_X(\boldsymbol{x})$に従ったものかは分からないので，このロスを評価することは困難である．

代わりに，サンプルの分布$\rho(\boldsymbol{x})$を使って，

$$
\begin{aligned}
  J_\text{ML} &= -E_{\boldsymbol{x}\sim\rho}[\log q_X(\boldsymbol{x};\theta)]\\
  &= E_{\boldsymbol{x}\sim\rho(\boldsymbol{x})}\left[\frac{1}{2\sigma^2}\|\boldsymbol{F} _ {xz}(\boldsymbol{x};\theta)\|^2-\log R_{xz}(\boldsymbol{x})\right]
\end{aligned}\tag{27}
$$

を考える．これを最小にすることは，サンプル$\rho(\boldsymbol{x})$が正規分布で選ばれる確率が最大になることに対応する．通常は$\sigma=1$なので，(10)式​が得られる．

## RCロス関数
configration spaceで定義されたreaction cordinate $r(\boldsymbol{x})$を使う場合は，次のRCロス関数を考える：

$$
\begin{aligned}
  J_\text{RC} &= \int p(r(\boldsymbol{x}))\log p(r(\boldsymbol{x}))\,dr(\boldsymbol{x})\\
  &= E_{\boldsymbol{x}\sim q_X(\boldsymbol{x})}\log p(r(\boldsymbol{x})).
\end{aligned}\tag{28}
$$

$p(r(\boldsymbol{x}))$は，上限と下限でのカーネル密度推定として計算される．

## 複数の温度を扱う場合
基準の温度の$\tau_k$倍の温度について計算する場合，$u(\boldsymbol{x})$は$1/\tau_k$倍，$\sigma^2$は$\tau_k$倍とする．
