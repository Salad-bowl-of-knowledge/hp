---
toc: true
layout: post
description:
author: 優曇華院
comments: true
categories: [statistics]
title: scGenの解説
---
# Principal Curve（主曲線）
## Principal ComponentとPrincipal Curve
$\boldsymbol{R}^d$内で$N$個のデータがどのように分布しているかを，比較的簡単な式で表したい（次元を減らしたい）時がある．一つのよく知られた方法は，線形回帰である．これは，線形関数$f$を用いて，$f(x_1,\ldots,\check{x}_i,\ldots,x_d)$の式を考え，実際のデータとの差の分散
$$\begin{aligned}
  \frac{1}{N}\sum_{k=1}^N[x_i-f(x_1,\ldots,\check{x}_i,\ldots,x_d)]^2
\end{aligned}$$
が最小となるようにする．これは，$\boldsymbol{R}^d$内で，$x_i$軸に沿った，データ点と直線の距離の２乗平均を最小にするのに等しい（図\ref{linearity_regression}）．

\begin{figure}[ht]
  \centering
  \includegraphics[height=4cm]{PCuA_fig1.pdf}
  \caption{線形回帰}
  \label{linearity_regression}
\end{figure}

これに対し，主成分分析では，同じく直線を考えるが，その直線に関する成分の２乗和
$$\begin{aligned}
  \sum_{k=1}^N \boldsymbol{x}\cdot\boldsymbol{e}_1
\end{aligned}$$
が最大となる単位ベクトル$\boldsymbol{e}_1$を方向ベクトルとする直線を第１主成分とする．ここで，各データ点と直線の距離を$d(\boldsymbol{x}_k)$とすれば，
$$\begin{aligned}
  d(\boldsymbol{x}_k)^2=\|\boldsymbol{x}\|^2-\boldsymbol{x}\cdot\boldsymbol{e}_1
\end{aligned}$$
なので，主成分分析は各データ点との距離の２乗平均
$$\begin{aligned}
  \frac{1}{N}\sum_{k=1}^N d(\boldsymbol{x}_k)^2
\end{aligned}$$
が最小となるような直線を選ぶことに等しい（図）．

Principal Curve Analysisはデータ点を曲線で代表する方法である．各データ点とその曲線上への射影点との距離の２乗平均が最小となるような曲線を考える（図）.

## Principal Curveの定義
$\boldsymbol{R}^p$内に確率密度$h$に従って分布した点を$\boldsymbol{X}$で表す．$E(\boldsymbol{X})=0$としても一般性を失わない．$C^\infty$級曲線$\boldsymbol{f}:\boldsymbol{R}\supset\Lambda\to\boldsymbol{R}^p$を考える．ただし$\boldsymbol{f}$は自己交叉しないものとする：
$$\begin{aligned}
  \lambda_1\neq\lambda_2\Rightarrow\boldsymbol{f}(\lambda_1)\neq\boldsymbol{f}(\lambda_2).\label{no-self_crossing}
\end{aligned}$$
また，パラメータ$\forall\lambda\in\Lambda$について$\boldsymbol{f}$は単位速さであるとする：
$$\begin{aligned}
  \|\boldsymbol{f}'(\lambda)\|=1.
\end{aligned}$$

$\boldsymbol{X}$の$\boldsymbol{f}$に関するprojection index $\lambda_{\boldsymbol{f}}:\boldsymbol{R}^p\to\boldsymbol{R}$を次のように定義する：
$$\begin{aligned}
  \lambda_{\boldsymbol{f}}(\boldsymbol{x})=\sup_\lambda\left\{\lambda\mid\|\boldsymbol{x}-\boldsymbol{f}(\lambda)\|=\inf_\mu\|\boldsymbol{x}-\boldsymbol{f}(\mu)\|\right\}.
\end{aligned}$$
すなわち，$\boldsymbol{x}$の$\boldsymbol{f}$に関するprojection index $\lambda_{\boldsymbol{f}}(\boldsymbol{x})$は，$\boldsymbol{x}$に最も近い$\boldsymbol{f}$の点に対応するパラメータのうち最大のものである．

\begin{dfn}
  曲線$\boldsymbol{f}$をself-consistentである，もしくはprincipal curveであるとは，全ての$\lambda\in\Lambda$に対し，
  $$\begin{aligned}
    E(\boldsymbol{X}\mid\lambda_{\boldsymbol{f}}(\boldsymbol{X})=\lambda)=\boldsymbol{f}(\lambda)
  \end{aligned}$$
  が成立することである(Hastie and Stuetzle, 1989)．
\end{dfn}
つまり，projection index $\lambda_{\boldsymbol{f}}(\boldsymbol{x})$によって$\lambda$に射影される$\boldsymbol{R}^p$の全ての点$\boldsymbol{X}$を考える．そのような$\boldsymbol{X}$の平均がちょうど$\boldsymbol{f}(\lambda)$になるのである．

Principal Curveは主成分の自然な拡張である．それは次の定理によって分かる：
\begin{thm}
  $\boldsymbol{u}_0\cdot\boldsymbol{v}_0=0$とすると，直線$l(\lambda)=\boldsymbol{u}_0+\lambda\boldsymbol{v}_0$がself-consistentならば，それは主成分である．
\end{thm}
\begin{proof}
  projection indexの性質（最短距離を指すパラメータが複数ある場合は最大値を取る）によって，異なる$\lambda$の原像$\lambda_{\boldsymbol{f}}{}^{-1}(\lambda)$は交わらない．よって，$\{\boldsymbol{X}\}$は，projection indexによって$\lambda$に写る$\boldsymbol{X}$の集合の直和で表される：
  $$\begin{aligned}
    \{\boldsymbol{X}\}=\bigoplus_\lambda\{\boldsymbol{X}\mid\lambda_{\boldsymbol{f}}(\boldsymbol{X})=\lambda\}.
  \end{aligned}$$



  よって，$\boldsymbol{X}$の平均は，ある$\lambda$にprojection indexによって写される$\boldsymbol{X}$の平均$\boldsymbol{X}_\lambda=E(\boldsymbol{X}\mid\lambda_{\boldsymbol{f}}(\boldsymbol{X})=\lambda)$の$\lambda$に関する平均に等しい：
  $$\begin{aligned}
    \begin{split}
      0=E(\boldsymbol{X}) &= E_\lambda E(\boldsymbol{X}\mid\lambda_{\boldsymbol{f}}(\boldsymbol{X})=\lambda)\\
      &= E_\lambda(\boldsymbol{u}_0+\lambda\boldsymbol{v}_0)\\
      &= \boldsymbol{u}_0+\bar{\lambda}\boldsymbol{v}_0.
    \end{split}
  \end{aligned}$$
  両辺$\boldsymbol{u}_0$で内積を取れば，$\boldsymbol{u}_0\cdot\boldsymbol{v}_0=0$なので
  $$\begin{aligned}
    \boldsymbol{u}_0=0.
  \end{aligned}$$
  projection indexによって$\lambda$に写る$\boldsymbol{X}$について$\boldsymbol{u}_0$，つまり$l$は原点を通るので，
  $$\begin{aligned}
    \lambda_{\boldsymbol{f}}(\boldsymbol{X}) = \boldsymbol{X}\cdot\boldsymbol{v}_0 = \boldsymbol{X}^t\boldsymbol{v}_0.
  \end{aligned}$$
  $\boldsymbol{X}$の共分散行列を$\Sigma$とすれば，
  $$\begin{aligned}
    \Sigma=E\left[(\boldsymbol{X}-E(\boldsymbol{X}))(\boldsymbol{X}-E(\boldsymbol{X}))^t\right]=E(\boldsymbol{X}\boldsymbol{X}^t)
  \end{aligned}$$
  となるので，
  $$\begin{aligned}
    \begin{split}
      \Sigma\boldsymbol{v}_0 &= E(\boldsymbol{X}\boldsymbol{X}^t)\boldsymbol{v}_0\\
      &= E_\lambda E(\boldsymbol{X}\boldsymbol{X}^t\boldsymbol{v}_0\mid\lambda_{\boldsymbol{f}}(\boldsymbol{X})=\lambda)\\
      &= E_\lambda E(\boldsymbol{X}\boldsymbol{X}^t\boldsymbol{v}_0\mid\boldsymbol{X}^t\boldsymbol{v}_0=\lambda)\\
      &= E_\lambda E(\lambda\boldsymbol{X}\mid\boldsymbol{X}^t\boldsymbol{v}_0=\lambda)\\
      &= E_\lambda\lambda^2\boldsymbol{v}_0.
    \end{split}
  \end{aligned}$$
\end{proof}

## projection indexの存在
\begin{lem}\label{compact_Q}
  全ての$\boldsymbol{x}\in\boldsymbol{R}^p$と$r>0$に対し，集合$Q\{\lambda\mid\|\boldsymbol{x}-\boldsymbol{f}(\lambda)\|\leq r\}$はコンパクトである．
\end{lem}
\begin{proof}
  $Q$が$\boldsymbol{R}^{p}$の有界な閉集合であることが言えれば良い．

  $\|\boldsymbol{x}-\boldsymbol{f}(\lambda)\|$は$\lambda$の連続関数で，値域が閉球体$B^\ast(0,r)$であるので，その原像$Q$も閉集合である．

  $Q$が有界でないと仮定する．$\|\boldsymbol{x}-\boldsymbol{f}(\lambda_i)\|\leq r$を満たす点列$\lambda_1,\ldots$が存在する．$B^\ast(\boldsymbol{x},2r)$を考える．$\boldsymbol{f}$は$\lambda_i$から$\lambda_{i+1}$の間で，ずっと$B^\ast(\boldsymbol{x},2r)$の中にあるか，一度$B^\ast(\boldsymbol{x},2r)$を出た後，再び入ってくる．何れにせよ，$\boldsymbol{f}$は単位速さであったから，$\boldsymbol{f}(\lambda_i)$から$\boldsymbol{f}(\lambda_i{i+1})$までの曲線の長さは少なくとも$\min(2r,\lambda_{i+1}-\lambda_i)$である．よって，仮定により$\{\lambda_n\}_{n\in\boldsymbol{N}}$が$B(\boldsymbol{x},2r)$内で無限長を持つことになり，矛盾である．
\end{proof}

\begin{lem}\label{existence_of_parameter_set}
  全ての$\boldsymbol{x}\in\boldsymbol{R}^p$に対し，$\|\boldsymbol{x}-\boldsymbol{f}(\lambda)\|=\inf_{\mu\in\Lambda}\|\boldsymbol{x}-\boldsymbol{f}(\mu)\|$なる$\lambda\in\Lambda$が存在する．
\end{lem}
\begin{proof}
  $r=\inf_{\mu\in\Lambda}\|\boldsymbol{x}-\boldsymbol{f}(\mu)\|$，$B=\{\mu\mid\|\boldsymbol{x}-\boldsymbol{f}(\mu)\|\leq2r\}$とする．$B$は空でなく，コンパクトなので，最大値・最小値が存在する．よって，$\inf_{\mu\in\Lambda}\|\boldsymbol{x}-\boldsymbol{f}(\mu)\|=\inf_{\mu\in B}\|\boldsymbol{x}-\boldsymbol{f}(\mu)\|$が成立する．
\end{proof}
$d(\boldsymbol{x},\boldsymbol{f})=\inf_{\mu\in\Lambda}\|\boldsymbol{x}-\boldsymbol{f}(\mu)\|$とする．
\begin{thm}
  projection index $\lambda_{\boldsymbol{f}}(\boldsymbol{x})=\sup_\lambda\{\lambda\mid\|\boldsymbol{x}-\boldsymbol{f}(\lambda)\|=d(\boldsymbol{x},\boldsymbol{f})\}$はwell-definedである．
\end{thm}
\begin{proof}
  $\{\lambda\mid\|\boldsymbol{x}-\boldsymbol{f}(\lambda)\|=d(\boldsymbol{x},\boldsymbol{f})\}$は補題\ref{existence_of_parameter_set}から空でなく，補題\ref{compact_Q}からコンパクトと分かる．
\end{proof}

## ambiguity pointの集合の測度
\begin{lem}\label{hyperplane}
  $\boldsymbol{x}$の最近点が$\boldsymbol{f}(\lambda_0)\ (\lambda_0\in\Lambda_0)$（ただし，$\lambda_0$はパラメータ範囲$\Lambda_0$の端点ではないとする）なら，$\boldsymbol{x}$は超平面$(\boldsymbol{x}-\boldsymbol{f}(\lambda_0))\cdot\boldsymbol{f}'(\lambda_0)$上にある
\end{lem}
\begin{proof}
  $\boldsymbol{f}(\lambda_0)$は$\boldsymbol{x}$からの距離が最短なので，
  $$\begin{aligned}
    \begin{split}
      0 &= \frac{d}{d\lambda}\|\boldsymbol{x}-\boldsymbol{f}(\lambda)\|^2 \mid_{\lambda_0}\\
      &= -2(\boldsymbol{x}-\boldsymbol{f}(\lambda_0))\cdot\boldsymbol{f}'(\lambda_0).
    \end{split}
  \end{aligned}$$
\end{proof}

\begin{dfn}
  最近点が複数ある（$\text{card}\{\lambda\mid\|\boldsymbol{x}-\boldsymbol{f}(\lambda)\|=d(\boldsymbol{x},\boldsymbol{f})\}>1$である）点$\boldsymbol{x}$をambiguity pointと呼ぶ．
\end{dfn}

$M_\lambda=\{\boldsymbol{x}\mid(\boldsymbol{x}-\boldsymbol{f}(\lambda))\cdot\boldsymbol{f}'(\lambda)=0\}$とする．補題\ref{hyperplane}から，$\boldsymbol{f}(\lambda)$が$\boldsymbol{x}$の最近点で$\lambda\in\Lambda_0$なら$\boldsymbol{x}\in M_\lambda$である．

全ての$\lambda$について，$\boldsymbol{f}'(\lambda)$と直交するよう$p-1$個の場クトル場$\boldsymbol{n}_1(\lambda),\ldots,\boldsymbol{n}_{p-1}(\lambda)$を考える．$\boldsymbol{\chi}:\Lambda\times\boldsymbol{R}^{p-1}\to\boldsymbol{R}^p$を次の様に定める：
$$\begin{aligned}
  \boldsymbol{\chi}(\lambda,\boldsymbol{v})=\boldsymbol{f}(\lambda)+\sum_{i=1}^{p-1}v_i\boldsymbol{n}_i(\lambda).\label{def_chi}
\end{aligned}$$
さらに，$M=\boldsymbol{\chi}(\Lambda,\boldsymbol{R}^{p-1})=\bigcup_\lambda M_\lambda$を，曲線上の点で，曲線と直交する超平面に属する点の集合とする．

\begin{dfn}
  $X$の部分集合族$\mathcal{F}\in2^X$が$\sigma$加法族であるとは，次の性質を満たすことである：

1. $A\in\mathcal{F}\Rightarrow A^c\in\mathcal{F}$；
    \item $n\in\boldsymbol{N}$に対し，$A_n\in\mathcal{F}\Rightarrow\bigcup_{n\in\boldsymbol{N}}A_n\in\mathcal{F}$．
    \end{enumerate}
\end{dfn}
$\sigma$加法族の定義から直ちに次のことが証明される：
\begin{thm}

1. $\phi, X\in\mathcal{F}$．
2. $n\in\boldsymbol{N}$に対し，$A_n\in\mathcal{F}\Rightarrow\bigcap_{n\in\boldsymbol{N}}A_n\in\mathcal{F}$．
3. $A,B\Rightarrow A\cup B\in\mathcal{F}$．
4. $A,B\Rightarrow A\cap B\in\mathcal{F}$．

\end{thm}
\begin{dfn}
  $\mathcal{F}$を$X$の$\sigma$加法族とする．$\mu:\mathcal{F}\to\boldsymbol{R}\cup\{+\infty\}$が測度であるとは次の性質を満たすことである：
1. $A\in\mathcal{F}$に対し$0\leq\mu(A)\leq+\infty$；
2. $A_i,A_j\in\mathcal{F}\ (i\neq j\in\boldsymbol{N})$に対し，$A_i\cap A_j=\varnothing\Rightarrow\mu(\bigcup_{i\in\boldsymbol{N}}A_i)=\sum_{i=1}^\infty\mu(A_i)$．

\end{dfn}
測度の定義から直ちに次のことが証明される：
\begin{thm}

1. $\mu(\varnothing)=0$．
2. $A,B\in\mathcal{F},\quad A\subset B\Rightarrow \mu(A)\leq\mu(B)$．
3. $A,B\in\mathcal{F},\quad A\subset B,\quad\mu(B)<+\infty\Rightarrow \mu(B\backslash A)=\mu(B)-\mu(A)$．
4. $A_i\in\mathcal{F}\ (i\in\boldsymbol{N})$に対し，$\mu(\bigcup_{i\in\boldsymbol{N}}A_i)\leq\sum_{i=1}^\infty\mu(A_i)$．
\end{thm}

\begin{lem}\label{measure_of_A_Mc}
  $M$に含まれないambiguity pointの（$p$次元）測度は$0$である：$\mu(A\cap M^c)$．
\end{lem}

\begin{proof}
  $\boldsymbol{x}\in A\cap M^c$とする．補題\ref{hyperplane}から，このような点$\boldsymbol{x}$が存在するのは，$\Lambda=[\lambda_\text{min},\lambda_\text{max}]$であり，$\boldsymbol{x}$が端点$\boldsymbol{f}(\lambda_\text{min})$, $\boldsymbol{f}(\lambda_\text{max})$から等距離にあり，かつそれが曲線$\boldsymbol{f}$との最短である時のみである．これは測度$0$の超平面を形成する．
\end{proof}

\begin{lem}\label{ambiguity_point_neighbor}
  $E$を測度$0$の集合とする．ambiguity pointの集合$A$の測度が$0$であるためには，$\forall\boldsymbol{x}\in\boldsymbol{R}^p\backslash E$に対し，$\mu(A\cap N(\boldsymbol{x}))$なる開近傍$N(\boldsymbol{x})$が存在することが十分である．
\end{lem}

\begin{proof}
  開被覆$\{N(\boldsymbol{x})\mid\boldsymbol{x}\in\boldsymbol{R}^p\backslash E\}$は明らかに$\bar{A}$を被覆する．$\bar{A}$は$\boldsymbol{R}^p$の有界閉集合なのでコンパクトである．よって，$\{N(\boldsymbol{x})\mid\boldsymbol{x}\in\boldsymbol{R}^p\backslash E\}$の中から有限個の被覆$\{N_i\}$を選んで，$A\subset\bar{A}\subset\bigcup_{i=1}^k N_i$とできる．よって，$\mu(A)\leq\mu(\bigcup_{i=1}^kN_i\cap A)\leq\sum_{i=1}^{k}\mu(N_i\cap A)=0$．
\end{proof}
\begin{lem}\label{only_compact_case}
  $\Lambda$がコンパクトな場合のみ考えても一般性を失わない．
\end{lem}

\begin{proof}
  $\Lambda_n=[-n,n]$, $\boldsymbol{f}_n=\boldsymbol{f}\mid\Lambda_n$とし，$A_n$を$\boldsymbol{f}_n$のambiguity pointとする．$\boldsymbol{x}$を$\boldsymbol{f}$のambiguity pointとする．補題\ref{compact_Q}から$\{\lambda\mid\|\boldsymbol{x}-\boldsymbol{f}(\lambda)\|=d(\boldsymbol{x},\boldsymbol{f})\}$はコンパクト，つまり$\boldsymbol{R}$の有限閉集合である．

  よって，ある$n$が存在し，$\{\lambda\}\in\Lambda_n$，$\boldsymbol{x}\in A_n$，$A\subset\bigcup_{n=1}^\infty A_n$が成立する．ここで，$\mu(A)\leq\mu(\bigcup_{n=1}^\infty A_n)\leq\sum_{n=1}^\infty\mu(A_i)$となる．

  コンパクトな$\Lambda_n$を考えて，$\boldsymbol{f}_n$のambiguity pointの集合$A_n$について，$\mu(A_n)=0$を示せばよい．
\end{proof}

\begin{dfn}
  写像$f:\boldsymbol{R}^m\to\boldsymbol{R}^n$について，$\boldsymbol{y}\in\boldsymbol{R}^n$が正則値であるとは，$\forall\boldsymbol{x}\in f^{-1}(\boldsymbol{y})$について，$f$の微分$f'$の階数が$p$となることである：$\text{rank}(f'(\boldsymbol{x}))=p$．そうでなければ，$\boldsymbol{y}$は臨界値であると言う．
\end{dfn}

\begin{thm}[Sardの定理]
  写像$f:\boldsymbol{R}^m\to\boldsymbol{R}^n$について$f$の微分可能性が十分高ければ，$f$の臨界値の集合$C$の測度は$0$である．
\end{thm}

\begin{lem}
  \ref{def_chi}で定義した$\boldsymbol{\chi}$の臨界値の集合$C$の測度は$0$である．
\end{lem}

\begin{proof}
  $\boldsymbol{\chi}$は$C^\infty$級なので，Sardの定理を使う．
\end{proof}

\begin{thm}
  ambiguity pointの集合$A$の測度は$0$である：$\mu(A)=0$．
\end{thm}

\begin{proof}
  補題\ref{only_compact_case}から$\Lambda$がコンパクトの場合のみ考える．補題\ref{measure_of_A_Mc}から$\mu(A\cap M^c)=0$．$\boldsymbol{\chi}$の臨界値の集合$C\subset M$を考える．この時，$\mu((A\cap M^c)\cup C)=0$なので，補題\ref{measure_of_A_Mc}から，$\forall\boldsymbol{x}\in\boldsymbol{R}^p\backslash((A\cap M^c)\cup C)=M\backslash C+M^c\cap A^e$について，$\mu(A\cap N(\boldsymbol{x}))=0$なる近傍$N(\boldsymbol{x})$が存在することを証明すればよい．

  $A$の外部$A^e$については自明なので，以下では$\forall\boldsymbol{x}\in M\backslash C$（正則値）について，$\mu(A\cap N(\boldsymbol{x}))=0$なる近傍$N(\boldsymbol{x})$が存在することを証明する．

  $\boldsymbol{\chi}^{-1}\subset\Lambda\times\boldsymbol{R}^{p-1}$は有限集合$\{(\lambda_1,\boldsymbol{v_1}),\cdots,(\lambda_k,\boldsymbol{v}_k)\}$である．

  仮に，無限集合であったと仮定する．この時，$\boldsymbol{\chi}(\xi_i,\boldsymbol{w}_i)=\boldsymbol{x}$を満たす$\Lambda\times\boldsymbol{R}^{p-1}$の部分集合$\{(\xi_1,\boldsymbol{w}_1),\ldots\}$が存在する．$\Lambda$はコンパクト，$\boldsymbol{\chi}$は連続なので，${\xi_1,\ldots}$の集積点$\xi_0$と対応する$\boldsymbol{w}_0$が存在し，$\boldsymbol{\chi}(\xi_0,\boldsymbol{w}_0)=\boldsymbol{x}$となる．$\boldsymbol{x}$は正則値なので，$\text{rank}(\boldsymbol{\chi}')=p$である．よって，$(\xi_0,\boldsymbol{w}_0)$と$\boldsymbol{x}$の近傍で（局所）微分同相になる．よって，これは$\xi_0$が集積点であるので矛盾．

  $\boldsymbol{\chi}$は正則値なので，$(\lambda_i,\boldsymbol{v}_i)$の近傍$L_i$と$\boldsymbol{x}$の近傍$N(\boldsymbol{x})$で微分同相になる．

  この時，$\tilde{N}(\boldsymbol{x})\subset N(\boldsymbol{x})$が存在して，$\boldsymbol{\chi}^{-1}(\tilde{N})\subset\bigcup_{i=1}^kL_i$が成立する．

  仮に上記のような$\tilde{N}$が存在しないと仮定する．この時，$\boldsymbol{x}$に収束する点列$\{\boldsymbol{x}_i\}$が存在し，$(\xi_i,\boldsymbol{w}_i)\notin\bigcup_{i=1}^kL_i$で，$\boldsymbol{\chi}(\xi_i,\boldsymbol{w}_i)=\boldsymbol{x}_i$が成立する．点列$\{\xi_i\}_{i\in\boldsymbol{N}}$は集積点$\xi_0\in\bigcup_{i\in\boldsymbol{N}}L_i$を持つ．しかし，これは$\boldsymbol{\chi}(\xi_0,\boldsymbol{w}_0)=\boldsymbol{x}$の連続性及び，$\boldsymbol{\chi}^{-1}(\boldsymbol{x})$の有限性から矛盾となる．

  以上から，$\boldsymbol{y}\in\tilde{N}(\boldsymbol{x})$に対し，$\boldsymbol{\chi}(\lambda_i(\boldsymbol{y}),\boldsymbol{v}_i(\boldsymbol{y}))=\boldsymbol{y}$を満たす$(\lambda_i(\boldsymbol{y}),\boldsymbol{v}_i(\boldsymbol{y}))$が各$L_i$に唯一存在する．ここで，$d_i(\boldsymbol{y})=\|\boldsymbol{y}-\boldsymbol{f}(\lambda_i(\boldsymbol{y}))\|^2$とする．補題\ref{hyperplane}から，
  $$\begin{aligned}
    \begin{split}
      & \text{grad}(d_i(\boldsymbol{y})) = \text{grad}\sum_{j=1}^p[y_j-f_j(\lambda_i(\boldsymbol{y}))]^2\\
      &= \left(\frac{\partial}{\partial y_1}\sum_{j=1}^p[y_j-f_j(\lambda_i(\boldsymbol{y}))]^2,\ldots \right)\\
      &= \sum_{j=1}^p\left(2[y_j-f_j(\lambda_i(\boldsymbol{y}))]\frac{\partial}{\partial y_1}[y_j-f_j(\lambda_i(\boldsymbol{y}))],\ldots \right)\\
      &= \left(\sum_{j=1}^p2[y_j-f_j(\lambda_i(\boldsymbol{y}))]\delta_{1j},\ldots\right)\\
      &\quad -\left(\sum_{j=1}^p2[y_j-f_j(\lambda_i(\boldsymbol{y}))]f^\prime_j(\lambda_i(\boldsymbol{y}))\frac{\partial\lambda_i}{\partial y_1},\ldots\right)\\
      &= \left(2[y_1-f_1(\lambda_i(\boldsymbol{y}))],\ldots\right)\\
      &\quad -2\frac{\partial\lambda_i}{\partial y_1}\left([\boldsymbol{y}-\boldsymbol{f}(\lambda_i(\boldsymbol{y}))]\cdot\boldsymbol{f}'(\lambda_i(\boldsymbol{y}))\right)\\
      %\left([\boldsymbol{y}-\boldsymbol{f}\(\lambda_i(\boldsymbol{y}))]\cdot\boldsymbol{f}'(\lambda_i(\boldsymbol{y})),\ldots\right)\\
      &= 2(\boldsymbol{y}-\boldsymbol{f}(\lambda_i(\boldsymbol{y}))).
    \end{split}\label{grad_d}
  \end{aligned}$$

  $\boldsymbol{y}\in\tilde{N}(\boldsymbol{x})$がambiguity pointになるのは，

  $$\begin{aligned}
      A_{ij} =\{\boldsymbol{z}\in\tilde{N}(\boldsymbol{x})\mid d_i(\boldsymbol{z})=d_j(\boldsymbol{z}),\lambda_i(\boldsymbol{z})\neq\lambda_j(\boldsymbol{z})\}
  \end{aligned}$$
  として，$\boldsymbol{y}\in A_{ij}\ (\exists i,j;i\neq j)$の時のみである．

  $\boldsymbol{f}$は自己交叉しない（$\eqref{no-self_crossing$}が成立する）ので，$\lambda_i(\boldsymbol{z})\neq\lambda_j(\boldsymbol{z})$とすると，$\eqref{grad_d}$から，
  $$\begin{aligned}
    \begin{split}
      \text{grad}(d_i(\boldsymbol{z})-d_j(\boldsymbol{z})) &= 2(\boldsymbol{f}(\lambda_j(\boldsymbol{z}))-\boldsymbol{f}(\lambda_i(\boldsymbol{z})))\\
      &\neq0.
    \end{split}
  \end{aligned}$$
  $A_{ij}$は$p-1$次元多様体であるので，その$p$次元測度は$0$となる：$\mu(A_{ij})=0$．よって，$\mu(A\cap\tilde{N})=\mu(\bigcup_{ij}A_{ij})\leq\sum_{ij}\mu(A_{ij})=0$．
\end{proof}

## bendingアルゴリズム
Principal curveは次のアルゴリズムによって求めることが出来る：
\begin{description}
  \item[initializaiton] $\boldsymbol{a}$を第１主成分として，$\boldsymbol{f}^{0}(\lambda)=\bar{\boldsymbol{x}}+\boldsymbol{a}\lambda$とする．$\lambda^{(0)}(\boldsymbol{x})=\lambda_{\boldsymbol{f}^{(0)}}(\boldsymbol{x})=0$とする．

1. $\boldsymbol{f}^{(j)}(\cdot)=\boldsymbol{E}(\boldsymbol{X}\mid\lambda_{\boldsymbol{f}^{(j-1)}}=\cdot)$とする
2. $\boldsymbol{f}^{(j)}(\lambda)$が単位速さとなるように調整する（曲線の形は変わらない）．
3. $\lambda^{(j)}(\boldsymbol{x})=\lambda_{\boldsymbol{f}^{(j)}}(\boldsymbol{x})\quad\forall\boldsymbol{x}\in h$とする．
4. $D^2(h,\boldsymbol{f}^{(j)})=E_\lambda E(\|\boldsymbol{X}-\boldsymbol{f}^{(j)}(\lambda^{(j)}(\boldsymbol{X}))\|^2\mid\lambda^{(j)}(\boldsymbol{X})=\lambda)$を計算する．

  \item[until] $D^2(h,\boldsymbol{f}^{(j)})$がある閾値以下になるまで繰り返す．
\end{description}
これはつまり，点$\{\boldsymbol{X}\}$と曲線の距離の２乗平均が極小値となる曲線を探している．

## 有限データ点のためのbendingアルゴリズム
$p$次元の$n$個のデータ点を考える．点が有限の場合，principal curveは$(\lambda_i,\boldsymbol{f}_i)$の$n$個の集合になる．曲線は単位速さであるとするので，$\lambda_i$は$\boldsymbol{f}_1$から$\boldsymbol{f}_i$までの折れ線に沿った距離とする．$\lambda_1=0$としておく．

まずは，projection index $\lambda_{\boldsymbol{f}^{(j)}}(\boldsymbol{x}_i)$を求める．$\boldsymbol{x}_i$と，$\boldsymbol{f}_k{}^{(j)}$と$\boldsymbol{f}_{k+1}{}^{(j)}$を両端とする線分の最短距離を$d_{ik}$とする．また，$\boldsymbol{f}_1{}^{(j)}$から最短距離を与える線分側の点までの折れ線に沿った距離を$\lambda_{ik}$とする．こうして，$1\leq k\leq n-1$に対し$d_{ik}$と$\lambda_{ik}$を求める．

$\boldsymbol{x}_i$のprojection index $\lambda_{i}$は$d_{ik}$の最小値を与える$\lambda_{ik}$とする：
$$\begin{aligned}
  \lambda_{i}=\lambda_{ik*},\quad k*=\text{arg}\min_{1\leq k\leq n-1}d_{ik}.
\end{aligned}$$

次に，projection indexが$\lambda_i$になるような点$\{\boldsymbol{x}\}$の平均を求めて，$\boldsymbol{f}^{(j+1)}(\lambda_i)$（つまり$\boldsymbol{f}_i{}^{(j+1)}$）を構成する．しかし，有限データ点の場合は$\lambda_i$に投影されるのは$\boldsymbol{x}_i$唯一という状況がほとんどである．よって，projection index $\lambda_k$が$\lambda_i$に近い点$\{\boldsymbol{x}_k\}$を取ってきて，それらの局所平均を取る(linear weighted running-line smoother)．

$\lambda_i$に近い$wn\ (0<w<1)$個の$\lambda_j$及び点$\{\boldsymbol{x}_j\}$に対し直線を，重み付け最小２乗法でフィッティングする．この際，各重み付けは$\lambda_i$で最大値を取って，$\lambda_i$との差が大きいほど$0$に近いもの，例えば，
$$\begin{aligned}
  w_{ij} = \left[1- \mid\frac{\lambda_j-\lambda_i}{\lambda_{wn \text{ th nearest}}-\lambda_i} \mid ^3\right]^3
\end{aligned}$$
などとする．$\{\boldsymbol{x}_j\}$の平均は，この直線$\boldsymbol{l}(\lambda)$の$\lambda_i$での値とする：
$$\begin{aligned}
  \boldsymbol{f}_i{}^{(j+1)} = \boldsymbol{l}(\lambda_i).
\end{aligned}$$

これによって$n$個の点$\{\boldsymbol{f}_i\}$が得られる．曲線の長さに従って$\{\lambda_{i}{}^{(j+1)}\}$を定めておく．

## k-sgementsアルゴリズム
上に述べたbendingアルゴリズムの他にもPrincipal Curveを求めるアルゴリズムはいくつかあるが，データの曲率が大きかったり，自己交叉があると使い物にならなくなる．それに対処出来るのが，k-segmentsアルゴリズムである(Verbeek, Vlassis, Kr\"{o}se, 2001)．

直線$s=\{\boldsymbol{s}(t)=\boldsymbol{c}+\boldsymbol{u}t\mid t\in\boldsymbol{R}\}$を考える．点$\boldsymbol{x}$との距離は
$$\begin{aligned}
  d(\boldsymbol{x},s)=\inf_{t\in\boldsymbol{R}}\|\boldsymbol{s}(t)-\boldsymbol{x}\|
\end{aligned}$$
で定義される．
\begin{dfn}
  $X_n$を$\boldsymbol{R}^d$から取ってきた$n$個のサンプルの集合とする時，Voronoi領域(VR) $V_1,\dots,V_k$を次のように定義する：
  $$\begin{aligned}
    V_i =\{\boldsymbol{x}\in X_n\mid i=\text{arg}\min_j d(\boldsymbol{x},s_j)\}.
  \end{aligned}$$
\end{dfn}
つまり，$V_i$は$i$番目の線$s_i$が最短となるような$X_n$の部分集合である．アルゴリズムの目標は全ての点の最短直線との距離の２乗和
$$\begin{aligned}
  \sum_{i=1}^k\sum_{\boldsymbol{x}\in V_i}d(\boldsymbol{x},s_i)^2\label{kmeans_square_sum}
\end{aligned}$$
を最小にするような$k$本の直線$s_1,\ldots,s_k$を見つけることである．

$\eqref{kmeans_square_sum}$を最小にするような$k$本の直線を見つけるためには，ランダムな向きと位置にある$k$本の直線を用意して，次のステップを繰り返せば良い：

1. それぞれの直線についてVRを決定する．
2. 直線をそれぞれのVRの第１主成分のベクトルで置き換える．

このアルゴリズムが収束することは次のことから分かる：

- $\eqref{kmeans_square_sum}$はVRの定義から，第１ステップで減少する．さらに，主成分の性質から，第２ステップでも減少する．
- $X_n$は有限個なので，VRの構成方法は有限である．

ここで，無限に長い直線ではアルゴリズムの計算量が増えるので，線分に限定する．すなわち，アルゴリズムを次のように変える：

1. それぞれの線分についてVRを決定する．
2. 線分をそれぞれのVRの第１主成分のベクトルで置き換える．そして，VRのデータ点の第１主成分の分散を$\sigma^2$として，VRの重心から$\frac{3}{2}\sigma$以内の範囲に存在する部分を新しい線分とする．

### $k$の決定
$k$を決めるには，$k=1$から初めて，ある条件（線分の上限数やパフォーマンスについての条件など）が満たされるまで増やしていけば良い．

新しい線分の挿入場所を決めるために，各データ点$\boldsymbol{x}_i$の所に長さ$0$の線分（つまり点$\boldsymbol{c}$）を追加する．この場合，点$\boldsymbol{x}$との最短距離は$d(\boldsymbol{x},s)=\|\boldsymbol{x}-\boldsymbol{c}\|$で与えられる．よって，この$0$長線分も追加した$k+1$本の成分でVRを構成し，$\eqref{kmeans_square_sum}$を計算する．

新しく追加した$0$長線分の中で，$\eqref{kmeans_square_sum}$を最も減らすような線分に関するVRを$V_{k+1}$とする．次に，$V_{k+1}$での第１主成分のベクトルから，平均からの距離$\frac{3}{2}\sigma$までの線分を作る．これによって$k+1$本の線分が得られた．

有限個の点の集合$S\subset\boldsymbol{R}^d$について，平均$\boldsymbol{m}$は２乗距離を最小にする：
$$\begin{aligned}
  \boldsymbol{m}=\text{arg}\min_{\mu\in\boldsymbol{R}^d}\sum_{\boldsymbol{x}\in S}\|\boldsymbol{x}-\mu\|.
\end{aligned}$$
よって，任意の$i$に対し
$$\begin{aligned}
  \sum_{\boldsymbol{x}\in S}\|\boldsymbol{x}-\boldsymbol{m}\|^2\leq\sum_{\boldsymbol{x}\in S}\|\boldsymbol{x}-\boldsymbol{x}_i\|^2.
\end{aligned}$$
新しい線分$s$は$\boldsymbol{m}$を含むので，$S$内の点から線分への２乗距離和は，$S$内の点から$\boldsymbol{m}$への２乗距離和以下である：
$$\begin{aligned}
  \sum_{\boldsymbol{x}\in S}d(\boldsymbol{x},s)^2\leq\sum_{\boldsymbol{x}\in S}\|\boldsymbol{x}-\boldsymbol{m}\|^2.
\end{aligned}$$
よって，線分の挿入による$\eqref{kmeans_square_sum}$の減少には下限が存在することが分かる（$V_{k+1}$を決めた時点で，$\eqref{kmeans_square_sum}$の減少は確約されており，以上の式で$S$を$V_{k+1}$とすれば，そこから更に減少することが言える）．

### 新しい線分の場所の効率的な探し方
あらかじめ$n$個のデータ間の２乗距離を表す$n\times n$行列$D$を考える：
$$\begin{aligned}
  D_{ij}=\|\boldsymbol{x}_i-\boldsymbol{x}_j\|^2.
\end{aligned}$$
．各点$\boldsymbol{x}_i$に対し，最も近い線分までの２乗距離$d_i^\text{VR}$を求めておく．更に
$$\begin{aligned}
  \boldsymbol{D}^\text{VR}=
  \begin{pmatrix}
    d_1^\text{VR}\\
    d_2^\text{VR}\\
    \vdots\\
    d_n^\text{VR}
  \end{pmatrix},\quad \mathit{VD}=[\boldsymbol{D}^\text{VR},\ldots,\boldsymbol{D}^\text{VR}]
\end{aligned}$$
とおく．ここで，
$$\begin{aligned}
  G_{ij}=\max(\mathit{VD}_{ij}-D_{ij},0)
\end{aligned}$$
とする．この時$G_{ij}$は，$0$長線分を$\boldsymbol{x}_j$に挿入した時の$\boldsymbol{x}_i$に関する２乗距離の減少に等しい．よって$\eqref{kmeans_square_sum}$の減少は$\sum_iG_{ij}$に等しいので，
$$\begin{aligned}
  \boldsymbol{I}=(1,\ldots,1)
\end{aligned}$$
を考えれば，$0$長線分を$\boldsymbol{x}_j$に挿入した時の$\eqref{kmeans_square_sum}$の減少は$\boldsymbol{I}G$の第$i$成分に等しい．

### 線分の結合
グラフ$G=(V,E)$を考える．ただし，$V$は$k$本の線分の$2k$個の端点である．更に，$k$本の線分と対応する辺全てを含むような$A\subset E$を考える．
\begin{dfn}
  全ての頂点を１度ずつつ通過する経路をハミルトニアン経路(HP)と呼ぶ．
\end{dfn}
HPは辺の集合$P\subset E$と考えることができる．線分を結合して多角形のPrincipal Curveを作るために，コストを最小にするようなHP $A\subset P\subset E$（線分に対応する辺を全て含み，かつ全頂点を一度だけ通る経路）を求めたい．経路$P$のコストは
$$\begin{aligned}
  l(P)+\lambda a(P)
\end{aligned}$$
とする．$l(P)$は経路の長さの総和である．辺$e=(v_i,v_j)$の長さは
$$\begin{aligned}
  l(e)=\|v_i-v_j\|
\end{aligned}$$
とする．$a(P)$は隣接する角度の和である．$\lambda$を調節することによって，辺の向きが変わることに対するペナルティの重み付けを調節する．

HPを作るためgreedy algorithmで考える．sub-HP $P_i$と$P_j$をそれぞれの頂点$v_i$，$v_j$で結ぶとする．$e=(v_i,v_j)$として，新しくできるsub-HPのコストは元のsub-HPのコストの和と$l(e)+\lambda a(e)$の合計になる．全ての辺$e\in(E\backslash A)$に対し，コスト$c(e)=l(e)+\lambda a(e)$を計算しておく．アルゴリズムは以下のようになる：

1. k個のsub-HP $A$から始める．
2. ２個以上のsub-HPがある限り続ける．
3. $i\neq j$として，２個のsub-HP $P_i$と$P_j$を$c(e)$が最小となるような辺$e\in(E-A)$で結ぶ．
これによって，折れ線が得られる．

### 目的関数

最適な$k$を求めるため，データの対数尤度を最大とする折れ線を考える．この折れ線の長さを$l$とし，$\boldsymbol{f}:[0,l]\to\boldsymbol{R}^d$で表されるとする．簡単のために，$t\in[0,l]$に対し，$p(t)=\frac{1}{l}$，$p(\boldsymbol{x}\mid t)$は正規分布とする．よって，各データ点の対数尤度への寄与は負号をかけて
$$\begin{aligned}
  \begin{split}
    &-\log p(\boldsymbol{x})\\
    &= -\log\int_{t\in[0,l]}p(\boldsymbol{x}\mid t)p(t)\,dt\\
    &= \log l+c_1-\log\int_{t\in[0,l]}\exp\left(-\frac{\|\boldsymbol{x}-\boldsymbol{f}(t)\|^2}{2\sigma^2}\right)\,dt
  \end{split}\label{k-segment_log_likelihood}
\end{aligned}$$
となる．ここで，図$\ref{k-segment_distance_approximation}$のように距離を取ると，
$$\begin{aligned}
  \begin{split}
    \|\boldsymbol{x}-\boldsymbol{f}(t)\|^2 &= d_\perp(\boldsymbol{f}(t),\boldsymbol{x})+d_\parallel(\boldsymbol{f}(t),\boldsymbol{x})\\
    &= d_\perp(\boldsymbol{s},\boldsymbol{x})+d_\parallel(\boldsymbol{f}(t),\boldsymbol{x}).
  \end{split}
\end{aligned}$$

\begin{figure}[ht]
  \centering
  \includegraphics[height=3cm]{PCuA_fig5.pdf}
  \caption{距離の近似}
  \label{k-segment_distance_approximation}
\end{figure}
よって，$\eqref{k-segment_log_likelihood}$の最後の項は次のように書ける：
$$\begin{aligned}
  \frac{d_\perp(\boldsymbol{s},\boldsymbol{x})^2}{2\sigma^2}-\log\int_{t\in[0,l]}\exp\left(-\frac{d_\parallel(\boldsymbol{f}(t),\boldsymbol{x})^2}{2\sigma^2}\right)\,dt.
\end{aligned}$$
$d_\parallel(\boldsymbol{s},\boldsymbol{x})=\inf_{t\in[0,l]}d_\parallel(\boldsymbol{f}(t),\boldsymbol{x})$とする．$\eqref{k-segment_log_likelihood}$は次のように近似できる：
$$\begin{aligned}
  \begin{split}
    &\log l+c_1+\frac{d_\perp(\boldsymbol{s},\boldsymbol{x})^2}{2\sigma^2}-\log\int\exp\left(-\frac{d_\parallel(\boldsymbol{f}(t),\boldsymbol{x})^2}{2\sigma^2}\right)\,dt\\
    &\sim\log l+c_1+\frac{d_\perp(\boldsymbol{s},\boldsymbol{x})^2}{2\sigma^2}-\log\int\exp\left(-\frac{d_\parallel(\boldsymbol{s},\boldsymbol{x})^2}{2\sigma^2}\right)\,dt\\
    &= \log l + \frac{d(\boldsymbol{s},\boldsymbol{x})^2}{2\sigma^2} + c.
  \end{split}
\end{aligned}$$
よって，全点についての和を取れば，対数尤度に負号をかけたものは
$$\begin{aligned}
  n\log l+\sum_{i=1}^k\sum_{\boldsymbol{x}\in V_i}\frac{d(\boldsymbol{s}_i,\boldsymbol{x})^2}{2\sigma^2}\label{k-segment_log_likelihood_sum}
\end{aligned}$$
となる．$\eqref{k-segment_log_likelihood_sum}$が初めて極小となる$k$が最適な$k$となる．
