---
title: "技術記事にGoogle Mapsを埋め込む方法"
emoji: "🗺"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - OSINT
  - CTF
  - googlemap
published: true
---

:::message
本記事は、[CTF Advent Calendar 2022](https://adventar.org/calendars/7550) 2日目の記事です。
昨日の記事は、xrekkusuさんの「[CTFプレイヤーにアンケート取ってなんかやりたくない？](https://forms.gle/XXtrwJ6AfC7WycjG9)」でした。CTFをやっている人向けのアンケートフォームになっているのでぜひ奮って回答しましょう。明日は、ふぁぼんさんの「[TSG LIVE! 9 ライブCTF参加記](https://yuyusuki.hatenablog.com/entry/2022/12/03/010000)」です。
:::

# TL;DR
- ZennやQiitaなどの技術記事投稿サイトでは、2022年12月2日現在**Google Mapsを直接埋め込めません**
- しかしCodePenの埋め込みは許可されているため、CodePenに所望のiframeを埋め込み、さらにそれを記事に埋め込むことで、なんとか埋め込み表示が可能です

@[codepen](https://codepen.io/kn1cht/pen/YzvJBGd)
@[codepen](https://codepen.io/kn1cht/pen/RwJevbq)

# Write-upに地図を入れたい
先日OSINTのCTFのWriteupをZennに書いていて、**地図やGoogle ストリートビュー**を記事に埋め込みたくなりました。OSINT（Open Source Intelligence）とは、公開された情報を集めて場所や人物に関する情報を突き止める活動のことです。CTF大会では、実際の写真や文書などをヒントに情報を引き出す問題が出題されます。

OSINTのCTFがどんなものか知りたい方は、meowさんのスライドが分かりやすいのでそちらをご覧ください。

@[speakerdeck](e039246c6200450b8e00eb6b89478b01)

さて、筆者は先日**Open xINT CTF 2022**というCTFに出場しました。競技を終えた後にチームとしてWriteupを書いてみると、場所を特定する問題の多くで**地図やストリートビューを参照**する必要がありました。

https://zenn.dev/kn1cht/articles/open-xint-2022-40548f

こうした場合に、多くのWriteupでは**単にGoogle Mapsのスクリーンショットを撮影**して記事を投稿していることでしょう。
ただし、筆者は2つの理由からスクリーンショットではなくiframeタグによる埋め込みを利用したいと思いました。

## Google Mapsの利用ガイドラインに違反しそう
Google MapsとGoogle Earthには[利用ガイドラインがあり、日本語でも専用ページが公開されています](https://www.google.com/intl/ja_ALL/permissions/geoguidelines/)。
このページで「ウェブやアプリケーションでの使用」の項目を見てみると、地図やストリートビューについては**埋め込み機能を使用するように**と描かれています。Google Earthについては、権利帰属を表示すれば掲載可となっています。

まあ現実にはブログでもSNSでもGoogle Mapsのスクリーンショットが貼られているのを日々見ますが、Google先生がいちいち怒らないだけで、厳密にはガイドラインに沿わない利用ということになります。
Google先生としては「Webではiframe埋め込みができるんだから、それを使ってよ」という考えだと思いますし、それは確かに正論なので、**なるべく公式の埋め込み機能を使いたい**という気持ちです。

なお、印刷物での利用に関してはまた異なるガイドラインがあります。ただ、国土地理院の[地理院地図](https://maps.gsi.go.jp/)や[OpenStreetMap](https://www.openstreetmap.org/)のほうがより自由な利用が認められているので、同人誌など印刷物で使う場合は活用するとよいでしょう。

https://muramototomoya.hatenablog.com/entry/20180623/1529717295

## 単なる画像よりも高機能
Google Mapsのiframe埋め込みでは、閲覧者が**自由に拡大・縮小・移動**して地図やストリートビューを操作できます。また、Google Mapsのサイトに遷移するのもワンクリックです。OSINTのWriteupの場合、同じ大会の競技者が解法を見比べたり答え合わせをしたりするために閲覧する場合も多いので、これは大きなメリットです。

# ZennやQiitaにはGoogle Mapsを直接埋め込めない
試しに、このiframeを埋め込んでみます。Google Mapsの「地図を埋め込む」機能からコピーしたものです（見やすさのために改行を入れました）。

```html
<iframe src="https://www.google.com/maps/embed?pb=!1m10!1m8!1m3!1d10907.667151089936!2d139.79851225471634!3d35.630959655805285!3m2!1i1024!2i768!4f13.1!5e0!3m2!1sja!2sjp!4v1669984523248!5m2!1sja!2sjp"
width="600" height="450" style="border:0;" allowfullscreen=""
loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
```

結果は次のとおりです。

![](/images/insert-googlemaps-to-writeup/zenn-iframe.jpg)
*Zennの場合*

![](/images/insert-googlemaps-to-writeup/qiita-iframe.jpg)
*Qiitaの場合*

だめですね……。ZennではHTMLコードがそのまま表示され、Qiitaでは何も表示されません。
そもそも技術記事投稿サイトで地図を見せたい場面は（OSINT以外で）そんなにないと思われるので、実装されていないことに文句は言えません。

一方、**はてなブログ等のブログサービスでは埋め込みが可能**なので、ブログでWriteupを書く方は活用するといいと思います。

https://sumikuni.hatenablog.com/entry/2020/11/30/111605

# CodePenを使った埋め込み
直接の埋め込みができないことは分かりました。それでもなにか抜け道がないかと探していたところ、こちらの記事がヒントになりました。

https://zenn.dev/crayfisher_zari/articles/4422a7f9ff3ff8

この記事自体は埋め込みしたiframeに対して色味の変化を加えるという趣旨ですが、その中でサンプルを示すために**CodePenの埋め込み機能**が使われています。
CodePenはWebのコードスニペット共有サービスで、実際の表示結果も含めて共有できる特徴があります。上記の記事では、CodePenにiframe埋め込みを貼り付け、さらにその結果を記事中に埋め込んでいるというわけです。

というわけで、CodePenに登録して、PenのHTML欄に先ほどのHTMLを貼り付けて公開してみると……。

![](/images/insert-googlemaps-to-writeup/createpen.jpg)

@[codepen](https://codepen.io/kn1cht/pen/NWzOeJQ)

できました！　Zennの場合、CodePenの埋め込み記法は`@[codepen](https://codepen.io/kn1cht/pen/NWzOeJQ)`のような形式です（URL部分に、自分のPenのURLを入れます）。

Qiitaの場合は、**CodePenのHTML埋め込みコードが必要**になるので公式の手順に従って作業しましょう。

https://qiita.com/Qiita/items/edae7417214c8e957f54

`<p>`タグで`data-default-tab="result"`を指定するとResultタブだけが表示されます。

![](/images/insert-googlemaps-to-writeup/qiita-codepen.jpg)

## サイズの調整
お気づきかもしれませんが、埋め込まれたCodePenの画面を見るとスクロールバーが出て、地図の下部が若干隠れます。

これは、Google Mapsで埋め込み時に中サイズを選択した場合**600px x 450px**のiframeになるのに対し、Zennに埋め込まれた状態での表示領域が**700px x 350 px**程度と縦方向に小さいのが原因です（PCでフルHDモニタに全画面表示した場合。画面サイズの異なる環境ではまた違います）。

従って、iframeタグの`height`属性を330などとしておくと、少なくとも画面サイズの大きい環境なら地図全体が表示されます。

@[codepen](https://codepen.io/kn1cht/pen/YzvJBGd)

# 応用（航空写真、ストリートビューなど）
Google Maps側が対応していれば、様々なコンテンツの埋め込みに応用できます。

- 航空写真（Google Mapsで航空写真表示にしてから埋め込みコードを取得します）
@[codepen](https://codepen.io/kn1cht/pen/QWxZYxd)

- ストリートビュー
@[codepen](https://codepen.io/kn1cht/pen/RwJevbq)

- ストリートビューにユーザが投稿した360度写真
@[codepen](https://codepen.io/kn1cht/pen/KKeGJpz)


# デメリットと既知の問題
## 埋め込み1つごとに新規Penが必要
もしかしたらうまい方法があるかもしれませんが、素朴に1つのPenに1つのiframeを貼るこの方法では**それぞれ個別にCodePenを作らなければなりません**。何十何百と埋め込みたい場合は手作業だとつらい可能性があります。

## 記事に使用中のPenを誤って編集しないよう注意が必要
作成者以外はPenを変更しても上書き保存されないので問題ありませんが、作成者自身が誤って編集してしまうと既存の記事に影響が出てしまいます。気をつけましょう。

## CodePenのUIも出てしまうのがちょっと邪魔
これは仕組み上仕方ないですね……。

## 「拡大地図を表示」からGoogle Mapsに飛ぶとエラーになることがある
こちらは筆者のPCにて確認した問題で、iPhoneでは正常にアプリに飛んだので環境依存かもしれません。埋め込み画面中の「**拡大地図を表示**（View on Google Maps）」リンクをクリックすると、「maps.google.com はブロックされています（ERR_BLOCKED_BY_RESPONSE）」のようなエラーでアクセスできません。

原因は今のところはっきりしませんが、URLを別のタブにコピペして開くか、新しいタブで開く（Ctrl+クリック、マウス中ボタンでクリックなど）ことで回避できます。
