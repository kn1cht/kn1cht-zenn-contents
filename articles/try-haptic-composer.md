---
title: "マルチプラットフォーム対応の触覚設計ツールHaptic Composerを触る"
emoji: "👆"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - haptics
  - vr
published: true
---

:::message
本記事は、[Human-Computer Interaction (HCI) Advent Calendar 2022](https://adventar.org/calendars/7901) 10日目の記事です。昨日の記事は、まだ枠が空いていますね。後からでもいいのでネタがある方の参加をお待ちしています。明日11日の記事は、Seitaro Kanekoさんの[手触りの物理的な測り方とHCIへの活かし方](https://gitogitogissyu.hatenablog.com/entry/2022/12/11/224505)です。
:::

今月始め、バーチャルリアリティを扱うニュースサイトMogura VR Newsに「**ゲーミングデバイスのRazer、デバイス横断で“触覚”を設定できる開発ツールをリリース VRも対応**」という記事が出ていました。

@[tweet](https://twitter.com/MoguraVR/status/1598143597047648261)

記事によると、これはRazer子会社のInterhaptics社によるHaptic Composerというソフトウェアで、デバイスを横断して触覚刺激を扱えることを目指しているそうです。視覚（映像）や聴覚（音響）と違って、触覚コンテンツの編集ソフトウェアは研究段階のものが多く、**一般人でも使える形で配布されるものはたいへん珍しい**です。

![](/images/hapticcomposer/editor-ui.png)
*Haptic Composerの画面*

本記事では、無料配布されているHaptic Composerを導入し、どのような触覚刺激を作れるのか試してみた結果をご報告します。

# 触覚コンテンツとデバイスの現状
実際にソフトを使う前に、触覚ソフトウェアの現状と標準化の重要性について簡単に触れておきます。

我々が映像や音を再生するとき、デバイスごとのデータフォーマットの違いや伝送方式について意識することはあまりないはずです。これは、ある程度統一された規格が存在し、各メーカーがそれに準拠した機器を製造しているためです。

触覚についてはどうでしょうか。市販される触覚デバイスはまだ黎明期といえ、**メーカーごとにソフトウェアを開発**しています。
そのため、あるコンテンツを触覚に対応させようとすると、2022年現在はデバイスそれぞれの開発キット（SDK）をコンテンツ側が個別に導入する方法が一般的です。

これを簡単にイラスト化すると次のようになります。この状況では、例えば「デバイス1」を買った消費者は「コンテンツA・B」を触覚つきで体験できますが、「コンテンツC・D・E」ではSDKに対応しないため触覚刺激がありません。また、「デバイス2」を開発するメーカーは、「コンテンツB・C・D」のユーザには触覚刺激を体験してもらえる一方、SDK対応のない「コンテンツA・E」のユーザにはアピールできません。

![](/images/hapticcomposer/content-and-device-1.png)
*触覚コンテンツが各デバイスに個別に対応する場合*

もし、**デバイスを横断して使える標準化したプラットフォームやSDKができ、それを全員が使う**とどうなるでしょうか？
消費者目線では、どのデバイスを購入しても楽しめるコンテンツは変わりません。また、デバイスメーカーやコンテンツ製作者は、共通のSDKにさえ対応すればエコシステム全体に参加できます。

![](/images/hapticcomposer/content-and-device-2.png)
*標準化したプラットフォームとSDKが普及した場合*

触覚コンテンツの標準化がいかに大切かお分かり頂けたと思います。触覚の標準化については国際電気標準会議（IEC）でも議論が始まっているものの、規格として完成するまでにはまだまだ時間がかかるでしょう。

https://www.iec.ch/blog/new-iec-doc-haptic-technology

そんな中、（まだ対応デバイスは限られるものの）共通のプラットフォームで複数デバイスに対応できるInterhapticsのソフトウェアが出てきたため、筆者は非常に期待しています。

# インストール
ニュース記事では「Razerが～」と書いてありますが、実際にHaptic Composerを配布しているのはInterhaptics社のウェブサイトです。

![](/images/hapticcomposer/interhaptics.png)

https://www.interhaptics.com

右上のボタンからダウンロードページに行き、Haptic ComposerのDownloadボタンを押します。
なお、2022年12月の時点で**対応OSはWindowsのみ**なのでご注意ください。

`HapticComposer_Setupx64.exe`が手に入るので、セットアップウィザードを進めます。

起動すると、**Razer ID**でログインを求められます。Razer IDがない方は作成しましょう。Razer製品を購入して製品登録したことがある方は、既にIDをお持ちかもしれません。

![](/images/hapticcomposer/razer-login.png)
*数少ないRazer要素*

# 振動刺激（Vibration）の編集
公式Documentationも親切なので併せてご覧ください。

https://www.interhaptics.com/blog/documentation/haptic-composer/haptic-composition/

新規作成するか、既存のファイルを開くか聞かれるのでNew Haptic Materialを選択し、名前などを設定するとまっさらな初期画面に移ります。右上のPerceptionsから**Texture**（テクスチャ、表面の質感）・**Stiffness**（固さ）・**Vibration**（振動）を選べるようです。

![](/images/hapticcomposer/first-ui.png)

ひとまずVibrationを選んで先に進みます。Haptic Composerには、**オーディオファイル（WAV）から自動的に触覚刺激を抽出**してくれる機能があります。

Import Audioボタンから、WAVファイルを読み込みます。今回は、公式DownloadページにあるサンプルからShotgunを選んでみました。

https://github.com/Interhaptics/hapsSamples

`Shotgun.wav`の下に3つ並んだボタンの一番右を押すと、**TransientとMelody**の2つが生成されました。前者は衝撃感を表す瞬間的な振動、後者は連続的な振動を受け持っているようです。

![](/images/hapticcomposer/shotgun-auto.png)

Melodyの枠内をダブルクリックすると、さらに詳しい設定画面が表れます。
Transientについては振幅を、Melodyについては振幅と周波数を自由にマウスドラッグで変えられます。

![](/images/hapticcomposer/editor-ui.png)

# 触覚刺激のテスト（Interhaptics Player）
触覚刺激のよさは触らないと分かりません。触って確かめながら編集を進める必要があります。

触覚のテスト用ツールとして、Interhapticsでは**Interhaptics Playerというスマホアプリ**を配布しています。Haptic Composerと連携して触覚刺激を簡単に試せる方法は現状これだけなので、同時にインストールしておくのがいいでしょう。

https://www.interhaptics.com/download/

ダウンロードページをスマホで開き、Testing Appの項目でiOSならTestFlight、AndroidならGoogle Playボタンを押します。iOSの方はTestFlightアプリの導入も必要です。

![](/images/hapticcomposer/TestFlight.png =250x)
*iOSの場合のインストール画面*

アプリを開くと、Haptic Composerと同様にRazer IDでログインを求められます。Composerとの連携に必要なので、同じRazer IDを使うようにしましょう。

最初の画面には、注意書きとして**①ミュートを解除する ②ボリュームを最大にする ③スマホケースを外す** の3点が書かれています。実際にボリュームを最大にすると効果音が大音量で流れるので、周りに人がいる環境で試す方は音量調節に気をつけましょう。

![](/images/hapticcomposer/player-ui.png)

PlayerのAudioタブには、多数のサンプル触覚刺激が掲載されています。これを試すだけでも小一時間遊べると思います。
iPhoneの場合はホームボタンの近くに振動部品が入っているので、**ホームボタンを触りながら体験**するとしっかり振動を感じられます。

@[tweet](https://twitter.com/kn1cht/status/1601237677856415744?conversation=none)

自分で作った振動を試したい場合は、Haptic Composerの画面下にあるSend to Playerボタンを押しましょう。
Playerアプリが起動していれば、SUCCESSFULLY SENT!とのメッセージが出てくるはずです。このとき、データはRazer IDを通じて受け渡されるので、**ComposerとPlayerを直接連携させる必要はありません**。楽でいいですね。

![](/images/hapticcomposer/sent-success.png =400x)

Player側ではTestタブを開き、DOWNLOAD MATERIALボタンを押すと一番最後にアップロードした刺激を読み込みます。▶︎ボタンで再生します。

![](/images/hapticcomposer/test-by-player.png =250x)

ご覧のとおり、同じ画面が上下2つに分かれてついており、2種類の刺激を触り比べながら編集を進められるように工夫されています。

# 触覚刺激ファイルの保存（hapsファイル）
満足いく振動ができたら、Ctrl+Sで保存しましょう。このとき保存されるのは、`.haps`という拡張子のファイルで、その実体は**触覚刺激の情報が書き込まれたJSON**です。
サンプルがhapsSamplesリポジトリにあるので構造を見てみます。

https://github.com/Interhaptics/hapsSamples/blob/master/HapticMaterials/Body%20Hit.haps

![](/images/hapticcomposer/haps-file.png)

メタデータに加えて、`m_melodies`リストに振動刺激の情報が格納されています。たとえば、`m_melodies`の2つ目の要素を見てみると、振幅変調や周波数変調の指令が時系列順に書かれているのが分かります。

このhapsファイルを受け渡すことで、Interhapticsではマルチプラットフォームでの触覚コンテンツ対応を実現しているようです。hapsファイルの仕様についてさらに詳細に知りたい方はドキュメントをご覧ください。

https://www.interhaptics.com/blog/documentation/haptic-composer/haptic-materials/

# テクスチャと固さの編集・テスト
Haptic Composerで作成できる知覚には、振動のほかにTexture（テクスチャ）とStiffness（固さ）があります。振動に比べるとサンプルも少なくまだまだこれからという印象ですが、簡単に試したので記載しておきます。

テクスチャは、手を動かした距離に応じて出力される振動で表現されます。Gratingと呼ばれる、一定周期で振動する刺激がサンプルとして提供されていました。
Playerにアップロードすると、Texturesタブで指を動かすことでテクスチャの触感を体験できます。

![](/images/hapticcomposer/texture-edit.png)
*左：テクスチャの編集画面 / 右：テクスチャのテスト再生画面*

固さは、PlayStation 5のコントローラのトリガー部分に実装されている触覚刺激です。スマホの振動では固さそのものを出力できないため、Interhaptics Playerでは振動の強度で代替的に提示されます。
Composerでは画像処理ソフトのトーンカーブのようなUIが表示され、距離と固さの関係を編集できます。Playerでは、スライダーを動かすことで振動の強さが変わる様子を体験できました。


![](/images/hapticcomposer/stiffness-edit.png)
*左：固さの編集画面 / 右：固さのテスト再生画面*

# デバイス対応
Haptic Composerで編集した触覚刺激は、InterhapticsのSDKを介して各種デバイスに使用できます。2022年12月の時点での対応プラットフォームは[**iPhone、Androidスマートフォン、Meta Questシリーズ、PlayStation 5コントローラ**](https://www.interhaptics.com/blog/documentation/sdk/supported-platforms/)とのことです。

https://www.interhaptics.com/blog/documentation/sdk/supported-platforms/

これらのデバイスで実際に使用するには、それぞれのためのSDKをUnityに導入して開発しなければなりません。**既存のVRコンテンツなどでいきなり使えるというわけではない**ので注意が必要です。今後対応コンテンツが出てくることに期待したいですね。

@[youtube](vJTBvT0E_oE)

# むすび
簡単ですが、触覚デザインツールHaptic ComposerとテストアプリInterhaptics Playerを使用した結果をレポートしました。対応デバイスは今のところメジャーなスマートフォン、ゲームコントローラ、VRヘッドセットのみですが、今後広がっていくと触覚コンテンツの標準化につながると思うので楽しみです。

なお、本ソフトウェアを試してみて質問やフィードバックがある場合は、Interhaptics社のDiscordで気軽に中の人と交流できます。Webサイト下部のDiscordリンクからjoinできます。

https://www.interhaptics.com/
