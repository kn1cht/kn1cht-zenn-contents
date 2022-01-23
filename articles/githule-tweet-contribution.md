---
title: "WordleがGitHubの草に見えるのでGitHubの草をWordleっぽくツイートできるようにした"
emoji: "🟩"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - クソアプリ
  - wordle
  - nextjs
  - vercel
published: true
---

# TL;DR
- 最近TLでWordleという単語当てゲームが流行っており、結果のツイートがたくさん流れてきます
- ただ、知らない側からするとGitHubのContribution graphs（通称：草）にしか見えません
- そこで、逆にGitHubの草をWordleっぽくツイートできるWebアプリ"GitHule"を作りました

https://githule.vercel.app/

![](/images/githule-tweet-contribution/ogp.png)

リポジトリはこちらです。

https://github.com/kn1cht/githule

# 経緯
## Wordle
最近Twitterを見ていて、四角い絵文字を敷き詰めた謎のツイートが流れてきたことはないでしょうか。

https://twitter.com/famitsu/status/1479226270755934210

ツイートに入っているWordleというキーワードで少し調べれば分かる通り、これは2021年秋から公開され数ヶ月で大人気となった **単語当てゲーム"Wordle"** の結果ツイートです。Wordleでは5文字の単語が毎日出題され、正解の単語にある文字を当てると出てくる緑や黄色の四角を見ながら**正解に近づく様子をTwitterで共有**できます。これがTwitterに流れる謎の絵文字の正体でした。

Wordleの人気はうなぎのぼりで、WORDLEjaやLetterleといった派生Webゲームも多数生まれています。

https://aseruneko.github.io/WORDLEja/

https://edjefferson.com/letterle/

## Wordleの結果がGitHubの草に見える
ただ、一部のソフトウェアエンジニア諸氏にとってはこれらの絵文字が別のものに見えてくることでしょう。
そう、GitHubの**Contribution graphs**（通称：草）です。

![](/images/githule-tweet-contribution/contribution-graphs.png)
*Contribution graphs*

GitHubでコミットやPull request、レビューといった行動をするとカレンダーに緑のマークが付くこの仕組みですが、並んだ四角がところどころ緑になっているところが確かにWordleの結果と似ています。

[Twitterで"Wordle GitHub"と検索](https://twitter.com/search?q=Wordle%20Github)してみても、同じように思った人が多いのが分かります。

https://twitter.com/mattn_jp/status/1483636275748143104

というわけで、~~オタクはすぐ逆張りするので~~GitHubのContributionsをWordleっぽくツイートできるWebアプリを作ってTLをさらに混乱させることにしました。

https://twitter.com/kn1cht/status/1484190266265276422

## GitHule
https://twitter.com/kn1cht/status/1484509378682052618

思いついた当日中に、最小限の機能で最初のバージョンを公開できました。
ネーミングはWordleのパクリで"GitHu + le"です。同じ文字数にするなら"GitHle"もありですが、どう発音すればいいのかわからないのでuも入れています。

UIはシンプルで**GitHubのユーザーネームを入れてDraw**を押すだけですが、ページ上中央にタイトルが全部大文字で出てるあたりはWordle本家へ寄せたレイアウトにしています。

https://twitter.com/kn1cht/status/1484819999927816193

2022/1/23現在のバージョンでは、データの読み込み後に**緑と黄色の境目となるコミット数や、グラフの幅と高さを変えられる**オプションも付きました。
これは、人によっては色の比率がおかしかったり、最近のコミットが少なく寂しい絵面になるといった問題を減らすためです。

# 技術選定
## Next.js & Vercel
一発ネタなのでなるべく時間をかけずに先人の知恵を流用していきたいところです。[クソアプリ Advent Calendar 2021](https://qiita.com/advent-calendar/2021/kuso-app)を軽く眺めると、Next.jsで開発してVercelにデプロイという構成を採用している人が体感で多かったため、同様の構成で行くことにしました。

特に、@nabettu氏の「[Twitterに誰でもドット絵を投下できるサービスを作ったよ](https://qiita.com/nabettu/items/fa79c73aca3c504ec016)」が「四角の絵文字を画面に並べる」「結果を絵文字でツイートする」という点で似た機能を備えていたため、とても参考になりました。

https://qiita.com/nabettu/items/fa79c73aca3c504ec016

## GitHubの草をWebアプリから取得する
任意のユーザのGitHub Contributions（以下：草）を取得できなければこのWebアプリは成立しません。調べた限り、草を取得するには以下のような手法があります。

1. GitHubのプロフィールページをスクレイピングする
    - プロフィールに表示されるSVG画像を取ってきて加工する方法
    - 例：[Githubの草を配列のデータとして取得する．](https://taroosg.io/how-to-get-github-grass-as-array)
2. GitHubのGraphQL APIで取得する
    - REST APIには該当する方法がなく、GraphQLでないとダメのようです
    - 例：[GitHubの草の情報をAPIで取得する方法](https://zenn.dev/yuichkun/articles/b207651f5654b0)
3. 草の情報を取得してくれる既存のWebアプリのAPIを使う
    - [GitHub Skyline](https://skyline.github.com/)や[GitHub Contribution Chart Generator](https://github-contributions.vercel.app/)といった草可視化系サービスの裏にあるJSONを取ってくる方法
    - 例：[Get GitHub User Contributions With GitHub Skyline API | Den Delimarsky](https://den.dev/blog/get-github-contributions-api/)

APIがあれば使いたいな～というのが最初の思いでした。しかし、2のGraphQL APIを利用するには**GitHubの個人アクセストークンが必要**で、不特定多数に使ってもらうWebアプリで積極的に使う気にはなれませんでした（草は公開情報なのだし、わざわざ認証する必要なくない？という気持ちもありました）。

3のように他のWebアプリからデータを貰う方法も検討したものの、CORSがあってフロントエンドだけでは取得が難しい上、正式に公開されたAPIではないものに依存するのはお行儀がよろしくないので諦めました。

そういうわけで「GitHubプロフィールのSVG画像から情報を取得する」方法で作ることにしました。ただ、[GitHub Contribution Chart Generatorのコード](https://github.com/sallar/github-contributions-chart)を眺めていると、まさに**GitHubプロフィールをcheerioでスクレイピングし、JSONに加工してフロントエンドに返す**ところまで実装されていました。フレームワークもNext.jsが使われていたので、これをforkしてAPI部分の実装をまるごとお借りしました。

# 実装

https://github.com/kn1cht/githule

## フロントエンド
GitHub Contribution Chart Generatorの`src/pages/index.js`をGitHule用のものに置き換えました。時間をかけずにデザインを整えるため、CSSフレームワークの[bulma](https://bulma.io/)を各所で使っています。

## バックエンド
Contributionsを取得する点は前述のようにGitHub Contribution Chart Generatorほぼそのままです。ただ、元の実装では**指定したユーザの全てのcontributionsを取ってこようとする**ため、長くGitHubで活動してきた人ほど取得に何十秒もかかってしまいます。

GitHuleでは過去140日（幅7x高さ20で表示した場合）のデータがあれば十分なので、過去2年分だけ取得するように変更しました。

```diff:src/utils/api/fetch.js
@@ -78,8 +78,8 @@
    };
  }

- export async function fetchDataForAllYears(username, format) {
-   const years = await fetchYears(username);
+ export async function fetchDataForLastTwoYears(username, format) {
+   const years = (await fetchYears(username)).slice(0, 2);
    return Promise.all(
      years.map((year) => fetchDataForYear(year.href, year.text, format))
    ).then((resp) => {
```

# おわりに
一発ネタアプリかつ草を取得する部分をfork元に頼ったため、かなり短い記事になってしまいました。
公開後Twitterを見ているとちらほらツイート機能を使ってくださった方がいて、ありがたい限りです。ぜひWordleっぽいGitHubの草をTLに放流してフォロワーを混乱させましょう。
