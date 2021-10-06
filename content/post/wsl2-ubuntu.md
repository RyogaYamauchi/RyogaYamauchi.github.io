---
title: "WSL2でWindowsの上にLinux環境を構築する"
date: 2021-10-06T19:52:46+09:00
draft: false
---

## はじめに

みなさんは何のOSを使っているでしょうか。Windwos? MacOS? Ubuntu? CentOS? それ以外?

僕は業務でも私生活でも**MacOS**を使用しています。  
理由は大学と職場が指定しているからです。

しかし、VRゲームやWindows専用ゲームがしたいときWindows機器を持っていないと何かと不便です。  
パッケージ管理系もlinuxで慣れているのでそちらを使いたい気持ちもあります。  
今回は、新しく**8万円**で買ったVR ReadyなWindowsにLinuxディストリビューションのUbuntuを乗せた話を書こうと思います。

## 購入したPC

大学生の財力なんてたかが知れています。  
買ったのはメルカリで使用歴3年程度の型落ち中古PC　その名もガレリアXVです！ﾊﾟｰﾝ!!!

{{< figure src="/images/gareria.jpg" class="center" width="600"  >}}
引用元 : https://game-pc-bto.com/review/galleria-xv/

このスペックで送料込み8万円、安いですね~

メルカリを常時監視してたら割と転がってたりします。自分でお掃除ができる人はおすすめです。  
メモリは16GBで増設済みです。特殊なことせず家で使う分には十分すぎる！

## Linux乗せる

乗せたOSは前述にもありますが、Ubuntu(20.04)です。  
理由は前に使ったことがあったためです、次はCentOS使ったことないから使ってみたいな、、

やり方はいたって簡単!!
Windowsさんが用意してくれています。

PowerShellで以下のコマンドを実行して、WSLを有効にします。  
うまくいかない場合は参考の(1)を見てみてください

```dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart```  
```dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart```  
```wsl --set-default-version 2```  

再起動

MicroSoft Storeを開いてUbuntu(20.04 LTS)を指定してインストールしてください
{{< figure src="/images/store.PNG" class="center" width="600"  >}}


次はユーザの作成です。  
PowerShellで以下のコマンドを実行します。   

最初にユーザ作成をする画面が表示されますが、そのアカウントはsuper userになるのでユーザ名、パスワードを設定して覚えておきます。

新しく、管理者権限を持たないユーザを作成します。

更新  
```sudo apt update```

アップグレード  
```sudo apt upgrade -y```

ユーザの追加  
```sudo adduser XXX``` // XXXは任意の名前

ユーザの追加が完了したら、PowerShellで以下を実行します。  

```ubuntu20.04 config --default-user XXX``` // XXXは先ほど指定したユーザネーム

これでwslにログインする際のデフォルトユーザが設定できました。

**WindowsでLinuxディストリビューションを使えるようになった!!**


## 備考

ここからはやってもいいし、やらなくてもいいことのTIPSです。

### ホームディレクトリの指定

次にホームディレクトリの指定をします。  
デフォルトでは、```/home/XXX```になっていて扱いづらいです。(僕だけかも)  

なので、ホームディレクトリをwindowsと同じような位置に設定してあげます。

```sudo vim /etc/passwd```  
```XXX:1000:1000:YYY:/home/XXX:/bin/bash```  
以下に変更↓  
```XXX:1000:1000:YYY:/mnt/c/Users/ZZZ:/bin/bash```

YYY, ZZZはそれぞれあなたのWindowsアカウントの名前になっていると思います。

ubuntuを再起動したらホームディレクトリが変更されているはずです。

### GitHub ssh設定

ホームディレクトリを変更したことにより、ubuntu上でのssh設定がややこしくなります。

ubuntu上でホームディレクトリを変更しましたが、gitさんがsshで使おうとするディレクトリの位置と違う場所になるので、シンボリックリンクでリンクしてあげます。

ubuntuのホームディレクトリに移動  
``` cd /home/XXX```  

ssh-keyの作成  
```ssh-keygen -t rsa -b 4096 -C "email.example.com"``` // emailはあなたのメールアドレスを設定してください  

ssh-keygenコマンドで作成した```id_rsa, id_rsa.pub```が存在するか確認してください  
```ls -la /home/XXX/.ssh```

シンボリックリンクを作成します  
```ln -s /home/XXX/.ssh ~/```

これでgitさんがsshでアクセスできる設定ができました！

GitHubとの連携は調べたら出てくるので書きませんが、お忘れなく、、(https://qiita.com/shizuma/items/2b2f873a0034839e47ce)

## さいごに

Windows環境にubuntuを乗せることで、linux系のソフトウェアが使えるようになります。  
ヤッタネ！！  
読んでくれてありがとう！

## 参考
(1) WSLインストール https://qiita.com/whim0321/items/ed76b490daaec152dc69  
(2) WSLインストール https://docs.microsoft.com/ja-jp/windows/wsl/install  
(3) WSLのホームディレクトリ位置変更 https://qiita.com/funacchi/items/c3bb78a546cf2605205d  
(4) WSLでgit sshを使う https://qiita.com/satto_sann/items/9d95241af81dc4466ee1  