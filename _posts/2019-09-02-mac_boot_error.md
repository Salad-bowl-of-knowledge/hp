---
toc: true
layout: post
description:
author: 優曇華院
comments: true
categories: [macOS]
title: macOS Xが起動しない（FFFFFFFF-FFFF-FFFF-FFFF-FFFFFFFFFFFF）
---

## linux入れてたパーティション消したら，macのパーティションが壊れた
表題の通りなのですが，優曇華院はmacにmac OS Xとlinuxをデュアルブートしていました．SSD (APPLE SSD SM0128G Media)の中にMacintosh HD（mac OSのための領域，約100GB）とLinux HD（Linuxのための領域，約20GB）のパーティションを作成していました．ある日，「もうLinuxいらねえな」となって，ディスクユーティリティからLinux HDを削除したら，なんか知りませんがエラーが出ました（MS-DOS (FAT)にしてたせい？）．しかも，Macintosh HDは100GBのままです．

パーティションでミスったらヤバい，みたいな話を聞いたことがあるので，嫌な予感はしました．再起動をかけたら案の定macがお陀仏になりました．

## 状況

- 起動すると，GNU GRUBが起動する
- Macのstartup manager（起動時にoption）を使っても，EFI bootしか表示されない（今まではMacintosh HDが表示されていた）
- リカバリーモード（起動時にcmd+R）は，変な地球儀が出た後に，食べかけのリンゴが出て，起動する
- セーフモード（起動時にshift）は使えない．GNU GRUBが起動する．
- リカバリーモードでディスクユーティリティ使うと，「内蔵」の項目には変なのが表示される（普通ならMacintosh HD），特に何も操作できない．

## 解決策

リカバリーモードのまま進めます．まずは，ターミナルを開いて，

```sh
gpt -r show disk0
```

でdisk0の中身を表示します．結果はこんな感じでした：

```text
       start        size  index  contents
       （略）
      409640  197530544      2  GPT part - FFFFFFFF-FFFF-FFFF-FFFF-FFFFFFFFFFFF
```

（もしdisk0がないとか言われたら，こんなのが出るまでdisk1, disk2,...って頑張ってください．）上で表示されている，FFFFFFFF-FFFF-FFFF-FFFF-FFFFFFFFFFFFのstart, size, indexの値をメモってください．優曇華院の場合はそれぞれ，409640, 197530544, 2です．

次に，一旦ディスクのマウントを解除します：

```sh
diskutil umountDisk disk0
```

次に，FFF...を一旦消します：

```sh
gpt remove -i [index] /dev/disk0
```

同じパーティションを正しい名前で追加します：

```sh
gpt add -i [index] -b [start] -s [size] -t [GUID] /dev/disk0
```

GUIDはAPFSコンテナの`7C3457EF-0000-11AA-AA11-00306543ECAC`を使います．

これで終わり．あとはリブートしてstartup managerからMacintosh HDを選べばok普通に起動できます．

## まとめ
- Time machineとかでバックアップ取っていれば，FFF...を消去してから，もっかいMacintosh HDを切って，OSをインストールすればいい
- OSの再インストールをしようとしたが，そもそもディスクを認識していないのでできるはずもなかった
- なんで名前変わったかは分からない
- 消滅したLinuxパーティションの在処は現在探しています
- Linuxが消えたのかは不明
- **デュアルブートしたら戻すのが大変だぞ！仮想マシンにしとけ！**

## 追記
- Linux HDは消えて，物理ボリューム（SSD）の中身はMacintosh HDと「空き領域」になっていました
- 空き領域を削除して，Macintosh HDを128GBに戻せました
- Linuxは消滅しました