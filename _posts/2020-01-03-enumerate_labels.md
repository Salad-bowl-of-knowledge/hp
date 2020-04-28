---
toc: true
layout: post
description: 
author: 優曇華院
comments: true
categories: [LaTeX]
title: enumerateのラベルを毎週の日付にする
---

## 概要
久しぶりのLaTeXネタ．~~カッコつけて~~カウンタ類をTeXで作ったら，引き継がれなかったりして面倒だった．やっぱりLaTeXだね．

表題の通りです．LaTeXの`enumerate`のラベルを１週間づつの日付にします．ソースは以下の２つのstyファイル：

- [enumitem_extended.sty](https://github.com/tetsu-osaka-physics/tetsu_physic/blob/master/enumitem_extended.sty)：`enumerate`環境のラベルを増やすやつ
- [Wdate.sty](https://github.com/tetsu-osaka-physics/tetsu_physic/blob/master/Wdate.sty)：日付処理のstyファイル

を適当なフォルダに入れれば動きます．

## 使い方
```latex
\documentclass{jsarticle}
\usepackage{enumitem_extended}
\begin{document}

\intercalarytrue % 2020年は閏年！
% \intercalaryfalse % 2021年は閏年じゃ無い！

\setcounter{Wmonth}{1}
\setcounter{Wday}{8}
\begin{enumerate}[label=\Wdate*]
  \item 進捗を生む
  \item 進捗を生む
  \item 進捗を生む
  \addWdate
  \item 進捗を生む
  \item 進捗を生む
  \item 進捗を生む
  \item 進捗を生む
  \addWdate
  \item 進捗を生む
  \item 進捗を生む
  \item 進捗を生む
  \item 進捗を生む
\end{enumerate}

\setcounter{Wmonth}{6}
\setcounter{Wday}{12}

\begin{enumerate}[label=\Wdate*]
  \item 進捗を生む
  \item 進捗を生む
\end{enumerate}

\end{document}
```

カウンターで開始日付を設定します．再設定しない場合は，前の`enumerate`環境の日付が引き継がれます． `\addWdate` で，週を進める（つまり，２週分進む）ことができます．

もし3日づつとかのラベルを作りたければ，`Wdate.sty` の中の`\addtocounter` の第２引数を変更すればオッケー．