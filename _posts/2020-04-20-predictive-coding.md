---
toc: true
layout: post
description:
author: 山拓
comments: true
categories: [neuroscience]
title: 予測符号化 (Predictive coding) とは何か
---

## 脳は外部環境を予測する
脳は不良設定問題 (ill-posed problem)を解けると言われます。不良設定問題というのは、問題を解くための条件が不足している問題のことです。例えば、網膜に投影された2D画像から3D構造の認知という逆問題などがあてはまります。では脳はどうやって不良設定問題を解いているかというと、解けるように条件を勝手においているためです。この条件とは、例えば「光は上から照らされている」、「テクスチャが密なほど遠くにある」などです。これは経験によって得た確率分布であるとも言い換えられます。

それでは、どのような学習によりこれらの条件を脳は獲得しているのでしょうか。今現在(2018年)のところは「外部環境を予測する教師なし学習」であると言われています。

視覚について考えてみましょう。まず、網膜の視細胞が光刺激を受け取り、LGN（外側膝状体）→V1（一次視覚野）→V2→… と低次から高次へ情報を伝達します。これは順行性結合(feed-forward connection)です。この過程で外界における隠れ変数の推定が行われます。隠れ変数には物体の大きさ、物体間での前後関係、奥行き、運動、光の当たり方、色など物体に関する多種多様な構成要素が含まれます。

こうして生まれた外界の仮説を用いて、今度は外界に対する予測を脳は生み出します。シミュレーションする、と言ってもいいかもしれません。予測は高次から低次に送られますが、これは逆行性結合(feed-back connection)です。こうして脳は、過去に得られた情報から未来の網膜像を予測し、実際に得られた網膜像と比較します。それらが一致していれば、つまり正しく予測できていれば問題はないのですが、予測できていなければ予測を生み出した仮説が間違っていた、つまり正しくエンコードできていなかったということになります。よって、変数の推定および予測を生み出したネットワークを最適化するため、予測と実際の入力との誤差信号が再度、順行性で送られます。少し曖昧な言葉が多くなりましたが、詳しいことは後で説明します。

## 視覚経路の双方向結合

## Predictive codingのモデル
### 2細胞モデル

このモデルは
> Alpha oscillations and travelling waves: signatures of predictive coding?
https://www.biorxiv.org/content/10.1101/464933v1.full

### (Rao & Ballard, 1999) モデル

### PredNet

## Free energy principleとPredictive coding
自由エネルギー原理(Free energy principle, FEP)

https://www.sciencedirect.com/science/article/pii/S0022249615000759

## 誤差逆伝搬法の近似としてのPredictive coding
Whittington and Bogacz Model
https://arxiv.org/pdf/1910.12151.pdf
https://www.cell.com/trends/cognitive-sciences/fulltext/S1364-6613(19)30012-9


## 脳におけるPredictive coding
> Urbanczik R, Senn W. Learning by the dendritic prediction of somatic spiking. Neuron. 2014;81(3):521–528. doi:10.1016/j.neuron.2013.11.030
https://pubmed.ncbi.nlm.nih.gov/24507189/

> Layer and rhythm specificity for predictive routing
https://www.biorxiv.org/content/10.1101/2020.01.27.921783v1


## 補遺
### Linear predictive coding (LPC)との関係

https://nms.kcl.ac.uk/michael.spratling/Doc/taxonomy_pc.pdf

> Spratling MW. A review of predictive coding algorithms. *Brain Cogn*. 2017;112:92–97. doi:10.1016/j.bandc.2015.11.003

### Kalman filterとの関係
カルマンフィルタやんけ
RNNをカルマンフィルタとしてとらえる

> Friston K. Does predictive coding have a future?. *Nat Neurosci*. 2018;21(8):1019–1021. doi:10.1038/s41593-018-0200-7


## 参考文献

