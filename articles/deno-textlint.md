---
title: "textlintをDenoで動かした"
emoji: "✍️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
    - textlint
    - deno
    - cicd
published: true
---

:::message
本記事は、[CI/CD Advent Calendar 2022](https://qiita.com/advent-calendar/2022/cicd)および[Deno Advent Calendar 2022](https://qiita.com/advent-calendar/2022/deno) 18日目の記事です。
Denoカレンダーの昨日の記事は、windchime-ykさんの「[DenoとPlanetScaleを繋げて簡単なAPIを作る](https://zenn.dev/windchime_yk/articles/deno-adapt-planetscale)」でした。本記事と同じくDenoのnpm互換性を活用した記事でしたね。
どちらのカレンダーも明日以降まだ枠が空いています。これを読んでいるあなたも是非。
:::

# TL;DR
- Denoでnpmパッケージが使えるようになったらしいのでtextlintを動かしてみました
- ただし、少し工夫が必要です
- Node.jsに比べて絶大なメリットがあるわけではないですが、`node_modules`が嫌な方には有益です

![](/images/deno-textlint/success-plugin.png)

試した結果は次のリポジトリに公開しています。

https://github.com/kn1cht/run-textlint-on-deno

# はじめに
2022年秋、Denoが正式にnpmモジュールに対応したとのニュースが飛び交いました。

https://deno.com/blog/v1.28

これを聞いて何かしら遊びたいと思って考えた結果、**Node.jsで動く文章校正ツールであるtextlint**をDenoで動かしてみることにしました。

https://textlint.github.io/

textlintは技術解説書、論文、小説など様々な執筆現場で活用されています。ツールとして使う上では手軽に取り扱える方が嬉しいので、シンプルな操作を目指しているDenoで動かしたらどうなるのか気になりました。

結論から言えば動きました。ただし、Node.jsで使うときとは少し違った工夫が求められるので、本記事で解説します。

# 環境
- Deno 1.29.1
- textlint v12.2.4

# textlintを動かす
## CLIで使う（失敗）
多くの方は、textlintを`$ textlint ariticle.md`のようにCLIで使っていると思います。CLIを提供しているnpmパッケージも`$ deno run --allow-env --allow-read npm:{package name}`のような感じで動かせるそうなので、まずはそれをやってみましょう。

```sh
$ echo "吾輩はは猫である。名前はまだないんです" > test.md
$ deno run --allow-env --allow-read --allow-sys \
npm:textlint --preset ja-technical-writing test.md
# Error
# Failed to load textlint's preset module: "ja-technical-writing" is not found.
# See FAQ: https://github.com/textlint/textlint/blob/master/docs/faq/failed-to-load-textlints-module.md
```

失敗してしまいました。textlintは、校正の判定基準である**ルール**・ルールをまとめた**プリセットがそれぞれ別のnpmパッケージ**として存在する設計です。つまりそれらを読み込めなければ当然動きません。

その後、ルールプリセットのパッケージをimportしてから実行しても、やはりFailed to load（略）になりました。
textlintがルール等を読み込むときには、グローバル/ローカルの`node_modules`からルールのパッケージを探してくれます。Denoのモジュール置き場は`node_modules`ではないので探しても見つからないわけですね。

READMEによると`--rules-base-directory`オプションで探す対象のディレクトリを変えられる、となっています。しかし、Denoのキャッシュディレクトリは`node_modules`と少し構造が異なる（パッケージ名の下にバージョンごとのディレクトリがある）ためこれでも上手く行きません。

```sh
$ deno run --allow-env --allow-read --allow-sys \
npm:textlint --preset ja-technical-writing \
--rules-base-directory ~/.cache/deno/npm/registry.npmjs.org test.md
# Error
# Failed to load textlint's preset module: "ja-technical-writing" is not found.
# See FAQ: https://github.com/textlint/textlint/blob/master/docs/faq/failed-to-load-textlints-module.md
```

現状では、textlintの中身を改造する方法を除けばDenoからtextlintのCLIを使うことは困難だと言えそうです。

## モジュールとして使う
ここで諦めかけましたが、textlintはプログラムの中でモジュールとして呼び出して使用することもできます。[公式READMEのUse as node moduleの部分](https://github.com/textlint/textlint#use-as-node-module)にやり方が書かれています。

上手くいったコードを示します。コツは**ルールプリセットも最初にimport**しておくことです。importしたものを使う必要はないので適当に捨ててしまって構いません。しかし行ごと消してしまうとエラーになります。

```ts:textlint.ts
import { TextLintEngine } from "npm:textlint";
import _ from "npm:textlint-rule-preset-ja-technical-writing";
const engine = new TextLintEngine();
const results = await engine.executeOnFiles(Deno.args);
if (engine.isErrorResults(results)) console.log(engine.formatResults(results));
```

```:.textlintrc
{
  "rules": {
    "preset-ja-technical-writing": true
  }
}
```

```sh
$ deno run --allow-env --allow-read --allow-sys textlint.ts test.md
```

![](/images/deno-textlint/success.png)
*出力。ミスがある文章を入力したため、ルールがエラーを3つ出している*

この方法の制限として、**公式のCLIを使っていない**ので、欲しいCLIの機能があれば自力で実装する必要があります。オプションを指定したいだけであれば`.textlintrc`に書いておく方法もあります。

## Denoの利点を活かせているのか？
Node.jsに対するDenoのメリットとしてよく言われるのは次のようなことですね。

- `package.json`なし：欲しいモジュールをURLで指定
- `node_modules`なし：読み込んだモジュールは作業ディレクトリには置かずキャッシュに保管
- TypeScriptのネイティブサポート
- 権限設定がありセキュリティに強い
- ロゴがかわいい

textlintを利用するときは、モジュールを読み込んで実行する作業がメインになるので、`package.json`と`node_modules`が作られないという点はメリットになり得ると思います。

それでは、**使い始めるときにいくつのファイルが必要か** （または、生成されるのか）を比べてみましょう。なお、textlintはローカルインストールとグローバルインストールの両方で使えますが、CIでの使用も見越してローカルインストールを想定します。

- Node.js
    1. `.textlintrc` : 各種設定
    2. `package.json` : 読み込むパッケージ、スクリプト指定など
    3. `package-lock.json` : 生成されたロックファイル
    4. `node_modules` : 生成されたパッケージ用ディレクトリ
- Deno
    1. `.textlintrc` : 各種設定
    2. `textlint.ts` : 読み込むパッケージの指定、textlint実行

CLIでの実行が成功して`.textlintrc`以外不要になればとてもクールでしたが、そうはいかず4ファイルと2ファイルという結果になりました。まあ、重いことで有名な`node_modules`が減るので、少しは嬉しいんじゃないでしょうか。

# ルールを追加してみる
前項では最低限の状態でtextlintを実行しました。実際の利用では、さらに多数のルールを読み込んで運用することになります。
`preset-ja-technical-writing`に含まれていない`textlint-rule-ng-word`を追加してみましょう。

```diff:textlint.ts
@@ -1,5 +1,6 @@
 import { TextLintEngine } from "npm:textlint";
 import _ from "npm:textlint-rule-preset-ja-technical-writing";
+import _ from "npm:textlint-rule-ng-word";
 const engine = new TextLintEngine();
 const results = await engine.executeOnFiles(Deno.args);
 if (engine.isErrorResults(results)) console.log(engine.formatResults(results));
```

```diff:.textlintrc
@@ -1,5 +1,6 @@
 {
   "rules": {
+    "ng-word": { "words": ["猫"] },
     "preset-ja-technical-writing": true
   }
 }
```

2行追加して、再度`deno run`すればルールを取得して実行してくれます。
Node.jsの場合は`npm install`でパッケージを追加し、`.textlintrc`に同様に追記です。ここの手間はあんまり変わりませんね。

![](/images/deno-textlint/success2.png)
*1つ目のエラーが新たに追加したルールのもの*

# プラグインを使ってみる
textlintは、プラグインによって様々な拡張が可能です。ここでは、一例としてLaTeXファイルを読み込めるようにする`textlint-plugin-latex2e`を追加してみましょう。

```diff:textlint.ts
@@ -1,6 +1,7 @@
 import { TextLintEngine } from "npm:textlint";
 import _ from "npm:textlint-rule-preset-ja-technical-writing";
 import _ from "npm:textlint-rule-ng-word";
+import _ from "npm:textlint-plugin-latex2e";
 const engine = new TextLintEngine();
 const results = await engine.executeOnFiles(Deno.args);
 if (engine.isErrorResults(results)) console.log(engine.formatResults(results));
```

```diff:.textlintrc
@@ -1,4 +1,5 @@
 {
+  "plugins": ["latex2e"],
   "rules": {
     "ng-word": { "words": ["猫"] },
     "preset-ja-technical-writing": true
```

```tex:test.tex
吾輩はは\textgt{猫}$e^{i\pi}+1=0$である。名前はまだないんです
```

`$ deno run --allow-env --allow-read --allow-sys textlint.ts test.tex test.md`のように、ファイル形式を混ぜて与えてみます。

![](/images/deno-textlint/success-plugin.png)

問題なく実行できています！

# Github ActionsでCIしてみる
CI/CDカレンダーの参加記事なので、最後はGitHub ActionsでCIして終わります。workflowでは、Denoをセットアップしてtextlintを実行するだけです。

ファイル名ベタ打ちはよろしくないため、findコマンドで.mdと.texのファイルを見つけてxargs経由で渡すようにしてみました。

```yml:.github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  textlint:
    name: Check by textlint
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: denoland/setup-deno@v1
      - name: Run textlint
        run: find . -type f -name '*.md' -or -name '*.tex' | xargs deno run --allow-env --allow-read --allow-sys textlint.ts
```

また、エラーが出たらきちんとCIが失敗するように、終了コードを指定しておきます。

```diff:textlint.ts
@@ -4,4 +4,7 @@ import _ from "npm:textlint-rule-ng-word";
 import _ from "npm:textlint-plugin-latex2e";
 const engine = new TextLintEngine();
 const results = await engine.executeOnFiles(Deno.args);
-if (engine.isErrorResults(results)) console.log(engine.formatResults(results));
+if (engine.isErrorResults(results)) {
+  console.error(engine.formatResults(results));
+  Deno.exit(1);
+}
```

![](/images/deno-textlint/actions-result.png)
*GitHub Actionsの実行結果*
