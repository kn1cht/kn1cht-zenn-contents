---
title: "触覚錯覚体験デモを作って分かったWebXRコンテンツ公開のTips"
emoji: "🤚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - vr
  - webxr
  - haptics
published: true
---

:::message
本記事は、[WexXR Advent Calendar 2021](https://adventar.org/calendars/6556) 16日目の記事です。
昨日の記事は、Hironoriさんの「[Webブラウザ上のMMDの世界に飛び込んで一緒に踊る](https://qiita.com/wirsind55/items/0ab5fa86a9019a80c025)」でした。
明日は、きれいなツチノコさんの「初心者がWebXR+Emscriptenでドはまりした話」です。
:::

# WebXR Pseudo-haptics
視覚刺激によって起こる触覚の錯覚Pseudo-hapticsのオンラインデモ「WebXR Pseudo-haptics」を公開しました。
**https://git.io/WEBXRPH** から体験できます！

https://www.youtube.com/watch?v=PnGpjr6gyj0

Pseudo-haptics（疑似触覚）は、**ユーザの入力と視覚的な表示のずれ**によって起こる触覚の錯覚です。物理的な触覚提示デバイスがなくても、重さや摩擦、凹凸などの体験を作り出せる利点があります。
例えば、マウスカーソルの動きが普段より遅い（下図の右側）と、抵抗力が大きくなったかのように感じる現象はpseudo-hapticsの一種です。

![](https://storage.googleapis.com/zenn-user-upload/9be32e07a048-20211215.gif)

WebXR Pseudo-hapticsは、**ブラウザで動作するpseudo-hapticsのデモンストレーション**です。 2Dモニタでの体験に加えて、ヘッドマウントディスプレイ（HMD）を用いてバーチャル空間でもpseudo-hapticsを体験できます。

制作のモチベーションについてさらに詳しいことは「[触覚のオンラインデモを作る試み](https://scrapbox.io/kn1cht/%E8%A7%A6%E8%A6%9A%E3%81%AE%E3%82%AA%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%83%87%E3%83%A2%E3%82%92%E4%BD%9C%E3%82%8B%E8%A9%A6%E3%81%BF)」という記事に別途まとめたので、本記事ではWebXR Pseudo-haptics（以下、WebXRPH）の実装や公開のTipsについてご紹介します。

- VRモード実装編
    - フレームワークの選定
    - 手やつかんだ物体の表示位置をずらす
    - 本物の手（物体）・みかけの手（物体）の表示を切り替える
    - 物体をつかんだ・はなしたことの判定
    - VRモード中は、Unity内のマウスカーソルを非表示にする
    - using WebXRできないときはアセンブリ参照を作成する
- 2Dモード実装編
    - VRの操作とマウス操作の判定はほぼ別々
    - CameraMainの位置は2D表示に合わせて自由にいじってOK
- 公開編
    - GitHub Pagesで公開する
    - なるべく短くて記憶しやすいURLでアクセス可能にする
    - ブラウザでUnityのロードが終わらないとき
    - フルスクリーンボタンを追加する

# VRモード実装編
## フレームワークの選定
WebXRコンテンツを開発するフレームワークやツールは様々あります。WebXRPHでは、ゲームエンジンUnityと、De-Panther氏が開発している**Unity WebXR Export**を使用しました。

https://github.com/De-Panther/unity-webxr-export

別にUnityでなくても作れるはずですが、筆者は大学院のVR研究や趣味の制作でUnityに慣れ親しんでいたため、**過去の経験や資産を活用して迅速に開発できる**ことを重視して選択しました。同様に、元々Unityで書いたコードがあったり、Unityに詳しい人が関わったりする場合には有力な選択肢だと感じます。

![](https://storage.googleapis.com/zenn-user-upload/410c61ad07da-20211216.png)
*Unityで開発中の画面*

Unity WebXR Exportでの開発の基本は、こりん（[@korinVR](https://twitter.com/korinVR)）さんの[Unity + WebXR開発メモ（Oculus Quest、Chrome対応） - フレームシンセシス](https://framesynthesis.jp/tech/unity/webxr/)さえ読んでおけば問題ないです。

## 手やつかんだ物体の表示位置をずらす
Pseudo-hapticsでは、「**ユーザの実際の入力から視覚表示をずらす**」必要があります。例えば、下図では両手を同じだけ動かして箱を持ち上げているのに、左の箱のほうが移動距離が短く表示されます。すると、持ち上げにくい左の箱が重く感じられます。

0.7という数字は倍率で、実際の移動距離を1として0.7しか箱が動かないという意味です。

[![Image from Gyazo](https://i.gyazo.com/1ff2606382661a6061e7ea7745fdb7e6.gif)](https://gyazo.com/1ff2606382661a6061e7ea7745fdb7e6)

しかし、「位置をずらす」こととVRのトラッキングは相性が悪いです。HMDやコントローラーの位置は、常に本当の位置が追跡されて表示位置に上書きされています。スクリプトで直接位置を書き換えようとしてもうまくいきません。

そこで、本物の手・箱の位置を参照して動く「みかけの手」「みかけの箱」を用意しました。本物の方を非表示にしてしまえば、みかけの手と箱だけが残り、体験者には手で箱を持ち上げている様子に見えます。
みかけの手と箱の位置は、箱をつかんだ瞬間の座標を基準座標として、`(本物の座標 - 基準座標) × 倍率`で求まります。

![](https://storage.googleapis.com/zenn-user-upload/2b8711aac739-20211216.png)

みかけの箱にアタッチしているスクリプトを示します。箱を手でつかんでいる間は、本物の箱（`this.trueCube`）の位置と倍率（`this.magnification`）を元に計算した位置へ箱を表示します。

```cs:Assets/Scripts/Weight/PseudoCube.cs
void Update()
{
    this.transform.rotation = this.trueCube.rotation;
    if(this.isGrabbed)
        this.transform.position = this.originPosition + (this.trueCube.position - this.originPosition) * this.magnification * (this.isMouse? this.magnification : 1f);
    else
        this.transform.position = this.trueCube.position;
}
```

## 本物の手（物体）・みかけの手（物体）の表示を切り替える
常にみかけの手を表示させていても体験できますが、みかけの手は動かない3Dモデルなので、コントローラを操作しても指などが動きません。そこで、**Pseudo-hapticsを発生させないときは本物の手を表示させる**ことにしています。

WebXRCameraSetプレハブ内の手のオブジェクト（handL, handR）にアタッチするスクリプトです。つかむ（PickUp）、はなす（Drop）のいずれかが発生したときに、本物の手（`trueHand`）とみかけの手（`pseudoHand`）の表示を入れ替えるようになっています。

```cs:Assets/Scripts/Weight/PseudoCube.cs
void Update()
{
    if (isControllerPickUp(this.controller) && this.trueCube != null)
    {
        this.pseudoHandModel.position = this.transform.position;
        this.pseudoHandModel.rotation = this.transform.rotation;
        this.trueHandRenderer.enabled = false;
        this.pseudoHandRenderer.enabled = true;
        this.pseudoHandModel.SetParent(this.trueCube.pseudoCube.transform);
        this.trueCube.pseudoCube.GrabStart();
    }
    else if (isControllerDrop(this.controller) && this.trueCube != null)
    {
        this.pseudoHandModel.parent = null;
        this.trueHandRenderer.enabled = true;
        this.pseudoHandRenderer.enabled = false;
        this.trueCube.pseudoCube.GrabEnd();
    }
}
```

## 物体をつかんだ・はなしたことの判定
Unity WebXR Exportでは、物体をつかむ・はなすの操作をいくつかのボタンで行なえます。これは、WebXR Interactionsパッケージに入っている[`ControllerInteraction.cs`のコード](https://github.com/De-Panther/unity-webxr-export/blob/22a1ca77c7f40530ece84a84d7c57f5984d854e3/Packages/webxr-interactions/Runtime/Scripts/ControllerInteraction.cs#L117-L129)に書かれています。

```cs:Packages/webxr-interactions/Runtime/Scripts/ControllerInteraction.cs
if (controller.GetButtonDown(WebXRController.ButtonTypes.Trigger)
    || controller.GetButtonDown(WebXRController.ButtonTypes.Grip)
    || controller.GetButtonDown(WebXRController.ButtonTypes.ButtonA))
{
  Pickup();
}

if (controller.GetButtonUp(WebXRController.ButtonTypes.Trigger)
    || controller.GetButtonUp(WebXRController.ButtonTypes.Grip)
    || controller.GetButtonUp(WebXRController.ButtonTypes.ButtonA))
{
  Drop();
}
```

つかむ・はなす操作を判定してなにかに使いたい（WebXRPHの場合は、みかけの箱の表示切り替えや表示位置の制御）場合は、**この条件判定文をそのまま流用してくる**のがよいでしょう。

## VRモード中は、Unity内のマウスカーソルを非表示にする
Unityでは、`Cursor.SetCursor()`でカスタムのマウスカーソルを設定できます。WebXRPHでは、このあと述べる2Dモードのために手の画像をマウスカーソルとして表示します。

ところが、WebGLビルドしてブラウザで実行すると、VRモード中でも**マウスカーソルがゲーム画面に重なっている場合、目の前に2D用のカーソルが表示されてしまう**問題が起きます。Oculus Quest 2でも、タイミング次第でカーソルが置かれた判定になりこの現象が発生します。

これは非常に鬱陶しいので、**VRモード中はUnity内のマウスカーソルを非表示**にするようにしました。VR（AR）モードかどうかは、WebXRCameraSetにアタッチされている`WebXRManager`コンポーネントの`XRState`で分かります。

```cs
using WebXR;
  // ...
  // （クラス内で）
  private WebXRManager webXRManager;
    // ...
    // （void Start()内で）
    this.webXRManager = GameObject.Find("WebXRCameraSet").GetComponent<WebXRManager>();
    // VRモードかどうかの判定
    Debug.Log(webXRManager.XRState == WebXRState.VR);
```

なお、`WebXRManager.XRState`へ実際に値が入るのはビルド済みのアプリをブラウザで動かしているときだけです。Unity Editor上で実行すると、`WebXR.WebXRSubsystem`がnullとなり、XRStateを読み取るときにエラーが起きます。

https://github.com/De-Panther/unity-webxr-export/issues/164

Editorでの開発中ずっとエラーが出るのは困るので、筆者は`XRState`を読み取る部分をtryブロックに入れ、例外をcatchで握りつぶすことにしました。

```cs:Assets/Scripts/Weight/PseudoCursor.cs
void Update()
{
    try
    {
        Cursor.visible = !(isTouching || webXRManager.XRState == WebXRState.VR); // disable cursor in VR mode
    }
    catch
    {
        Cursor.visible = !isTouching;
    }
    // 略
}
```

## using WebXRできないときはアセンブリ参照を作成する
慣れている方なら言うまでもないかもしれませんが、筆者は困ってしまったのでメモします。

WebXRネームスペースのクラス等を使用するために`using WebXR`を書いて「見つかりません」のようなエラーが出るときは、アセンブリ定義への参照（Assembly Definition Reference）をAssetsフォルダ内に設定すれば直ります。

![](https://storage.googleapis.com/zenn-user-upload/8630924bfc21-20211216.png)
*WebXRが見つからないエラー*

Projectウィンドウで「右クリック→Create→Assembly Definition Reference」と進み、インスペクタでAssembly Definition欄へ望みのアセンブリ定義を設定、Applyします。これでWebXRネームスペースが使えるようになるはずです。

![](https://storage.googleapis.com/zenn-user-upload/5c9900247d09-20211216.png)
*アセンブリ定義への参照を設定*

（今になってissueを読み返したらasmrefではなくasmdefがおすすめされていましたが、asmrefでも直ったので結果オーライと思っておきます）

https://github.com/De-Panther/unity-webxr-export/issues/123

# 2Dモード実装編
Pseudo-hapticsは2D状態（VRモードやARモードではない状態）でも体験できます。そこで、WebXRPHにはHMDを所持していなくても遊べる「2Dモード」を付けました。

## VRの操作とマウス操作の判定はほぼ別々
2Dということは、**マウスを動かしたり、ドラッグ・アンド・ドロップしたり**という操作に対応しなければなりません。

普通に物体を掴む程度ならコードを書かなくても可能です。しかし、WebXRPHのように「つかむ・はなす・かざす」といった操作を検出して手を加える場合、操作を検出する方法がVRとマウスで異なります。従って、**判定部分のコードはVRコントローラとは別に書く**ことになります。

判定する動作 | VRコントローラの場合 | マウスの場合
------------|--------------------|---------------
 かざす・手を入れる  | OnTriggerEnter() | OnMouseEnter()
 離れる・手を外に出す| OnTriggerExit()  | OnMouseExit()
 つかむ  | 手のコライダーが接触 + ボタン押す | OnMouseDown()
 はなす | ボタンはなす | OnMouseUp()

また、WebXRPHの場合はVRモードと2Dモードで「手」の表示方法が違ったため、表示部分についても両モードでほぼ別々に実装しました。

## CameraMainの位置は2D表示に合わせて自由にいじってOK
WebXRCameraSetの中には、カメラが5つ含まれています。その中で`CameraMain`が2Dモード中の視点を担当しています。
`CameraMain`の位置を調節しておくと実行時にそのまま表示されます。従って、**2D時に見やすい位置に移動しておく**といいでしょう。VRと2D画面では距離感や視界が異なるので、それぞれ見やすい位置も変わってくることが多いです。

![](https://storage.googleapis.com/zenn-user-upload/e0f6611d0ee6-20211216.png)
*カメラたち*

WebXRPHでは、2Dモード時に奥行き感は不要と判断したので、さらに`CameraMain`のProjectionをPerspectiveからOrthographicに変更しました。

# 公開編
ビルドしたら、いよいよインターネットに公開です。WebXRは、「**ブラウザさえあれば体験できる手軽さ**」が特徴ですが、ユーザ体験がスムーズになるよう工夫をしなければ手軽さが失われかねません。

## GitHub Pagesで公開する
WebXRコンテンツはHTTPS接続で読み込めるウェブサイトに配置する必要があります。どこに置いてもいいと思いますが、WebXRPHでは無料でHTTPS接続のサイトが作れるGitHub Pagesをデプロイ先に選びました。

## なるべく短くて記憶しやすいURLでアクセス可能にする
スタンドアロンHMDの中で圧倒的人気を誇るOculus (Meta) Quest 2は、WebXRの体験に適したデバイスです。ただ、コンテンツにアクセスしてもらうには、**URLをQuestに入力する**必要があります。

Questには現状PCから文字列をコピペするような機能がないので、他のデバイスで取得したURLをQuestで開くには次のような方法が考えられます。

1. Questのキーボードで、1文字1文字入力する
2. TwitterやSlackなどURLを送れるWebサービスにQuestブラウザでもログインしておき、自分へのDMなどでURLを送り込む
3. [Firefox Reality](https://support.mozilla.org/kb/install-firefox-reality)をインストール・ログインして、履歴やブックマークの同期を使う
4. Oculus Developer HubやChromeデベロッパーツールなど開発者向けの手段
    - [Unity + WebXR開発メモの「QuestでブラウザのURLを簡単に開くには」](https://framesynthesis.jp/tech/unity/webxr/#quest%E3%81%A7%E3%83%96%E3%83%A9%E3%82%A6%E3%82%B6%E3%81%AEurl%E3%82%92%E7%B0%A1%E5%8D%98%E3%81%AB%E9%96%8B%E3%81%8F%E3%81%AB%E3%81%AF)に書かれています

1「VRキーボードで打ち込む」は下準備を必要としないものの、20文字、30文字と**長いURLになればなるほど耐えがたい作業**になっていきます。意味のないランダムなURLだと覚えにくいため、HMDとPC画面を何度も見比べながらの入力になるかもしれません。
2、3、4は一度ログイン等の設定を済ませた人なら楽に使えますが、そうではない人には体験までの手間が増え、体験を諦めてしまう可能性が高まります。

そこで、「**なるべく短く、記憶しやすいURL**」でアクセスできるようにしました。10文字～15文字くらいならVRキーボードで入力するのもあまり苦痛ではないですし、ランダムではなく覚えやすい単語にすれば打ち間違いが減ります。
この方法は、超有名なWeb上のVRコミュニケーションサービス[Hubs by Mozilla](https://hubs.mozilla.com/)から着想を得ました。Hubsでは、PCで開いたページをQuestなどスタンドアロンHMDで開くための機能として、短縮URL **hub.link** と4文字のワンタイムコードを利用できます。ユーザがQuestで入力すべきなのは、`hub.link`とワンタイムコードで**合計12文字**です。

![](https://storage.googleapis.com/zenn-user-upload/2cadd48dd924-20211216.png)
*hub.linkのワンタイムコードが発行された様子*

そういうわけで、WebXRPHのURLは `git.io/WEBXRPH`（14文字）としました。

![](https://storage.googleapis.com/zenn-user-upload/db556af92f13-20211216.png)
*短縮URLでアクセスできる*

git.ioは、GitHub内URL専用の短縮URLです。短縮URLは便利な反面、スパムや詐欺でURLを隠すために用いられるなど怪しい印象も付きまとうため、汎用的な短縮サービスではなくGitHubに関係することが明確なgit.ioを選択しました。

- [GitHub 専用 URL 短縮サービス「git.io」 | DevelopersIO](https://dev.classmethod.jp/articles/introduction-git-io-github-url-shorten-service/)

オススメの使い方は、上の記事の最後にあるAPIです。**好きな文字列**（大文字・小文字区別あり）**で短縮URLを作れる**ので、コンテンツのタイトルなどの単語を使って、記憶しやすいURLにできます。

Quest対策以外にも、短いURLなら宣伝で伝えやすかったり、再アクセス時に思い出しやすいなどの利点があります。

## ブラウザでUnityのロードが終わらないとき
書き出したWebGLビルドをブラウザでいざ開くと、**ロードが9割くらいで全然進まなくなってしまう**ことがあります。
これが発生したら、Project Settings → Player → Publishing Settings → Decompression Fallbackにチェックを入れると解消します。

https://qiita.com/aguroshou0413/items/1451a6779a92acb96b78

## フルスクリーンボタンを追加する
Unity WebXR Exportで出力したゲームのHTMLファイルにはフルスクリーンボタンがありません。WebXRPHには2Dモードがあるので、フルスクリーンで体験したいこともあるのではと思い、ボタンを加えました。

フルスクリーンボタンがないといってもHTML上にないだけで、CSSやJavaScriptはそのままですので`id=unity-fullscreen-button`のdivタグを追加するだけでOKです。
WebXRPHはARに対応しないため、ついでにARボタンを削除しました。

```html
       </div>
       <div id="unity-footer">
         <div id="unity-webgl-logo"></div>
+        <div id="unity-fullscreen-button"></div>
         <button id="entervr" value="Enter VR" disabled>VR</button>
-        <button id="enterar" value="Enter AR" disabled>AR</button>
         <div id="unity-webxr-link">Using <a href="https://github.com/De-Panther/unity-webxr-export" target="_blank" title="WebXR Export">WebXR Export</a></div>
         <div id="unity-build-title">webxr-pseudo-haptics</div>
       </div>
```

![](https://storage.googleapis.com/zenn-user-upload/3622538a8503-20211216.png)
*フルスクリーンボタンが表示された*

# おわりに
本記事では、WebXRPHの実装・公開で詰まった点や工夫した点を並べてみました。皆様がWebXRコンテンツを公開される上で参考になるものが1つでもありましたら幸いです。

https://twitter.com/kn1cht/status/1467418646444404742

WebXR Pseudo-hapticsについて、12月18日（土）に開催される[バーチャル学会2021](https://sites.google.com/view/virtualconference-2021)で発表します！
学会ではClusterでの口頭発表とVRChatでのポスター発表があり、触覚錯覚体験としての工夫や、本記事で書いたような技術的な工夫についてお話します。もっと聞いてみたいことがあるという方は、ぜひVR会場へお越しください！
