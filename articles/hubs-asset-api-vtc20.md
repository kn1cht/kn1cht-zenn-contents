---
title: "Hubsに貼り付けたメディアを外部から差し替え可能にするWeb API"
emoji: "🐥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - vr
  - hubs
  - firebase
published: false
---

この記事は、[**Hubs Advent Calendar 2020**](https://qiita.com/advent-calendar/2020/hubs) 11日目の記事です（大遅刻すみません）。

https://qiita.com/advent-calendar/2020/hubs

---

# TL;DR
- Hubs上で表示される画像や動画などの**メディアURLを外部から指定できる**APIを作りました
- **Googleスプレッドシートを更新するだけ**で、迅速にメディアの設置・差し替えが完了します
- Google Firebase用のコードを公開しています

[![](https://storage.googleapis.com/zenn-user-upload/n0bt3leahwj58rkjpqiokfemyuiz =350x)](https://github.com/kn1cht/hubs-asset-api-firebase)

# はじめに

2020年10月、GREE VR Studio Laboratory主催のVRハッカソン「[**VTech Challenge 2020**（VTC20）](https://vr.gree.net/lab/vtc/vtc20/)」が開催され、ソーシャルVR「[Hubs](http://hubs.mozilla.com/)」に関する作品や知見が披露されました。
筆者は「研究室の成果を広く伝えるバーチャル展示室」というテーマで発表し、幅広い層の方が参加するバーチャル展示という目的に沿った工夫をお話させていただきました。

![](https://storage.googleapis.com/zenn-user-upload/mal9ckv0gxd8jsd33m43prybkuwq =550x)
*Hubsに設置したスライド画像を前に議論している様子*

Hubsが画像や動画、スライドを多用する発表に向いている理由の1つに、**ドラッグ&ドロップで簡単にVR空間へファイルを取り込める**ことがあります。
しかし、研究室の学生全員など多くの人からの資料を一斉に取り込むような状況では、**3D空間での配置作業の手間**が無視できません（メディアを吸着するMedia Frameが実装されて楽にはなりましたが）。VRイベント当日より前に十分余裕を持って配置作業をできればよいですが、**開始ギリギリに提出されたり、内容が急遽差し替えになったり**しても迅速に資料をアップデートできるとスムーズです。

そこで、HubsからのリクエストをいったんダミーのURLで受け取り、**Googleスプレッドシートに記載されたファイルのURLへリダイレクトするAPI**「[hubs-asset-api](https://github.com/kn1cht/hubs-asset-api-firebase)」を作りました。
hubs-asset-apiはGoogle Firebaseで動いており、ダミーURLにリクエストがあるとスプレッドシートを参照して、ファイルがあるURLへの302リダイレクトを返すようになっています。

![](https://storage.googleapis.com/zenn-user-upload/z56edftau89nghe7xjiujtnsfaee)

hubs-asset-apiにより、以下のことが実現します。

1. Hubsシーン内への**メディアの取り込みがGoogleスプレッドシートの編集だけ**で完了する（作業負担軽減・位置を誤って動かすなどのミス回避）
1. メディアを更新する際も、**スプレッドシートを編集するだけで最新の内容に**置きかわる（メディアの置き直しやSpokeでの再編集不要）
1. Spokeでの**Hubsシーン制作担当**と、メディアファイル準備担当の分業

本記事では、VTC20でお話しきれなかったhubs-asset-apiの中身について解説します。

# 背景
## 各種ソーシャルVRでのメディアの扱い

ソーシャルVRでメディアを取り込む方法は様々です。
**VRChat**では、画像や3Dオブジェクトは**事前にワールドかアバターにUnityで組み込んでおく**のが原則です。例外として、[`VRC_Panorama`](https://docs.vrchat.com/docs/vrc_panorama)などURLを参照できるコンポーネントがあり、[自動更新される掲示物](https://booth.pm/ja/items/2401621)などに応用されています。
**cluster**では、アップロードしておいた**画像・動画・PDFファイルがインベントリに表示**され、イベント会場の画面に出力できます。しかし、ワールド内の好きな場所にメディアを設置するような機能はありません（ワールド自体に組み込むことは可能）。
**Neos VR**は自由度がたいへん高く、**インベントリやPCのディレクトリからメディアファイルを取り出し**、手に持ったり配置したりできます。

## Hubsのメディア表示はWebリンクのプレビュー

Hubsでは、**画像・動画・PDFなどのメディアやWebページへのリンク**を3D空間に取り込めます。ユーザが使えるインベントリはないものの、Neos VRと同様に空間での移動・配置が自由自在にできます。

![](https://storage.googleapis.com/zenn-user-upload/xikh29n3jwyxy1w4a3iu67erqw9x)


あたかもメディアがVRに取り込まれたように見えいますが、HubsはWebベースの技術でできているので、VR内で見えている**画像や動画はそれぞれWebリンク**です。その証拠に、TabキーかSpaceキーホールドでメディアのメニューを開くと、`Open link`ボタンからファイル自体のURLに飛べます。

![](https://storage.googleapis.com/zenn-user-upload/rs7hryn5cd07eadhvfnmdlewwzq1 =500x)
*`Open link`ボタン*

SlackやDiscordなどでも、画像のURLを貼り付けたらプレビューが展開されます。2D画面と3DのVR空間という違いがあるだけで、実態はどちらも**ファイルへのURLを見やすく表示しているだけ**と言えます。

![](https://storage.googleapis.com/zenn-user-upload/zu7ayn47e6uouffdwzlka1gb8snj)
*どちらもWebリンク先のプレビューという点では同じ*

Hubsでは、Hubsサーバでアップロードしたもの以外のURLを貼ってもメディアとして展開されます。これは、**Webで公開されているURLさえあれば、どこにある画像を貼ってもVR内で表示される**ということです。

![](https://storage.googleapis.com/zenn-user-upload/gxtgbb5482dn6norrhm3tzrp0dai)
*github.comでホストされている画像ファイルを貼り付けた様子*

そして、URLのリンク先にリダイレクトがある場合、**Hubsはリダイレクト先まで見に行ってくれます**。hubs-asset-apiは、ダミーのURLからファイルの実体URLにリダイレクトさせることで、外部から読み込まれるメディアを操作できるようにしています。

![](https://storage.googleapis.com/zenn-user-upload/v60y5gd7dtmzfau12xn71ipnwf7t)


# APIの実装

## クラウドサービス選定
hubs-asset-apiに最低限必要な機能は以下の2点です。

1. ダミーURLへのリクエストに**302リダイレクトで実体URLを返す**
1. ダミーURLをもとに、**実体URLをGoogleスプレッドシートから取得する**

後者のGoogleスプレッドシートとの相性を考えて、Googleが提供するクラウドサービスを使うことにしました。Googleのクラウドサービスには無料枠があるので、**小規模の利用なら費用をかけずに維持できる**のも利点でした。

もっとも簡単にWebアプリをデプロイできるGoogleサービスといえば
Google Apps Script（GAS）ですが、[**GASで返せるHTTPステータスは200のみ**](https://issuetracker.google.com/issues/36759757)で、任意のURLへリダイレクトさせるような使い方はできません。そこで、モバイルバックエンド（mBaaS）の[Firebase](https://firebase.google.com/)を選定しました。

![](https://storage.googleapis.com/zenn-user-upload/muotvoi7wtwc34jzrdndorvqw56g =500x)

hubs-asset-apiでは、Firebaseの機能のうち、URLへのアクセスなどのイベントをトリガーに処理を実行する**Cloud Functions**を主に使用します。Cloud Functionsは、従量制課金のBlazeプランでないと使用できないので注意しましょう。従量制と言っても、[かなりゆるい制限の無料枠](https://firebase.google.com/pricing)が設定されているため、個人的に使う程度ではほぼ無料に収まります（まさかの課金に気付けるよう、予算アラートは必ず設定しておきましょう！）。

## 実装解説

https://github.com/kn1cht/hubs-asset-api-firebase で公開しているコードについて説明します。いろいろとファイルがありますが、Firebaseプロジェクトに必要な設定ファイル等がほとんどで、ソースコード自体は`functions/index.js`と`functions/store.js`だけです。

[![](https://storage.googleapis.com/zenn-user-upload/n0bt3leahwj58rkjpqiokfemyuiz =350x)](https://github.com/kn1cht/hubs-asset-api-firebase)

### index.js

ダミーURLへのリクエストを受け取ります。ダミーURLのパスはどんな文字列でもいいのですが、筆者は`/A-01.png`のようなファイル名にしました。

```js:index.js
exports.httpEvent = functions.https.onRequest(async (req, res) => {
  const filename = req.path.split('/')[1];
```

ダミーのファイル名を取得したら、Google Sheets APIでURLが書かれたスプレッドシートを読みに行きます……と言いたいところですが、[**Sheets APIにはアクセス頻度のリミットがある**](https://developers.google.com/sheets/api/limits)ため1秒に何度もアクセスするわけにはいきません。そこで、Firebaseで提供されているNoSQLの**Cloud Firestore**にシートの内容をキャッシュするようにしました。

```js:index.js
  const sheetData = await store.getDocInCollection('sheet', 0);
  const updatedMillis = sheetData.updatedAt ? sheetData.updatedAt.toMillis() : 0;
  const elpsedTimeSec = (new Date().getTime() -  updatedMillis) / 1000;

  let sheetVals = JSON.parse(sheetData.vals || '{}');
```

その上で、アクセス時点で**Firestoreの最終更新から60秒**過ぎている場合はSheets APIを使ってスプレッドシートの中身を取得します。当然Firestoreの方も更新します。
`getSpreadSheetValsFromApi()`関数はSheets APIを使っているだけなので省略します。

```js:index.js
  if(elpsedTimeSec >= 60 || Object.keys(sheetVals).length === 0) {
    sheetVals = await getSpreadSheetValsFromApi();
    await store.setDocInCollection('sheet', 0, {
      updatedAt: firestore.FieldValue.serverTimestamp(),
      vals: JSON.stringify(sheetVals)
    });
  }
```

最後にダミーURLの列から該当する行を検索し、実体URLの列からファイルが置かれているURLを取り出して、302リダイレクトとして返します。「ダミーURLの列」「実体URLの列」が左から何番目なのかは、`functions/config/default.yaml`で設定できます。

```js:index.js
  const urlIndex = sheetVals[config.fileColumnNo].indexOf(filename);
  res.redirect(302, sheetVals[config.urlColumnNo][urlIndex]);
});
```

### store.js

Cloud Firestoreを読み書きするための実装です。特に変わったことはしていないので、コードは折りたたんでいます。
Cloud Functionsでは、`firebase-admin`パッケージを読み込むとFirestoreへ簡単にアクセスできて楽です。

:::details store.js

```js:store.js
'use strict';

const admin = require('firebase-admin');
admin.initializeApp();

class Store {
  constructor() {
    this.db = admin.firestore();
  }

  async getDocInCollection(cName, dName) {
    const doc = await this.db.doc(`${cName}/${dName}`)
      .get().catch((err) => console.error(err));
    return (doc.exists ? doc.data() : {});
  }

  async setDocInCollection(cName, dName, data) {
    await this.db.doc(`${cName}/${dName}`)
      .set(data).catch((err) => {
        console.error(err);
        return false;
      });
    return true;
  }
}

module.exports = Store;
```

:::

## デプロイ

（[README](https://github.com/kn1cht/hubs-asset-api-firebase/blob/master/README.md)に詳しい手順を記載しています）

Firebaseプロジェクトを作成してCLIでログインし、URL編集用のGoogleスプレッドシートを作成しておきます。設定ファイルにスプレッドシートのIDやシート名、列番号を書き込みます。

```bash
$ cp functions/config/example.yaml functions/config/default.yaml
$ vi functions/config/default.yaml
```

[GCP Console](https://console.cloud.google.com/apis/library/sheets.googleapis.com)でGoogle Sheets APIを有効化します。

![](https://storage.googleapis.com/zenn-user-upload/epwnyygdjfc713s91s6zka89htal =500x)

Firebaseプロジェクトをデプロイしたら完了です。ダミーのURLへブラウザ等でアクセスして、きちんとリダイレクトされることを確認しましょう。

```bash
$ firebase deploy
```

