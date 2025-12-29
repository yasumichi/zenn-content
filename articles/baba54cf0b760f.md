---
title: "今年の GitBucket との関わり"
emoji: "👋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['GitBucket', 'Scala']
published: true
---
## 今年の GitBucket との関わりについてまとめますがその前に
### gitbucket-plugins.github.io は wiki に移行し閉鎖予定だそうです

永らく [GitBucket](https://gitbucket.github.io/) のコミュニティプラグインの紹介サイトとして設置されていた [GitBucket Community Plugins](https://gitbucket-plugins.github.io) のサイトは、GitBucket の [Wiki](https://github.com/gitbucket/gitbucket/wiki/) 上の１ページとしてメンテナンスされるようになりました。

今後は、

[Community Plugins · gitbucket/gitbucket Wiki](https://github.com/gitbucket/gitbucket/wiki/Community-Plugins)

を参照するようにしてください。

自作プラグインを載せたいという場合は、直接、Wiki を編集すれば良いようです。([編集履歴](https://github.com/gitbucket/gitbucket/wiki/Community-Plugins/_history)は Git で管理されてます。)

### 関連 issue

- [Close gitbucket-plugins.github.io · Issue #61 · gitbucket-plugins/gitbucket-plugins.github.io](https://github.com/gitbucket-plugins/gitbucket-plugins.github.io/issues/61)
- [Can you redirect or link from https://gitbucket-plugins.github.io/ to https://github.com/gitbucket/gitbucket/wiki/Community-Plugins? · Issue #3901 · gitbucket/gitbucket](https://github.com/gitbucket-plugins/gitbucket-plugins.github.io/issues/61) 

## gitbucket-drawio-plugin を動くように修正

4年近くメンテナンスされていない [gitbucket-drawio-plugin](https://github.com/onukura/gitbucket-drawio-plugin) に手を入れ、動くようにしました。

ついでにインターネットに接続できない環境のためにセルフホストの [draw.io](https://viewer.diagrams.net/) と連携できるように設定を追加しました。

![](https://storage.googleapis.com/zenn-user-upload/c8d000b77e3f-20250726.png)

本家へ[プルリクエスト](https://github.com/onukura/gitbucket-drawio-plugin/pull/1)を発行しましたが、なしのつぶてなので [gitbucket-drawio-plugin (Forked version)](https://github.com/yasumichi/gitbucket-drawio-plugin) として、[Community Plugins · gitbucket/gitbucket Wiki](https://github.com/gitbucket/gitbucket/wiki/Community-Plugins) に追加しました。

### 関連記事

- [gitbucket-drawio-plugin を動くようにしたい](https://zenn.dev/yasumichi/articles/04d09995388959)
- [gitbucket-drawio-plugin からセルフホストの draw.io にアクセスさせたい](https://zenn.dev/yasumichi/articles/87cf2e9ab60aa5)

## GitBucket Markdown Enhanced Plugin の開発

[GitBucket Markdown Enhanced Plugin](https://github.com/yasumichi/gitbucket-markdown-enhanced) は、[Visual Studio Code](https://code.visualstudio.com/) の拡張機能である [Markdown Preview Enhanced](https://shd101wyy.github.io/markdown-preview-enhanced/#/) のようにマークダウンでリッチな表現ができるようにするプラグインです。

まだ、一部の機能しか再現できていませんが、[PlantUML](https://plantuml.com/ja/) や [mermaid](https://mermaid.js.org/) によるダイアグラムや [KaTeX](https://katex.org/) による数式を埋め込むことが可能です。

そのほか、目次の自動生成や脚注の作成、issue へのリンクの挿入などが行えます。

2025/12/29(月)現在は、リポジトリビューアーにのみ対応していますが、GitBucket 4.45.0 がリリースされれば、コミットコメントや issue 、Wiki などでも利用できる見込みです。

- [Add support for an alternative renderer to commit comments, wikis, and issues. by yasumichi · Pull Request #3882 · gitbucket/gitbucket](https://github.com/gitbucket/gitbucket/pull/3882)

### 関連記事

:::details ちょっと多いので畳みます
- [GitBucket の Markdown レンダリングを強化する試み #Scala - Qiita](https://qiita.com/yasumichi/items/4a72493c03d5858a391a)
- [GitBucket Markdown Enhanced Plugin の強化 #Scala - Qiita](https://qiita.com/yasumichi/items/f33924ef28babf969cb0)
- [GitBucket Markdown Enhanced Plugin に PlantUML サポートを組み込む #Scala - Qiita](https://qiita.com/yasumichi/items/8e7655f514497b87b6e2)
- [flexmark-java でリンク生成時に URL をカスタマイズする #Scala - Qiita](https://qiita.com/yasumichi/items/169faf1419be41b82ae2)
- [flexmark-java で WikiLink を有効にし、リンク URL の解決をカスタマイズする #Scala - Qiita](https://qiita.com/yasumichi/items/b3c2b2d0aaa03e2786af)
- [GitBucket Markdown Enhanced Plugin に Mark のレンダリング機能を追加しました](https://zenn.dev/yasumichi/articles/7d0b42a9858912)
- [GitBucket Markdown Enhanced Plugin でプレビュー時に KaTeX が描画されない問題の対処](https://zenn.dev/yasumichi/articles/823e8d61867102)
- [GitBucket Markdown Enhanced Plugin でプレビュー時に mermaid 図形が描画されない不具合を修正しました。 #JavaScript - Qiita](https://qiita.com/yasumichi/items/903eaf20276b2c01b8fa)
- [GitBucket Markdown Enhanced Plugin でも issue リンクが使えるようになりました #Scala - Qiita](https://qiita.com/yasumichi/items/7e3845aa96733d41627b)
- [GitBucket Markdown Enhanced Plugin に WaveDrom サポートを追加しました #Scala - Qiita](https://qiita.com/yasumichi/items/78f3e44fb22b93cd52b9)
- [GitBucket Markdown Enhanced Plugin に Graphviz の描画と自動リンク機能を追加しました #Scala - Qiita](https://qiita.com/yasumichi/items/d4b0cf525470a98e86dc)
- [GitBucket Markdown Enhanced Plugin でシンタックスハイライトを有効にする #JavaScript - Qiita](https://qiita.com/yasumichi/items/aa52da49d52b89ecfde6)
- [PlantUML の SourceStringReader.generateImage() は deprecated だってさ #CSS - Qiita](https://qiita.com/yasumichi/items/e55b27dd07f9d41821af)
- [GitBucket Markdown Enhanced Plugin で KaTeX の記法を Markdown Preview Enhanced に合わせました #Scala - Qiita](https://qiita.com/yasumichi/items/c270c701ed270f622a76)
- [GitBucket Markdown Enhanced Plugin 独自のインライン記法を表に入れると例外が発生し描画ができない問題を解消しました #Scala - Qiita](https://qiita.com/yasumichi/items/a24742d5a129732fb0ea)
- [GitBucket Markdown Enhanced Plugin でリンクの解決を見直しました #Scala - Qiita](https://qiita.com/yasumichi/items/891578c48573de80b412)
:::

## GitBucket Flexible Gantt Plugin

[GitBucket Flexible Gantt Plugin](https://github.com/yasumichi/gitbucket-flexible-gantt-plugin) は、issue に以下の項目を関連付け、ガントチャートに反映できるプラグインです。ガントチャートを表示する部品として、[Frappe社](https://frappe.io/)の [Frappe Gantt](https://github.com/frappe/gantt) を採用しています。

- 開始日
- 終了日
- 進捗率
- 依存 issue

GitBucket のコミュニティプラグインには、既にガントチャートを表示できる [Gantt Chart plugin](https://github.com/kasancode/gitbucket-gantt-plugin) がありますが、期間を設定できないのが、個人的に不満でした。そこで作成したのが、このプラグインです。

インストールすると登録済み issue の右側に次のようなパネルが表示されます。

![](https://github.com/user-attachments/assets/ac85fd39-50be-42d5-a849-c7372a06ad36)

右上の鉛筆アイコンをクリックすると編集モードに移行します。

![](https://github.com/user-attachments/assets/160f056e-52a8-446b-8c1c-70782702b288)

`Dependencies` のテキストボックス右にある ![](https://github.com/user-attachments/assets/aed49e29-ca48-4527-883e-fe8c6c522d6e) をクリックすると issue の一覧がダイアログで表示されます。

![](https://github.com/user-attachments/assets/c2ffae5f-fb97-4b39-a27d-d55d3b7813a2)

依存させたい issue のチェックボックスを選択し、

![](https://github.com/user-attachments/assets/519f3786-6ffb-47d6-a6a2-1109129a5b79)

`Update` をクリックするとテキストボックスに反映できます。

![](https://github.com/user-attachments/assets/56747dee-80cb-4a2b-a50d-965109b84d8a)

issue に設定した情報を元にリポジトリのサイドメニュー `Flexible Gantt` をクリックすると以下のようにガントチャートが表示されます。

![](https://github.com/user-attachments/assets/1e5328de-fab2-46de-8ac3-1c2bfd801d89)

ガントチャート上のバーでドラッグ＆ドロップにより、issue の期間の変更や進捗率の変更も可能です。

また、バーをクリックすると該当の issue を新しいウィンドウ(ブラウザの設定により別タブ)で開きます。ただし、上記のドラッグ＆ドロップで反応してしまう場合もあり、改善方法を検討中です。

この画面から新しい issue を作成することもできます。

パネルの上にフィルタリングのメニューを用意してあります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/36738/2b21167c-fbd3-49b8-b977-0ba48da05821.png)

`Milestone` から絞り込みたいマイルストーンを選択するとタスクが絞り込まれます。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/36738/7096bf4b-997b-44c9-b114-c2bc8669b6d5.png)

掲載している画面では、英語の issue のみですが、日本語の issue でも問題ありません。

### 関連記事

- [GitBucket で issue に開始日・終了日・進捗率等を追加しガントチャートに表示するプラグインを作成してみた #Scala - Qiita](https://qiita.com/yasumichi/items/3f2f560b570a0aab4bbd)
- [GitBucket Flexible Gantt Plugin 0.1.1 をリリースしました #JavaScript - Qiita](https://qiita.com/yasumichi/items/4c7a806bb12ba85ec8f7)
- [GitBucket Flexible Gantt Plugin でマイルストーンやラベルでフィルタリングできるようにしました #Scala - Qiita](https://qiita.com/yasumichi/items/3642c690846098f72d47)
- [GitBucket Flexible Gantt Plugin に依存 issue の入力支援ダイアログを追加しました #JavaScript - Qiita](https://qiita.com/yasumichi/items/431c5f411addfe39313d)

## 今後やりたいこと

- GitBucket Markdown Enhanced Plugin の機能追加
- [gitbucket-commitgraphs-plugin](https://github.com/yoshiyoshifujii/gitbucket-commitgraphs-plugin) が、インターネットに接続できない環境だとグラフが表示されないので修正したい。
- GitBucket 本体に同梱された JavaScript 等のライブラリにかなり古いものがあるので最新化したい。