# Slidevを導入してMarkdownで美しいスライドを書こう

[Slidevを導入してMarkdownで美しいスライドを書こう - Qiita](https://qiita.com/loftkun/items/2fbeddc9449eb5d85dfd) の記事もご参照ください。

このMarkdownはQiita向けの記法を使用している箇所がありますのでQiitaの方が読みやすいと思います。

## Slidevとは

[Slidev](https://sli.dev/)はエンジニア向けプレゼンテーションツールです。
Markdownで記述した文書から美しいスライドを生成できます。

[![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/21106/7d8c1664-4f49-a55c-c8cf-985fdee98e48.png)](https://demo.sli.dev/starter/)

[公式デモ](https://demo.sli.dev/starter/) とそのソースとなる[Markdown](https://github.com/slidevjs/slidev/blob/main/packages/create-app/template/slides.md) もご参照ください。

ドキュメントは以下にあります。

https://sli.dev/guide/

## 目次

本記事は以下について記載しています。

**1. 注目の機能**

私はこれまでに類似のツールとして、[reveal.js](https://revealjs.com/) や [Marp](https://marp.app/) を使ったことがありますが、現在はSlidevをメインで使っています。Slidevの良い点を紹介したいと思います。

**2. Slidevを使ってみよう**

使い始めるための環境や設定について紹介します。
1コマンドでインストール、スライド生成、ブラウザでの表示までできるので楽です。
`Node.js >=14.0`が必要ですのでその点はお気をつけを。

**3. サンプルMarkdown**

基本的な記法を網羅し、かつ勉強会などのプレゼンでよく使いそうなレイアウトや機能を盛り込んだサンプルを用意しました。
コピペしていただけると記法をざっと確認でき、すぐにオリジナルのスライド作成に専念いただけると思います。

それでは、注目の機能からご紹介します！

## 1. 注目の機能

:bulb:  2021/07/28にリリースされた [v0.22.7](https://github.com/slidevjs/slidev/releases/tag/v0.22.7) を元に記載しています。

### 1.1. 柔軟なレイアウト

Slidevは [Windi CSS](https://windicss.org/) ( [Tailwind CSS](https://tailwindcss.com/) 互換 ) を採用しておりGridレイアウトを指定することができます。
上下左右自由にコンテンツを配置できますので、紙面を最大限有効に活用してスライドを作成できます。

![grid.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/21106/1a788ca6-3d63-735e-f01c-cec803bba13f.jpeg)

上記はスライドをプレビューしながらMarkdownを編集している時のスクショです ( [関連ツイート](https://twitter.com/loftkun/status/1414180818717528066) ) 。
Gridレイアウトを使用しているため、右側のプレビュー画面で、同一ページ内にコンテンツを左右に配置している領域とそうでない領域を表示できていることにご注目ください。

Gridレイアウトの活用により上記例の`ながい文章です・・・`の領域のように、スライドの幅を目一杯使える領域と、`before`、`after`の領域のように左右に分割している領域を混在できています。

このようにページの途中であっても領域分割する/しないを使い分けられます。
私が試した範囲ではSlidevはこのレイアウトを簡単に実現できました。

後ほどご紹介するサンプルMarkdownにこのようなGridレイアウトの記法を盛り込んでいますのでご参照ください。

参考ドキュメント : Grids | Slidev

https://sli.dev/guide/faq.html#grids

この種のツールはMarkdownに記述した文章や画像表示を順序通り上から下に並べてスライドに変換するのが基本的な動作となりますが、ページ内で左右にコンテンツをレイアウトする機能が整備されているかはスライド作成ツール選定時の重要なポイントだと思います。

### 1.2. シンタックスハイライトと行番号指定

コマンドやソースコードをスライドに掲載する際、あまり労力をかけず、かつ良い感じのシンタックスハイライトを施したいですよね。
私は以前まで [Carbon](https://carbon.now.sh/) というサービスにコードを貼り付けてシンタックスハイライトされた画像を生成してスライドに貼り付けていました。

この方法のデメリットは画像化してしまうため、文字列のコピーができなくなることです。スライドを公開してコマンドやコードをコピーして使ってもらうことができません。
また、検索に引っかからないことと、コードを変更した場合、画像を生成し直す手間がかかることもデメリットです。

SlidevはMarkdownに ``` で記述したコードブロックをそのままシンタックスハイライトして表示してくれますので、前述のデメリットは解消されます。
文字列のコピーや検索も可能ですし、Markdownのコードブロックを書き直せばプレビューもそれに追従します。
画像の準備から開放され、執筆に専念できるようになりました。

以下例のように行番号の表示も可能です( `v0.22.6` 以降 )。

![syntax.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/21106/73d44462-9e1d-8dd2-de59-6baeded69506.png)

特定の行をハイライトさせることも可能です。
上記の例では4行目以降をハイライトさせることで左右のコードを比較する際に1-3行目は着目する必要がないことも示しています。

ハイライトする行を指定する場合は以下のように`{}`の中に行番号範囲を記述します。

<pre>```python {4-}
import os
test_path = os.path.join("data", "data-01.txt")

f = open(test_path, "a", encoding="utf-8")
f.write("this is new append line\n")
f.close()
&#096;&#096;&#096;
</pre>

3行目までなら`{-3}`、4行目以降なら`{4-}`、1,3,5行目なら`{1,3,5`}のように書けます。

参考ドキュメント : Code Blocks | Slidev

https://sli.dev/guide/syntax.html#code-blocks

普段コーディングしているエディタに近いシンタックスハイライトされた表示でプレゼンでき、コードについて話をしたり質問を受けたりする際に行番号で着目箇所を伝達できるのは便利なポイントです。

### 1.3. 個別に style を指定できる

ある特定のページ内のみ文字サイズなどのデザインを変更したい場合は多々あります。
そのような場合、以下のように &lt;style&gt;タグ をページ内に記述できます。

<pre>---

## 2. 第1章 このページにあるコードブロック内の文字サイズは10pxです

&lt;style&gt;
code{
    font-size: 10px;
}
&lt;/style&gt;

ほげほげ

```python
ここはコードブロックです。
ここの文字サイズは10pxになります。
&#096;&#096;&#096;

---

## 第2章 このページは上記の&lt;style&gt;の影響は受けません

ふがふが
</pre>

ページ内でのみ有効なスタイルを設定できますので、微調整したい際、かゆいところに手が届きます。

参考ドキュメント : Override Local Style | Slidev

https://sli.dev/guide/faq.html#override-local-style

### 1.4. mermeid記法による図の挿入

[mermeid記法](https://mermaid-js.github.io/mermaid/#/)による図(SVG)の挿入ができます。

![syntax.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/21106/268b9128-22f5-e392-a279-1c06cfdb35c9.png)

![syntax.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/21106/c2460893-cf7e-d40b-bc96-251011b357da.png)

参考ドキュメント : Diagrams | Slidev

https://sli.dev/guide/syntax.html#diagrams

また、[Draw.io - Diagrams.net](https://app.diagrams.net/) で作成した SVGなども`![TITLE](URL)`記法で画像として挿入できます。
Draw.ioの方が作図の自由度は高いと思いますので使い分けると良さそうですね。
VSCode上でDraw.ioのエディタをオフラインで使える [Draw.io Integration](https://marketplace.visualstudio.com/items?itemName=hediet.vscode-drawio) がおすすめです。

### 1.5. Vueコンポーネントの追加

SlidevはVueベースで作成されておりVueのコンポーネントを使用できます。
以下はTwitterコンポーネントの使用例です。簡単にツイートを埋め込むことができます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/21106/283565c7-b3f5-3ab8-8700-ab72a568ee5b.png)

コンポーネントは以下のように簡単に呼び出せます。

```
<Tweet id="1423237009561186308"/>
```

他にも[公式デモ](https://demo.sli.dev/starter/5)にCounterのサンプルがあります。

参考ドキュメント : Components | Slidev

https://sli.dev/custom/directory-structure.html#components

### 1.6. HTMLを埋め込める

SPAとして動作しますのでHTMLを記述できます。
YouTube動画などの埋め込みコードをそのまま貼り付けることができます。

![syntax.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/21106/a6da7ff0-c095-8d18-cd12-753ed5c3e513.png)


## 2. Slidevを使ってみよう

それではさっそくSlidevを使ってみましょう。`Node.js >=14.0`が入っている環境をご用意ください。

### 2.1. インストール

インストールコマンドは以下です。

```bash
$ npm init slidev@latest
```

もしくはyarnでもインストールできます。

```bash
$ yarn create slidev
```

上記コマンドを実行すると、Slidevのインストール後、同梱されている `slides.md` をソースとして生成したスライドをブラウザで閲覧できます。

実行例

以下のように`Project name`や使用するパッケージマネージャーを聞かれますのでお好みで選択します。

```bash
loft@ubu20-02:~/dev/slidev$ npm init slidev@latest
npx: installed 22 in 2.462s

  ●■▲
  Slidev Creator  v0.22.7

✔ Project name: … slidev-test
  Scaffolding project in slidev-test ...
  Done.

✔ Install and start it now? … yes
✔ Choose the agent › npm

( 略 )

> slidev-test@ dev /home/loft/dev/slidev/slidev-test
> slidev --open



  ●■▲
  Slidev  v0.22.7 

  theme   @slidev/theme-seriph
  entry   /home/loft/dev/slidev/slidev-test/slides.md

  slide show      > http://localhost:3030/
  presenter mode  > http://localhost:3030/presenter
  remote control  > pass --remote to enable

  shortcuts       > restart | open | edit
```

これだけの手順でSlidevを利用可能です。ブラウザで`http://localhost:3030/`にアクセスすると、同梱されている `slides.md` をソースとした以下のようなSPAを確認できるはずです。

![syntax.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/21106/7d8c1664-4f49-a55c-c8cf-985fdee98e48.png)

この後は`slides.md`を編集してオリジナルのスライドに仕上げていけばOKです。
SPAにリアルタイムに反映されますので、プレビューしながらスライドを執筆でき効率良く作業できます。

画面左下のNavigation Barで以下の操作が可能です。
[ドキュメント](https://sli.dev/guide/navigation.html)にショートカットなども載っていますのでご参照ください。

- [スライドのタイル表示](https://sli.dev/guide/navigation.html#slides-overview)
- ダークモード切り替え
- [Presenter Mode](https://sli.dev/guide/presenter-mode.html)に切り替えてNoteを確認
    - 各ページに記載した最後のコメント ( `<!-- この部分 -->` )がNoteになります。
- [カメラ映像を表示する](https://sli.dev/guide/recording.html#camera-view)
    - D&Dで自由に移動できます。
- スクリーンを[録画](https://sli.dev/guide/recording.html)して動画を作成
    - 動画はダウンロードフォルダに生成されます。

ディレクトリ/ファイル構成は以下のようになっています。この一式をgitで管理すると良いです。

```bash
loft@ubu20-02:~/dev/slidev$ ls -Fla slidev-test/
total 144
drwxrwxr-x   4 loft loft  4096  8月  6 20:41 ./
drwxrwxr-x   6 loft loft  4096  8月  6 20:41 ../
drwxrwxr-x   2 loft loft  4096  8月  6 20:41 components/
-rw-rw-r--   1 loft loft    78  8月  6 20:41 .gitignore
-rw-rw-r--   1 loft loft   163  8月  6 20:41 netlify.toml
drwxrwxr-x 249 loft loft 12288  8月  6 20:41 node_modules/
-rw-rw-r--   1 loft loft    33  8月  6 20:41 .npmrc
-rw-rw-r--   1 loft loft   275  8月  6 20:41 package.json
-rw-rw-r--   1 loft loft 89328  8月  6 20:41 package-lock.json
-rw-rw-r--   1 loft loft   267  8月  6 20:41 README.md
-rw-rw-r--   1 loft loft  8180  8月  6 20:41 slides.md
-rw-rw-r--   1 loft loft    80  8月  6 20:41 vercel.json
loft@ubu20-02:~/dev/slidev$ 
```

### 2.2. 2回目以降の起動

2回目以降は、生成されたディレクトリに移動して以下コマンドを実行して起動できます。

```bash
$ npm run dev -- ${Markdownファイルのパス}
```

実行例

```bash
loft@ubu20-02:~/dev/slidev$ cd ./slidev-test/
loft@ubu20-02:~/dev/slidev/slidev-test$ npm run dev -- slides.md 
```


なお、Slidevをインストールしたリモートサーバに、VSCodeのRemote SSHで接続し、
VSCodeのターミナルでSlidevを起動すると、VSCodeが待受けポートを検出しport-forwadして、VSCodeをインストールしているローカルマシンでブラウザまで立ち上げてSPAを表示してくれますのでVSCode + Remote SSHで開発するのも便利です。

### 2.3. 公開用のSPAを生成する

公開用のSPAを生成する場合は以下コマンドを実行します。

```bash
$ npm run build -- ${Markdownファイルのパス}
```

実行例

```bash
loft@ubu20-02:~/dev/slidev/slidev-test$ npm run build -- slides.md 

> slidev-test@ build /home/loft/dev/slidev/slidev-test
> slidev build "slides.md"



  ●■▲
  Slidev  v0.22.7 

  theme   @slidev/theme-seriph
  entry   /home/loft/dev/slidev/slidev-test/slides.md


vite v2.4.4 building for production...
transforming (217) node_modules/@slidev/theme-seriph/styles/prism.cssUse of eval is strongly discouraged, as it poses security risks and may cause issues with minification
✓ 222 modules transformed.
( 略 )
loft@ubu20-02:~/dev/slidev/slidev-test$ ls -Fla dist/
total 16
drwxrwxr-x 3 loft loft 4096  8月  6 21:20 ./
drwxrwxr-x 5 loft loft 4096  8月  6 21:20 ../
drwxrwxr-x 2 loft loft 4096  8月  6 21:20 assets/
-rw-rw-r-- 1 loft loft  736  8月  6 21:20 index.html
loft@ubu20-02:~/dev/slidev/slidev-test$ 
```

`dist`ディレクトリの内容物一式をサーバーにデプロイするとSPAとしてスライドを公開することができます。

デプロイ先は普通のHTTPサーバーでも GitHub Pages でも良いですが、`netlify.toml` や `vercel.json` が同梱されており、ビルドコマンド、公開ディレクトリ、`redirects`設定などが記述されていることから、[Netlify](https://www.netlify.com/) や [Vercel](https://vercel.com/dashboard) にデプロイすることが意識されており、設定ファイルの恩恵を受けられます。( 開発者の [Anthony Fu](https://twitter.com/antfu7)氏は Netlifyを推奨するとDiscord上で発言していました )。
どちらもGitHubのリポジトリを指定するとWebアプリとしてデプロイできるサービスでCDが無料かつdefaultで動きますので、どちらかにデプロイすると良いと思います。

Slidevのインストール手順や基本的な使用方法をご紹介しました。

## 3. サンプルMarkdown

基本的な記法を網羅し、かつ勉強会などのプレゼンでよく使いそうなレイアウトや機能を盛り込んだサンプルMarkdownを用意しました。

https://github.com/loftkun/slidev-example

このサンプルリポジトリの[slides.md](https://github.com/loftkun/slidev-example/blob/main/slides.md)をコピペしていただけると記法をざっと確認でき、すぐにオリジナルのスライド作成に専念いただけると思います。


NetlifyにSPAをデプロイしています。

https://610e32261ec7261030b9c1db--relaxed-varahamihira-0ef9b6.netlify.app/

いちおう GitHub Pages も用意しています( redirect系設定が適用されないのでリロードはできません )。

https://loftkun.github.io/slidev-example/dist/

以下に補足を記載します。

## 3.1. Frontmatter

サンプルMarkdownの冒頭部分は以下のようにしています。

```yaml
---
theme: seriph
title: SlidevのMarkdown記法サンプル
download: false
lineNumbers: true
background: https://source.unsplash.com/collection/94734566/1920x1080
class: 'text-center'
---
```

Markdown冒頭は`Frontmatter`と呼ばれる領域で、Slidevの設定を記述します。

テーマは`seriph`としています。`default`にはない`layout: cover`が使えるので採用してみました。

その他のテーマは以下で確認できます。

https://sli.dev/themes/gallery.html

`download`を`true`にすると Navigation Bar にPDFのダウンロードボタンが出現します。

PDFの生成には`playwright-chromium`パッケージが必要です。

```bash
$ npm install playwright-chromium
```

ビルド時間が長くなりますので不要な場合は`false`にしておくと良いです。
また、SPAとPDFではレンダリング結果が変わるため、SPAできっちり揃えたレイアウトがPDFでは崩れている可能性が高いことにも注意が必要です。

その他、Frontmatterに記述できる設定は以下をご参照ください。

https://sli.dev/custom/#frontmatter-configures

## 3.2. Gridレイアウト

いくつかのページで使っていますが、以下ページは3分割の例です。

[![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/21106/bf405817-7b26-b914-e91d-f3817bd05bf4.png)](https://610e32261ec7261030b9c1db--relaxed-varahamihira-0ef9b6.netlify.app/8)

Markdownは以下のように記述します。等分割でなく%で指定できるのも便利です。

```markdown

---

## Gridレイアウトを使うぞ

ここは横幅いっぱい使えるよ

<div class="grid grid-cols-[33%,33%,33%] gap-4"><div>

左

</div><div>

中央

</div><div>

右

</div></div>

ここは横幅いっぱい使えるよ

```

## 3.3. 行番号の表示/非表示を切り替える

以下ページではPythonのコードは行番号を表示し、bashコマンドには表示していません。

![syntax.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/21106/5f6b7644-52ca-9adc-786c-b78817621f24.png)

現状、1ページ内で表示/非表示をきめ細かく設定する方法は用意されていません。
やや邪道なのですが、bashコードのコードブロックの内容を左にずらすstyleを適用して行番号部分を隠すようにしています。

```markdown
<style>
.language-bash span.line { /* bashのコードブロック */
  margin-left: -40px; /* 左に40px移動して行番号を隠す(邪道) */
}
</style>
```

## 4. まとめ

Markdownでスライドを記述できるSlidevについてご紹介しました。
Slidevを使うことでデザインの調整が最低限で済むためプレゼン内容の執筆に専念できます。

しばらく活用している内にケアすると良い点やTipsが溜まってきていますので、機会があれば別記事にしたいと思います。
本記事がMarkdownからスライドを作成するためのご参考になりましたら幸いです。



