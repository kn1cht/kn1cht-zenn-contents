---
title: "sb2re-online: 小さいWebツールをDenoで作ってGitHub Pagesに公開してみた"
emoji: "🌐"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - deno
  - webアプリ
  - scrapbox
published: true
---

# TL;DR

- **Scrapbox記法の文章をRe:VIEW記法へ変換**してくれるWebアプリOnline ScrapBox to Re:VIEW Converter (sb2re-online) を作りました。
- Deno向けのバンドルツール**packup**でビルドし、GitHub Pagesで公開しています。

https://twitter.com/kn1cht/status/1538518222755942402

[kn1cht.github.io/sb2re-online/](https://kn1cht.github.io/sb2re-online/)で使えます。

https://kn1cht.github.io/sb2re-online

リポジトリはこちらです。

https://github.com/kn1cht/sb2re-online

# この記事で得られるかも知れない情報

- Deno製のコマンドラインツールをWebアプリ化する方法
    - sb2re-onlineはブラウザだけで動く小さいアプリなので、バンドルツールの`packup`で静的ファイルをビルドしています
    - DenoのWebアプリをデプロイする方法としては[Deno Deploy](https://deno.com/deploy)が有名ですが、サーバーサイドがいらない用途だったため、公開にはGitHub Pagesを使いました
    - 内部で呼び出したツールが出してきたエラーを、ページに分かりやすく表示する仕組みも入れました

# モチベーション

筆者が所属する同人サークル「[東京大学きらら同好会](https://utkiraracircle.github.io/)」では、複数人で制作する同人誌の原稿を、Wikiツールの[Scrapbox](https://scrapbox.io/)で執筆しています。
また、合同誌として組版する段階では組版ソフトウェア[Re:VIEW](https://reviewml.org/ja/)を使用しています。

Scrapboxは独自のマークアップ形式を採用しており、Re:VIEWで使用するためにはフォーマットの変換が必要です。この目的のため、サークル会員のふぁぼん氏が**Scrapbox記法からRe:VIEW記法への変換ツール**`sb2re`を実装してくれました。

https://twitter.com/syobon_hinata/status/1471864549720883204

`sb2re`はDenoで作られていて、`deno install`でインストールすれば気軽に使えます。ただ、「Denoをインストール→sb2reをインストール」という手順をいちいち踏むよりも、ブラウザだけでいきなり使えたほうがさらにさらに気軽でしょう。

![sb2re-onlineの画面](/images/sb2re-online/sample.png)

というわけで、sb2re-onlineができました。左のボックスにScrapboxのテキストを貼り付けると、右のボックスにリアルタイムでRe:VIEWへの変換結果が出てきます。

# 技術選定と初期の実装

`sb2re`がDeno製なので、ブラウザ版もDenoで作りたいな～ということでDenoでフロントエンド開発する方法を調べました。なお、筆者はDeno初体験のNode.jsキッズです。

## ビルドツール（packup）

Denoで使えるWebフレームワークはかなりいろいろな種類が出てきています。今回の用途では処理がブラウザだけで完結するので、**なるべくシンプルにフロントエンドだけを扱うもの**がいいと思いました。

https://scrapbox.io/uki00a/Deno%E3%81%AE%E3%83%95%E3%83%AD%E3%83%B3%E3%83%88%E3%82%A8%E3%83%B3%E3%83%89%E3%83%95%E3%83%AC%E3%83%BC%E3%83%A0%E3%83%AF%E3%83%BC%E3%82%AF%E3%81%AE%E6%AF%94%E8%BC%83

というわけで選んだのが[packup](https://github.com/kt3k/packup)です。設定ファイルいらずで、エントリポイントの`index.html`を渡せばビルドしてくれるシンプルさに惹かれました。

https://zenn.dev/kt3k/articles/1df2e54cd9d4f3

## React

フロントエンドのライブラリにはReactを使いました。Deno世界ではSkypack経由でimportできます。
textareaの更新を検知するためにReact Hook Formも入れておきました。

```ts:app.tsx
import React from "https://cdn.skypack.dev/react@18.1.0?dts";
import ReactDOM from "https://cdn.skypack.dev/react-dom@18.1.0?dts";
import { useForm } from "https://cdn.skypack.dev/react-hook-form?dts";
```

## sb2re

Scrapbox→Re:VIEW変換ツール`sb2re`をそのまま使います。
Deno同士なのでそのままGitHubの`.ts`ファイルをimportしてくれば完了です。Deno最高！

```ts:app.tsx
import scrapboxToReView from "https://raw.githubusercontent.com/fabon-f/sb2re/master/mod.ts";
```

## 実装

実装といっても、textareaに入ってきた文字列を`sb2re`に投げつけて結果を表示させるだけなので、新規に書くことはほとんどありません。

```ts:app.tsx
function App() {
  const { register, watch } = useForm();
  const watchInput = watch("input", sampleTxt);
  return (
    <div id="app">
      <div class="nav">
        <h1>sb2re-online: Online Scrapbox to Re:VIEW Converter</h1>
      </div>
      <div class="editor">
        <div class="editor_wrapper">
          <textarea value={watchInput} class="input" {...register("input")}></textarea>
        </div>
        <div class="editor_wrapper">
          <textarea value={scrapboxToReView(watchInput.trim())} class="output"></textarea>
        </div>
      </div>
    </div>
  );
}

addEventListener("DOMContentLoaded", () => ReactDOM.render(<App />, document.querySelector("#main")));
```

なんとメイン部分20行くらいで立派なWebアプリが完成しました（`index.html`と`style.scss`は別）。Deno最高！（2）

## ビルド

`packup build index.html`すると`dist/`以下にHTML, JS, CSSが書き出されます。

## 公開

Deno製Webアプリのデプロイ先としては[Deno Deploy](https://deno.com/deploy)がよく使われている印象です。ただ、sb2re-onlineは静的ファイルをブラウザに読ませるだけでOKなので、サーバーサイドの機能までは必要としていません。

そこで、packupのビルド結果をGitHub Pagesにデプロイすることとしました。
GitHub Actionsで`packup build`し、[peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)によって`dist/`を`gh-pages`ブランチにコミットする仕掛けです。

```yml:.github/workflows/gh-pages.yml（抜粋）
    steps:
      - uses: actions/checkout@v2
      - uses: denoland/setup-deno@v1
      - name: Install packup
        run: deno run -A https://deno.land/x/packup@v0.1.13/install.ts
      - name: packup build
        run: packup build index.html
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

# ツール側が出してきたエラーをWebページに表示させる

メイン部分はここまでで完成なので余談です。

`sb2re`はScrapbox記法をパースしてRe:VIEWに変換しています。**その途中でエラーが起きると、例外を投げて止まる**場合がありました。

コマンドラインツールなら「じゃあ直して再実行しようか」で済む話です。
一方、Reactで特に対策なくエラーが発生すると、何の前触れもなく**画面が真っ白になって操作不能**になります。ユーザはエラーの原因が分からないままページを再読み込みするしかありません。
さらには、sb2re-onlineでは文字を入れた瞬間に`sb2re`が実行されるため、エラー原因を特定するまで何度もWebアプリが停止してしまいます。

これはユーザ体験が悪すぎるということで、try-catchでエラーを受け取って`react-notifications`へ渡すようにしました。

```ts:app.tsx
import { ReactNotifications, Store } from "https://cdn.skypack.dev/react-notifications-component?dts";

// （中略）

function executeSb2re(watchText: string, option: {}) {
  let converted = "";
  try {
    converted = scrapboxToReView(watchText, option);
  } catch(e) {
    Store.addNotification({
      title: "Please fix your Scrapbox syntax",
      message: e.toString(),
      type: "danger",
      container: "bottom-left",
      dismiss: {
        duration: 6000,
        onScreen: true,
        showIcon: true,
      },
    });
  }
  return converted;
}
```

これで、エラーになるような構文が入力されてもWebアプリは落ちず、さらに通知メッセージからエラー原因を推定できるようになりました。

![Error Notification](https://i.gyazo.com/580fb16b6121f5848eb0f4eb6a19d8f6.gif)

また、以前の`sb2re`ではエラーを示す方法として`throw new Error()`と`console.error()`が混在していました。後者はブラウザの開発者ツールを開かなければ見ることもできないので、作者のふぁぼん氏に相談したら、エラー時に呼ばれる関数を外から指定できるオプションを用意してくださいました。感謝――。

# まとめ

Scrapbox記法からRe:VIEW記法への変換を気軽にできる小さなWebツールsb2re-onlineをDenoで制作しました。

https://kn1cht.github.io/sb2re-online

「Denoは開発体験がいいぞ」と色んなところで耳にしていたとおり、**ミニマルな実装の完成までがたいへんスムーズ**で驚きでした。これ試したいな、と思ったときにCDNやGitHubのURLをちょろっと書くだけで取得してきてくれるのは楽しいですね。
今後もDenoでなにか作ってみたくなりました。

# 宣伝

Scrapbox, Re:VIEW, sb2re-onlineを活用して制作したまんがタイムきららの合同誌「Micare vol. 2」が、コミックマーケット100 土曜日東地区Y16a「**東京大学きらら同好会**」で頒布されます！　夏コミにお越しの方はぜひ！！

https://twitter.com/UTKiraraCircle/status/1535190931204624385
