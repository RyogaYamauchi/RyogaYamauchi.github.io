---
title: "HugoとGithub.ioで静的なブログを作ろう！"
date: 2021-10-06T23:55:33+09:00
draft: false
---

# はじめに

HugoとGithub.ioを使って、無料で静的なブログの作り方の話です。

Hugoとは、go言語で作られたstatic site generatorです。（静的なサイトを作ってくれる便利な奴です）  
デプロイはGithub.io(Github pages)を用いて無料で完結します。

## Hugo
{{< figure src="/images/hugo.png" class="center" width="200"  >}}

Hugoをインストールします。

```brew install hugo```

これでhugoを使えるようになったので、プロジェクトの作成をします

```hugo new site XXX``` //XXXはプロジェクトの名前

色々とファイルが作成されます。

### Hugo テーマ（theme）

デザインは無料で配布されています。  
好きなパターンを選んで、そのサイトに書いてある通りに実践していくとデザインが適用されます。

https://themes.gohugo.io/

（僕はGithub styleを使わせていただきました。このサイトもこれで作らています。）  
https://themes.gohugo.io/themes/github-style/

適用してみます。

```cd XXX```  
```git init```  
```git submodule add git@github.com:MeiK2333/github-style.git```

(sshの設定をしていない人はトークンを発行してhttpでアクセスしても大丈夫です)

### Github.ioのリポジトリを作成する

Githubにアクセスして、新しくリポジトリを作成します。  
privateでも大丈夫だと思います。

名前は```YYY.github.io``` // YYYは任意の名前


{{< figure src="/images/githubio1.PNG" class="center" width="600"  >}}

Settings -> Pagesを開いて Branch: master, \docsに設定します。

{{< figure src="/images/githubio2.PNG" class="center" width="600"  >}}

これでGithub.ioの設定は完了です

config.tomlというプロジェクト全体に適用される設定ファイルのようなものを作成します

```vi config.toml```

```
base URL = "https://YYY.github.io/   (YYYはgithub.ioで設定したもの)
languageCode = "ja"  
title = "俺のブログだ！！（ここは任意）"  
theme = "github-style"
publishDir = "docs"  
```

設定できましたえらい！

### 記事を作成する

```hugo new post/self_introduction.md```

これでprojectにself_introduction.mdが作成されます。

```
---  
title: "HugoとGithub.ioで静的なブログを作ろう！"  
date: 2021-10-06T23:55:33+09:00  
draft: false  
---
```

draft: trueにしないと公開されないので注意してね

---の下に書きたいことをマークダウンで連ねていきます！

ある程度かけたら、ローカルサーバを起動します。

```hugo server```

簡単すぎん...?

これでlocalhost:1313がアクセスできるようになるので、お使いのブラウザで開いてみてください。

{{< figure src="/images/githubio3.PNG" class="center" width="600"  >}}

ホームページとPostsにタイトルに設定したものが追加されていると思います。

サーバを起動している間、ファイルを編集して保存するたびに更新されるのでレイアウトを確認しながら書けるのはうれしいですね。

### 静的サイトを構築する

　```docs/```に書いたマークダウンファイルをhtml形式に変換してデプロイできるようにします

先ほどconfig.tomlにpublishDir = "docs"を追加したので、特に変更点はありません。

``` hugo -b https://YYY.github.io/``` (YYYはGithubで設定したリポジトリの名前)

これでdocs/に書いたものが展開されます。

### デプロイする

```
git add .  
git commit -m "サイト構築"  
git push origin master
```

普通にmasterにpushするだけでデプロイが完了します。超簡単！

構築には数分くらいかかる場合があるので、少し待ってURLにアクセスしてみてください。

```https://YYY.github.io/``` (YYYはGithubで設定したリポジトリの名前)

{{< figure src="/images/githubio3.PNG" class="center" width="600"  >}}

公開されました！えらい！！

hugoは他にもカスタムでcssを組めたり、twitterの表示やgoogle analyticsの設定、色々なことができるので是非試してみてください！！

ここまで読んでいただきありがとうございました！

## 参考
https://www.membersedge.co.jp/blog/create-hugo-theme-and-deploy-to-github-pages/