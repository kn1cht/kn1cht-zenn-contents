---
title: "NeoRoidアバターをHubsでも使ってみた"
emoji: "🧍‍♀️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - blender
  - vr
  - モデリング
  - hubs
  - NeosVR
published: true
---

この記事は、[**NeosVR Advent Calendar 2020**](https://adventar.org/calendars/5277)および[**Hubs Advent Calendar 2020**](https://qiita.com/advent-calendar/2020/hubs) 12日目の記事です。
NeosVR Advent Calendar 2020の昨日の記事は、piacerexさんの[NeosVR＋Elixirで気軽にVR WebSocketプログラミング（VR |> AR投影アプリの裏側）](https://qiita.com/piacerex/items/4d8b23d99434d5a841ca)でした。
Hubs Advent Calendar 2020の昨日の記事は、筆者担当です。鋭意執筆中です……。

-----

## TL;DR
- VRメタバース「Neos VR」の日本語コミュニティは、簡単に軽量アバターを作ってシェアできる「**NeoRoid**」を開発しています
- 最近、NeoRoidアバターをUnityにインポートし、他のVRプラットフォームでも使用できる[NeoRoidHub for Unity](https://neoseast-japan.booth.pm/items/2562276)がリリースされました
- Mozillaが開発するWebVRプラットフォーム「Hubs」にも、**NeoRoidアバターを少ない作業時間で持ち込む**ことができました

@[tweet](https://twitter.com/kn1cht/status/1338143025575260160)

## はじめに

### [Hubs](http://hubs.mozilla.com/)とは
ブラウザで動作する**ソーシャルWebVRプラットフォーム**です。専用ソフトのインストールやアカウント作成が不要なことから、**VRに慣れていない人でも簡単に参加できます**。Mozillaがオープンソースとして開発しており、Hubsをベースとして個人や企業が独自にWebVRサービスを展開できるのもポイントです。

詳細は、Hubs Advent Calendar 1日目「[2020年のHubsふりかえり & Hubs Advent Calendar 2020について](https://note.com/kn1cht/n/nc03eef9587f6)」をご覧ください。

https://note.com/kn1cht/n/nc03eef9587f6


### [Neos VR](https://neos.com/)とは
Solirax Ltdによる**VRメタバース**です。自ら「メタバース」を名乗っているだけあって、Neos VR内だけで**PC上のファイルを取り込んだり、アバター・ワールドを作ったり**と様々なことがVR内で完結します。
さらには、LogiXというプログラミング言語や、NCRという暗号通貨まで実装されており、「世界」を作ろうという気概に溢れたプラットフォームです。

詳細は、NeosVR Advent Calendar 1日目「[マシコの知らないNeosVRの世界](https://note.com/sirojake/n/n2a1ef10a23af)」をご覧ください。

https://note.com/sirojake/n/n2a1ef10a23af

### NeoRoidとは
オレンジさん・もふやんさんが開発された、Neos VR上の**軽量アバターメーカー**です。他のVRサービスであれば別のソフトとして提供されそうなところ、**ワールド内でアバターのデザインから着用までが完結**するのは流石Neos VRですね。

@[tweet](https://twitter.com/mikan3134/status/1309433591059808256)

2020年9月30日には、作成したアバターを共有できる「NeoRoid Hub」が、12月1日には **NeoRoidアバターをUnityにワンクリックでインポートできる「NeoRoidHub for Unity」** がリリースされました。NeoRoidHub for Unityを使えば、VRChatやclusterなど他の著名なVRサービスでもNeoRoidアバターを使えます。

@[youtube](cvxqFEYkF1A)

## NeoRoidアバターをHubsでも使いたい

Twitterなどで何度かNeoRoidアバターを拝見して、「これはHubsで使うのにもピッタリなのでは？」と感じていました。なぜなら、NeoRoidアバターは **「頭・胴体・両手パーツのみ」「ポリゴン・マテリアルが軽量」** というHubsアバターとして望ましい特徴を既に備えているからです。

### 頭・胴体・両手パーツのみ
Hubsのアバターでは、逆運動学（IK）計算が行われません。従って、腕や足の関節を表現できず、VRChatやclusterなどでよく使われる全身（full body）アバターが使えません。

例えば、デフォルトのロボットアバターは首と胴体、そして両手が空中に浮いているデザインです。

![](https://storage.googleapis.com/zenn-user-upload/khw15h19lm2j5l0mctd4q0lwgjv7)

そこで、普段使っているアバターをHubsでも使いたい場合は**腕のメッシュを削ったり、ボーンを丸ごと入れ替えたり**と面倒な工程が必要でした。

https://note.com/7cancer/n/n8dbc31152f89

https://zenn.dev/kn1cht/articles/962fbcd5739e818e95a1

ここでNeoRoidアバターを見てみると、**Hubsアバターのデザインに良く似ている**ことが分かります。メッシュをHubs用に調整する作業が不要になりそうです。

2等身にデフォルメされているので、デフォルトアバターにも違和感なく混ざれます。

![](https://storage.googleapis.com/zenn-user-upload/qi1q3aucudega14nojlbhbe3r342)

### ポリゴン・マテリアルが軽量
NeoRoidアバター「初音ミク」をBlenderで開いた様子です。**ポリゴン数は2100程度、テクスチャは1024px角のもの1枚**で表現されています。

![](https://storage.googleapis.com/zenn-user-upload/c35tq8pw9xlraxieqq1wwqwlr0ap)

Hubsは数万ポリゴンになるような複雑なモデルも表示できますが、可能な限りポリゴンやマテリアルを軽量化することが推奨されています。HubsのDiscordコミュニティでは、開発メンバーが「**アバターのポリゴン数は5000未満、できれば3000程度で、マテリアル数は1～2が望ましい**」と発言されていました。
NeoRoidは、この要件を余裕でクリアしています。VRChatで言うところのExcellent判定です。

## Hubsへ持ち込んだ結果
早速ですが、こちらHubsのアバターアップロード画面です。Blenderで1時間ほどの作業の後、**無事NeoRoidをHubs内で着用できました**。

![](https://storage.googleapis.com/zenn-user-upload/wmmoi6jxrv3bdwdg513mwe6jbnan)

隣にいるのは筆者がCC0アバターをHubs用アバターに改造したものです。NeoRoidは腰ぐらいまでの高さで胴体が終わり、空中に浮いた感じになっています。

![](https://storage.googleapis.com/zenn-user-upload/xp6nagh8649u8hbwcrwe53cqsk0m)

## ソフトウェアごとの手順
今回取った方法では、**Hubsへ持って行く際にボーンをHubs用のものに置き換える作業がある**ため、簡単に即アップロード！とは行きません。ご了承ください。

### Neos VR
NeoRoidクリエーターはNeos VRにありますので、Neos VRが必要……と思いきや、実は**NeoRoid Hubに公開されているアバターをお借りするだけならNeos VRに一度も入らなくても先に進めます**。

※ NeoRoid Hubに登録されたアバターはCC0、パブリック・ドメイン相当です

~~と、これで終わらせたらNeos界隈の方に殴られそうなので~~ 、ちゃんとNeos VRのチュートリアルを受けてNeoRoidクリエーターへ行ってきました。ここでもNeoRoid Hubのアバターを閲覧したり、その場で着替えたりできます。

![](https://storage.googleapis.com/zenn-user-upload/tfyityijeu1kq5wvu867hsejvtie)

自分好みのデザインで作りたい方は、Neos内でこだわりのモデルを作ってみてはいかがでしょうか。

### Unity
最初にゲームエンジンのUnityを経由します。
といっても、[NeoRoidHub for Unity](https://neoseast-japan.booth.pm/items/2562276)をインポートして使うだけで、Unityの知識は特にいりません。

https://neoseast-japan.booth.pm/items/2562276

手順の[解説記事](https://note.com/litalita9764/n/n38baea01990e)も出ていますので、辿っていけば大丈夫です。

https://note.com/litalita9764/n/n38baea01990e

![](https://storage.googleapis.com/zenn-user-upload/9vzkdl5bjrjuo7quniq9c6anaosg)

インポートしたら、VRMやglTF形式を扱えるUnityパッケージ[UniVRM](https://github.com/vrm-c/UniVRM/releases)でエクスポートします。これでUnityの作業は終わりです。

https://github.com/vrm-c/UniVRM/releases

### Blender
今回はボーンを入れ替えるので、ほとんどの作業はBlenderで行いました。
メッシュがそのまま使えるとはいえ、**Blenderでメッシュやボーンを動かしたり、ウェイトを割り当てたり**といった基本操作のスキルが必要です。

ここからの手順を全て丁寧に解説するのはかなり大変なので、筆者の記事「[**Mozilla Hubsで手持ちのアバターを使えるようにする手順**](https://zenn.dev/kn1cht/articles/962fbcd5739e818e95a1)」をご覧ください。Blenderを多少触っておられる方なら乗り越えられると思います（丸投げ）。

https://zenn.dev/kn1cht/articles/962fbcd5739e818e95a1

代わりに、いくつか作業中のスクショをご紹介します。

![](https://storage.googleapis.com/zenn-user-upload/p6vqz5xfkif084rstif2qlcu9xge)
*デフォルトのロボットアバターのモデルに、NeoRoidモデルを位置合わせ*

![](https://storage.googleapis.com/zenn-user-upload/gtut8gpj7reo6vklytlj474zx5rt)
*ボーン（アーマチュア）の位置調整。Neckが頭と胴体の間の空間に来るようにしました*

![](https://storage.googleapis.com/zenn-user-upload/9g9g4vc4fkw8t9g3ltaa3l3zrc26)
*ウェイトの割当て。NeoRoidモデルにもウェイトが付いていますが、頂点グループの名称が違うので注意*

Hubsでは指を動かすこともできるものの、NeoRoidには指がないので対応しないことにしました。以下に、NeoRoidアバターとHubsアバターの頂点グループの対応を示します。

| NeoRoid     | Hubs      |
| ----------- | --------- |
| Head        | Head      |
| Chest       | Spine     |
| Right wrist | RightHand |
| Left wrist  | LeftHand  |
| RightEye（ウェイトの割り当てはない） | RightEye  |
| LeftEye（ウェイトの割り当てはない） | LeftEye   |

こうして見てみると、 **ボーンを総入れ替えせず、リネームだけでもHubsアバター化が可能なのでは？** とも思えますね（終わってから気づく人……）。もし自動化ができたらもっとお手軽になりそうです。

ともあれ、Blenderの工程を終えたらglTFバイナリ（.glb）形式で書き出しせば準備完了です。

### Hubsで動きを確認する

アバターが完成したら、Hubsへアップロードして動きを確認しましょう。Hubsはアカウントがなくても使えますが、**アバターのアップロードにはメール認証が必要**です。

デスクトップモードでは、`i`キーを押すと自分の姿を確認できます。
**VR機器を使わない場合両手は表示されない**ので、両手が見えなくても慌てる必要はありません。

![](https://storage.googleapis.com/zenn-user-upload/82lqr0ymfpoc33r75b42320o3jup)

## おわりに

**HubsでもNeoRoidできました**。

「Hubsはブラウザだし、できることが少ないのでは？」と感じる方は多いかもしれません（実際にアバターの表現力はVRChat等にかないません）。ただ、Webブラウザで動くという無視できないメリットがあります。学会やカンファレンスなど、**参加の障壁を極力なくしたいオンラインイベント**での利用が増えていくことでしょう（先日のXRKaigiでも、バーチャル会場としてHubsが採用されました）。

Hubsを使う必要が生じたとき、**自分好みの外観のアバターを気軽に持ち込める方法**の1つとしてNeoRoidは有効だと感じました。

![](https://storage.googleapis.com/zenn-user-upload/aqsd6snkmrcnsvsktd3kzy0bp0nd)

-----

明日13日のNeosVR Advent Calendar 2020は、Hamadoriさんの「NeosVRでパズルゲーム作ったよ（仮題）」の予定です。

明日13日のHubs Advent Calendar 2020は、まだ担当の方がおられません。14日目はkamimi01さんの「[3D Scene オンラインエディタ - Spoke - Architecture Kit編](https://qiita.com/kamimi01/items/b13fe9c5eca6e2ce907c)」です。

