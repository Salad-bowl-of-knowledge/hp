---
toc: true
layout: post
description:
author: 山拓
comments: true
categories: [neuroscience]
title: 統合情報理論(IIT)の数理と実装
---

この記事は**統合情報理論 (Integrated information theory, IIT)** を少しでも数式で理解してみようというのが趣旨です。前半で数式的な理解、後半で[PyPhi](https://pyphi.readthedocs.io/en/latest/index.html)を使ったシミュレーションを行ってみます。なお、意識研究をしているわけではないのでご了承ください。

書くに至った動機としては
- IITが人気を集め (あとFEPも人気すぎませんか？)、IITという名は聞くし一般向けの本や総説は読むも、数式的な理解が伴っていなかった
- ネット上 (だいたいTwitter)ではふわっとした話しかない。いや、もちろんIITなど意識研究をしている方々もいらっしゃるのは知っているが、ふわっとした話が圧倒的である。意識研究の進捗を聞かされているものの中身を誰も教えてくれはしないのである (論文を読めという話ですが)
- ふわっとした言語的理解は理解していないのと同義であるし、少しでも数式的な理解を深めたい
- なぜ、そんなにIITがもてはやされるのか全く分からない
- 研究をしていて、(研究室①の) ボスに「統合情報量の計算ができると良いですね」と言われた
- 一応、大脳半球間投射に関わる研究をしている (研究室②)

などです (全方面に喧嘩を売っていますね)。まあ、勉強ノートだと思ってください。内容に関しては論文を読むのが一番でしょう。

それで、とりあえずマルコフ連鎖が必要っぽいので過去に書いた記事を挙げておきます。
https://omedstu.jimdofree.com/2018/05/04/%E3%83%9E%E3%83%AB%E3%82%B3%E3%83%95%E9%80%A3%E9%8E%96-markov-chain/

## 参考文献
- [Integrated Information Theory](http://integratedinformationtheory.org/)
- [Integrated information theory - Scholarpedia](http://www.scholarpedia.org/article/Integrated_information_theory)
- [Integrated information theory - Wikipedia](https://en.wikipedia.org/wiki/Integrated_information_theory)
- S. Krohn & D. Ostwald. [Computing integrated information](https://academic.oup.com/nc/article/2017/1/nix017/4060547). *Neuroscience of Consciousness*. 2017.
- Oizumi M, Albantakis L & Tononi G. [From the Phenomenology to the Mechanisms of Consciousness: Integrated Information Theory 3.0](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003588).  *PLoS Computational Biology*. 2014.
- [pooneilの脳科学論文コメント: 大泉匡史さん「意識の統合情報理論」セミナーまとめ](http://pooneil.sakura.ne.jp/archives/permalink/001594.php) ([pdf](http://pooneil.sakura.ne.jp/images/IIT20161115.pdf))
- 苅田裕也. [統合情報理論の可能性](http://kymst.net/index.php?plugin=attach&refer=GrpE%2Fjournal&openfile=ActaEps006.pdf). 2016.
- [Oizumi Lab](https://sites.google.com/a/g.ecc.u-tokyo.ac.jp/oizumi-lab/)の刊行物
- [CSC-UW/iit-pseudocode: Pseudocode for the major algorithms of Integrated Information Theory](https://github.com/CSC-UW/iit-pseudocode)

PyPhi: A toolbox for integrated information theory
https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1006343

https://singularity.jp/20180915-oizumi/
https://singularity.jp/download/%E6%84%8F%E8%AD%98%E3%81%AE%E7%B5%B1%E5%90%88%E6%83%85%E5%A0%B1%E7%90%86%E8%AB%96%E3%81%8B%E3%82%89-%E6%84%8F%E8%AD%98%E3%81%AE%E7%90%86%E8%AB%96%E3%81%AE%E5%89%B5%E3%82%8A%E6%96%B9%E3%82%92%E8%80%83/

http://kymst.net/index.php?plugin=attach&refer=GrpE%2Fjournal&openfile=ActaEps006.pdf

http://bn.dgcr.com/archives/20190111110100.html


https://twitter.com/hayashiyus/status/1021235120638865408?s=20
https://sites.google.com/a/g.ecc.u-tokyo.ac.jp/oizumi-lab/home

https://figshare.com/articles/phi_toolbox_zip/3203326/10

https://bsd.neuroinf.jp/wiki/%E6%84%8F%E8%AD%98

https://science.sciencemag.org/content/366/6463/293



- IIT 3.0 Tutorial | Tsuchiya Lab
https://www.youtube.com/watch?time_continue=2328&v=IflW_oaOJqg

