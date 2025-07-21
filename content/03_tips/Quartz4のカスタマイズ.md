---
tags:
  - tips
---
# Quartz 4のカスタマイズ
## 基本的なカスタマイズ

カスタマイズは公式のページを確認すれば詳しい説明と設定が確認できる。

> [Plugins](https://quartz.jzhao.xyz/plugins/)

とりあえず改行関連のプラグインPlugin.HardLineBreaks()を導入する。これを導入することで見た目通りの改行になる（はず）。

```
...
  plugins: {
    transformers: [
      Plugin.HardLineBreaks(), ←これを追加
      Plugin.FrontMatter(),
      Plugin.CreatedModifiedDate({
        priority: ["frontmatter", "git", "filesystem"],
      }),
      Plugin.SyntaxHighlighting({
        theme: {
          light: "github-light",
          dark: "github-dark",
        },
        keepBackground: false,
      }),
...
```

タイトルの変更は以下の部分から。
```
pageTitle: "任意のタイトル",
```

日本語のWebサイトなのでロケールを日本語に変更。
```
locale: "ja-JP",
```

フォントは以下の部分から。以下の設定はこのWebサイトのもの。
```
typography: {
  header: "M PLUS 1p",
  body: "Noto Sans JP",
  code: "IBM Plex Mono",
},
```

グラフは必要ないので削除。
```
right: [
  Component.Graph(), ←これを削除
```
## 目次のサイズを変更してみた

右サイドバーに表示される目次について。
[[さよならを云って]]のように長大なコンテンツの場合、目次が多くなる。その場合、初期設定では画面の一番下まで目次が表示されてしまい、バックリンクが隠れてしまう。
そこで、以下のように最大の高さの設定をcustom.scssに追加した。

```
div.toc {
    max-height: 70%;
  }
```

%の部分を好みに設定してくだちい。%設定じゃなくてもremとかpxでもいいぞよ。まあレスポンシブ的には%がいいはず。よくわかんないけど。

## 行間の調整

初期設定ではすこし詰まりすぎなので行間を調整。ローカルで確認しながら適宜好みの値にしてください。こちらもcustom.scssに追加。基本的にcssを弄るときはcustom.scssに追加するのが吉っぽい。

```
p {
    line-height: 1.8;
    padding: 0.5;
}
```

## 3行以上の改行の表示について

プラグインやCSSの調整でなんとかならないかと試行錯誤しているが、現状は思うようにできていない。そのため`<br>`タグを文中に挿入して力業で突破している。

## Google Search Consoleの利用

アナリティクスについてはGoatcounterを利用することにした。
コンフィグの`analytics: {},`でアナリティクスを簡単に設定できる。Goatcounterの場合はコードを挿入するだけでいい。

ちゃんと検索エンジンに登録されているか確認したいため、Search Consoleを利用したい。しかしこの設定がかなり難しかった。公式のプラグインでは対応していないため、メタタグの挿入やHTMLのアップロードで対応する必要がある。しかし、HTMLのアップロードについてはどのフォルダに置いて`sync`しても、Githubに直接アップしてもSearch Consoleが認識してくれなかった。まだやれてないことがあるかもしれないが、諦めた。

そのため、メタタグ`<meta name="google-site-verification"......`の挿入で対応することにした。しかし、Quartz 4は直接HTMLをいじれる構造ではないため、メタタグが挿入される設定ファイルをいじる必要があった。

その設定ファイル`Head.tsx`だった。このファイルをメタタグ`<meta name="google-site-verification"......`を挿入することでちゃんとHTMLに反映された。もちろんSearch Consoleも認識してくれた。