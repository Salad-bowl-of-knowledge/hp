---
toc: true
layout: post
description:
author: 山拓
comments: true
categories: [neuroscience]
title: 人工神経回路による脳の理解はどこまで進んだか
---

[神経科学 Advent Calendar 2019](https://adventar.org/calendars/4132)の2記事目です。人工神経回路 (Artificial neural network, **ANN**) を用いた研究により、脳の理解はどこまで進んだか、次に何が調べられるべきなのかということについて解説します。

 昨年の年末からhttps://github.com/takyamamoto/BNN-ANN-papersにANNと脳に関する論文リストを作成しており (これは先に研究が出てしまう悲劇が頻発したための措置ですが)、このリストがそのまま参考文献となっています。

 本記事は特に([B.A. Richards, T.P. Lillicrap, et al. *Nat. Neurosci.* 2019](https://www.nature.com/articles/s41593-019-0520-2))での議論を参考にしています(翻訳ではないです)。 この論文はANNと脳についての研究を先導してきた多くの研究者が共著者となっています（一体どうやって纏めたのやら）。論文にアクセス権の無い場合は、Kording先生が[twitter](https://twitter.com/KordingLab/status/1189158149476896769)にpdfの短縮リンクを貼ってくださっているのでそこから読むことができます。

## 脳とANNを3つの観点で捉える
([B.A. Richards, T.P. Lillicrap, et al. *Nat. Neurosci.* 2019](https://www.nature.com/articles/s41593-019-0520-2))で言っていることは多くありますが、ざっくり言えば、『脳もANNと同じように**目的関数(objective functions), 学習則(learning rules), 構造(architectures)**の3つの観点で捉えよう』という主張です ([T.P. Lillicrap, K.P. Kording. *arXiv*. 2019](https://arxiv.org/abs/1907.06374)でも、この前段階となる主張をしています)。ちなみにこれはDeep learningに限った話ではなく論文のタイトルを機械学習と置き換えても良いのですが、Reviewerが"Deep learning"を入れるように言ったため、タイトルに入ったそうです(ソースはtwitterですがリンクを忘却しました)。

かなり思い切った主張ですが、この考えが生まれるまでの動向を(不正確かもしれませんが)、本記事で解説します。

## 脳はある目的(関数)に対して最適化されている
近年の研究により(古いものは1980年代のものも含みますが)、ある目的関数に対して最適化されたANNが脳と同様の、あるいは類似した神経活動を得ることがあることが分かってきました（ただし全てを説明できるわけではありません）。これは神経活動を生み出すための機構を考えるという従来の研究と異なり、目的関数さえ仮定してしまえば（もちろん仮定するのはこれだけではないですが）自然に出現する、という簡素さと美しさがあります。

全部説明するのは不可能なので、一部の論文を広く浅く紹介します。

### 視覚系
視覚系は元々の研究が多いのと、ANNが畳み込みニューラルネットワーク(CNN)で成功をしたために論文の数が最も多いです。

まず、先駆的な研究を紹介します。([D. Zipser, R.A. Andersen. *Nature.* 1988](https://www.nature.com/articles/331679a0))は網膜座標から頭部中心座標への変換をするように誤差逆伝搬法で最適化した3層NNの中間層に、7a野のニューロンの活動に類似したゲインフィールドが得られるという内容です。

([A. Krizhevsky, I. Sutskever, G. Hinton. *NIPS*. 2012](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf))はご存知の通り、AlexNetの論文です。CNNにImageNetデータセットの画像分類タスクを学習させると、CNNの畳み込みカーネルに一次視覚野(V1野)の単純細胞の受容野に似たパターンが現れた、というのが関連する結果です。なお、V1野の単純細胞の受容野がガボール関数で近似できることについては([JP. Jones & LA. Palmer. *Neurophysiol.* 1987](http://www.neuro-it.net/pdf_dateien/summer_2004/Jones 1987.pdf))を参照してください。

([D. Yamins, et al. *PNAS*. 2014](https://www.pnas.org/content/111/23/8619); [S. Khaligh-Razavi & N. Kriegeskorte. *PLoS Comput. Biol.* 2014](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003915))は物体認識において、V4野やIT野などの高次視覚野においてもANNと神経活動に相関があったという論文です。これらの論文が出てからANNを用いた脳の研究に関する論文の数が急速に増えたという感じがします。

### 聴覚系
([U. Güçlü, et al., *NIPS.* 2016](https://arxiv.org/abs/1606.02627); [A.J.E. Kell, et al., Neuron. 2018](https://www.sciencedirect.com/science/article/pii/S0896627318302502?via%3Dihub))は音楽やヒトの声を分類するように学習させたANNが、階層的な情報処理をしており、ヒトfMRIのボクセル応答も予測することを示しています。

([T. Koumura, H. Terashima, S. Furukawa. *J. Neurosci*. 2019](https://www.jneurosci.org/content/39/28/5517))では、自然音を分類するように学習させたANNは、聴覚経路全体と最適変調周波数や上限周波数の分布が類似することを示しています(書いてて合ってるか不安になったので[NTTのプレスリリース](https://www.ntt.co.jp/news2019/1907/190710a.html)を読んでください)。

### 運動野
一報だけ紹介。([D. Sussillo, et al., *Nat. Neurosci.* 2015](https://www.ncbi.nlm.nih.gov/pubmed/26075643))はRNN(recurrent neural network)にサルの到達運動時の筋電活動(electromyographic; EMG)を出力するよう学習させると、RNNのユニットは運動野のニューロンのダイナミクスに類似した発火パターンを生み出したという論文です。出力が筋電活動なのに中間にニューロンの活動が現れたということや、正則化を付けないと現れなかったというのも面白い点です。

### 場所受容野
([C. Cueva, X. Wei. *ICLR*. 2018](https://arxiv.org/abs/1803.07770); [A. Banino, et al. *Nature*. 2018](https://deepmind.com/documents/201/Vector-based Navigation using Grid-like Representations in Artificial Agents.pdf))は自己運動の速度を積分して自己位置を推定させるタスクをRNNに学習させると、中間層にグリッド細胞のような発火パターンが生まれたという論文です。

どちらも主張はほぼ同一ですが、([C. Cueva, X. Wei. *ICLR*. 2018](https://arxiv.org/abs/1803.07770))ではグリッド細胞の発達過程が実際の嗅内皮質のニューロンと類似することを示しています。

一方で([A. Banino, et al. *Nature*. 2018](https://deepmind.com/documents/201/Vector-based Navigation using Grid-like Representations in Artificial Agents.pdf))はCuevaらの研究より発表は遅かったのですが、Baninoらの研究ではグリッド細胞のパターンが壁面から少しずれていることまで再現されていました。グリッド細胞の発火の方向が壁面から少しずれていることは謎でしたが、この実験からこれが最適であるということが分かりました。つまり、壁面からずれていることで格子パターンが部屋全体を覆うことができ、一様な場所受容野の形成に役立っているといえます。また、このネットワークを持った強化学習エージェントが迷路課題において高い性能を出したことも注目されました。

いずれの研究でも正則化(Cuevaらは発火抑制損失、BaninoらはDropout)を入れなければグリッド様の発火が生まれなかったことを述べています。

### その他
ANNを用いた研究対象となる脳の機能は様々です。特にヒトに固有の高次機能については、fMRIなどでは脳の機能部位しか分からなかったため、ANNは打開策となるかもと期待されています。関連する総説としては([N. Kriegeskorte & P. Douglas. *Nat. Neurosci.* 2018](https://arxiv.org/abs/1807.11819))などを参照してください。

## 脳における目的関数とは何か
前節のような結果が多数報告されたため、([B.A. Richards, T.P. Lillicrap, et al. *Nat. Neurosci.* 2019](https://www.nature.com/articles/s41593-019-0520-2))では「神経細胞の個々の活動の意味をボトムアップ的に考えるのではなく目的関数の最適化の結果として捉えよう」といったことが言われています。それでは脳における目的関数とは何でしょうか。

ANNでは定義しなければならない目的関数ですが、脳内では明確に存在はしていません。そもそも目的**関数**という言い方ですが、これは生体内のパラメータなどが不明であっても数値的に表現はできると思うので関数であることに違いはないと思います。ただし、ANNの目的関数のように微分可能かということは保証されていません。

一度、脳から離れて**生物および生体の目的関数**を考えてみましょう。生物の目的は主に種の生存と個体の生存の2つがあるでしょう(この他にもあると思いますが)。前者は子孫の数、後者は寿命の長さが目的関数であると言えます。これらの目的関数を最大化するために生物は主に、4つのF (**Four Fs**)であるfeeding, fighting, fleeing, および fornicating (またはmating)を行います。

生体内で考えると、体温や血中酸素濃度や血糖値などなど恒常性(homeostasis)の維持が至る所で行われています。この場合の目的関数は定常状態からの変位と言えます(この場合は最小化します)。

脳に戻れば、例えば物体認識では物体に対する神経表現の距離を最大化している、要は**距離学習**(metric learning)していると言えそう…だったり言えそうじゃなかったりします。ANNでは教師ラベルを仮定してCross entropy誤差を目的関数と置くのが基本(非生物的)ですが、最近は教師なしの距離学習も盛んです。あまりサーベイできていませんが、最近だと([K. He, et al., *arXiv*, 2019](https://arxiv.org/abs/1911.05722))があります。少し違うのですが、1つの物を手に取って色々な角度から見れば、1つの物体としてラベル付けされた複数の画像が得られます。これを生かした距離学習もしているのでは、と自分の中では思っています。もう少しはっきりした例を挙げれば強化学習における**報酬**(reward)の最大化があるでしょう。これは結構明示的で、報酬という大域的目的関数のためのサブ目的関数が複数存在するように思います。また**好奇心**(curiosity)や**驚き**(surprise)による学習も存在します。これに関して、報酬、好奇心、驚きに基づいた学習を指す言葉として、それぞれ**Reward-based learning, curiosity-driven learning, Surprise-based learning**などがあります。

解剖学・発生学的な観点で言えば回路の配線最適化問題も挙げられるでしょう。できるだけ配線長が短くなるように効率的なネットワークを構築することが目的となります。また、頭蓋骨という体積の制約の中で、可能な限り回路を大きくするという形態学的な面においても目的関数が存在するでしょう。

少し計算論の方に話を逸らせば、**予測符号化**(predictive coding, [R.P. N. Rao & D.H. Ballard*. Nat. Neurosci.* 1999](https://www.nature.com/articles/nn0199_79))では予測誤差の最小化、これをBayesの枠組みで拡張した(知覚においてBayes的考えを予測符号化ネットワークに乗っけたり行動も付けたりした)**自由エネルギー原理**(free energy principle, [K. Friston, et al., *Journal of Physiology.* 2006](https://www.fil.ion.ucl.ac.uk/~karl/A free energy principle for the brain.pdf))では自由エネルギーの最小化(あるいは変分下限(variational lower bound)の最大化、記述長(description length)の最小化)などを仮定しています。他のANNに関与しない研究もこうした目的関数を定義していることは多いです。

脳は学習方法を学習できたりと様々なことができます。実際には目的関数が複数並列・分散しているのでしょう。では目的関数がどこに表現されていて、外部からどのように知ることができるか、という疑問が当然でます。前者は恐らくゲノム(適応的に目的関数を生み出している感じもありますが、大概は報酬の最大化だったり)、後者は経験的に知るしかないのでは、と思います(曖昧過ぎますが)。

## 脳では如何なる最適化手法が用いられているか
脳における学習は**Hebb則**や**STDP(spike-timing dependent plasticity)則**などが挙げられますが、これらは完全ではありません。上で議論されていた最適化は**誤差逆伝搬法(back-prop)**によるものです。この節では**貢献度分配問題(credit assignment problem)**を中心として脳における最適化について考えます。

### 貢献度分配問題 (credit assignment problem)
**貢献度分配問題(credit assignment problem)**はある出力を得るためにどのニューロンまたはシナプスに功罪(credit/blame)、要は貢献度を割り当てるかという問題です。ある出力について貢献度の高いニューロンの出力(発火率)は大きくなりますし、逆は小さくなります。

### 誤差逆伝搬法の近似
誤差逆伝搬法が生体内で用いられていないという批判は数多く、DNAの二重螺旋構造を発見したCrickも記事を書いています([F. Crick. *Nature*. 1989](https://www.nature.com/articles/337129a0))。

色々と問題点はあるのですが、逆伝搬の結合重みが順伝搬の結合重みの転置行列になっているという問題(**weight transport problem**)は、例えば逆伝搬時の重みをランダム行列にすることでも上手くいく([T. Lillicrap, et al., *Nat. Commun.* 2016](https://www.nature.com/articles/ncomms13276))、という研究により緩和されています。この手法を**Feedback alignment**と言います。誤差逆伝搬法を緩和しようという試みは多数ありますが(他には**Equilibrium Propagation**([B. Scellier & Y. Bengio. *Front. Comput. Neurosci.* 2017](https://arxiv.org/abs/1602.05179))など)、とりあえず総説(J. [Whittington & R. Bogacz. *Trends. Cogn. Sci.* 2019](https://www.sciencedirect.com/science/article/pii/S1364661319300129?via%3Dihub); [T.P. Lillicrap & A.Santoro. *Curr. Opin. Neurobiol.* 2019](https://www.sciencedirect.com/science/article/pii/S0959438818302009))を参照してください。

誤差逆伝搬法は現状ANNを訓練するのに最適ですが、それは層を縦断して貢献度分配問題を解決できるためです。以前は誤差逆伝搬法は全く生理学的ではないと切り捨てられてきましたが、緩和すれば全く不可能ではないかもしれません。また誤差逆伝搬法は無理でも、勾配法（に類する最適化）は使ってるんじゃないか、という意見もありますが決着はついていません(参考：[Kording先生のtweetに対するリプライ](https://treeverse.app/view/1hX6Xm0w))。また、誤差逆伝搬法に関係なく貢献度分配問題を解決する研究も出始めています([J. Aljadeff, et al., *arXiv.* 2019](https://arxiv.org/abs/1911.00307))。

よく**生物学的妥当性(biological plausibility)**や**生理学的妥当性(physiological plausibility)**という語を用いますが、ここでの生物学や生理学には「人類が既知の」という枕詞が付きます。生体内では発見されるまで人類が予想していなかった現象も多いので、否定はしきれないでしょう（かといって自分が完全肯定しているということでもないですが）。

### 進化による獲得
ANNが脳と違う点として学習データを大量に必要とする、という指摘があります。確かに脳は少ないサンプルで物事を見分ける**Few shot learning**が可能です。しかし、脳の最適化は生まれてから起こっているのではなく、進化の過程においても最適化が起こっています。

([A. Zador. *Nat. Commun*. 2019](https://www.nature.com/articles/s41467-019-11786-6))ではヒトを含めた動物の行動は未知の優れた教師あり/教師なし学習によるのではなく、大部分がゲノムに符号化された神経回路によると主張しています。同時にゲノムにはあらゆることが記述できるわけではない(**genomic bottleneck**)ので、優れた汎用的な回路(いわゆる **canonical microcircuit**)が生まれたとしています。

物体認識で考えれば、幼児であっても顔に対してはよく反応します。また、飼育員が顔を隠して育てたサルであってもヒトとサルの顔の識別はできます([Y. Sugita, *PNAS*. 2008](https://www.pnas.org/content/105/1/394))。ただしこれについては経験がやっぱり大事だという主張もあります([M. J. Arcaro, et al., *Nat. Neurosci*. 2017](https://www.nature.com/articles/nn.4635))。関連して、謎ですが、ANNにおいて顔認識ニューロンが未訓練でも自発的に生まれるという報告もあります ([S. Baek, et al., *bioRxiv.* 2019](https://www.biorxiv.org/content/10.1101/857466v1))。

色々と議論はありますが、最適化において進化が占める割合はとても大きいです。しかし、ヒトに戻って考えてみると環境は常に変化しているにも関わらず何とか適応できています。このことを考えれば貢献度分配問題は進化による最適化だけでは説明できるとは言えなさそうです。

## *in silico*から*in vivo*へ
結局、ANNを用いた*in silicoの*研究で分かったことは『**脳は目的関数を最適化している**』ということです(個々の論文を読めばそれだけではないですが)。この結果を*in vivoの研究*に持っていくわけですが、解決すべき問題は『**脳における貢献度分配問題を解決するような学習則は何か**』ということです。

かなり難しいと思いますが、研究が進めばいずれ解決できる問題だと自分は思っています。

