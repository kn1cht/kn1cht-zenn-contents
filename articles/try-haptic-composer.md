---
title: "マルチプラットフォーム対応の触覚設計ツールHaptic Composerを触る"
emoji: "👆"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

今月始め、バーチャルリアリティを扱うニュースサイトMogura VR Newsに「**ゲーミングデバイスのRazer、デバイス横断で“触覚”を設定できる開発ツールをリリース VRも対応**」という記事が出ていました。

@[tweet](https://twitter.com/MoguraVR/status/1598143597047648261)

記事によると、これはRazerに買収されたInterhapticsによるHaptic Composerというソフトウェアで、デバイスを横断して触覚刺激を扱えることを目指しているそうです。視覚（映像）や聴覚（音響）と違って、触覚コンテンツの編集ソフトウェアは研究段階のものが多く、**一般人でも使える形で配布されるものはたいへん珍しい**です。

![](/images/hapticcomposer/editor-ui.png)
*Haptic Composerの画面*

本記事では、無料配布されているHaptic Composerを導入し、どのような触覚刺激を作れるのか試してみた結果をご報告します。

# 触覚コンテンツとデバイスの現状
実際にソフトを使う前に、触覚ソフトウェアの共通化の現状について簡単に触れておきます。

我々が映像や音を再生するとき、デバイスごとのデータフォーマットの違いや伝送方式について意識することはあまりないはずです。これは、ある程度統一された規格が存在し、各メーカーがそれに準拠した機器を製造しているためです。

触覚についてはどうでしょうか。市販される触覚デバイスはまだ黎明期といえ、**メーカーごとにソフトウェアを開発**しています。
そのため、あるコンテンツを触覚に対応させようとすると、使いたいデバイスの開発キット（SDK）をコンテンツ側が個別に導入する方法が2022年現在は一般的です。

これを簡単にイラスト化すると次のようになります。この状況では、例えば「デバイス1」を買った消費者は「コンテンツA・B」を触覚つきで体験できますが、「コンテンツC・D・E」ではSDKに対応しないため触覚刺激がありません。また、「デバイス3」を開発するメーカーは、対応コンテンツが「コンテンツE」しかないため、他のコンテンツのユーザに購入してもらうことが難しくなります。

![](/images/hapticcomposer/content-and-device-1.png)
*触覚コンテンツが各デバイスに個別に対応する場合*

もし、**デバイスを横断して使える標準化したプラットフォームやSDKができ、それを全員が使う**とどうなるでしょうか？
消費者目線では、どのデバイスを購入しても楽しめるコンテンツは変わりません。また、デバイスメーカーやコンテンツ製作者は、共通のSDKにさえ対応すればエコシステム全体に参加できます。

![](/images/hapticcomposer/content-and-device-2.png)
*標準化したプラットフォームとSDKが普及した場合*

触覚コンテンツの標準化がいかに大切かお分かり頂けたと思います。触覚の標準化については国際電気標準会議（IEC）でも議論が始まっているものの、規格として完成するまでにはまだまだ時間がかかるでしょう。

https://www.iec.ch/blog/new-iec-doc-haptic-technology

そんな中、（まだ対応デバイスは限られるものの）共通のプラットフォームで複数デバイスに対応できるInterHapticsのソフトウェアが出てきたため、筆者は非常に期待しています。

# インストール
ニュース記事では「Razerが～」と書いてありますが、実際にHaptic Composerを配布しているのはInterHaptics社のウェブサイトです。

![](/images/hapticcomposer/interhaptics.png)

https://www.interhaptics.com

右上のボタンからダウンロードページに行き、Haptic ComposerのDownloadボタンを押します。
なお、2022年12月の時点で**対応OSはWindowsのみ**なのでご注意ください。

`HapticComposer_Setupx64.exe`が手に入るので、セットアップウィザードを進めます。

起動すると、**Razer ID**でログインを求められます。Razer IDがない方は作成しましょう。Razer製品を購入して製品登録したことがある方は、既にIDをお持ちかもしれません。

![](/images/hapticcomposer/razer-login.png)
*数少ないRazer要素*

# 触覚刺激の編集
公式Documentも親切なので併せてご覧ください。

https://www.interhaptics.com/blog/documentation/haptic-composer/haptic-composition/

新規作成するか、既存のファイルを開くか聞かれるのでNew Haptic Materialを選択し、名前などを設定するとまっさらな初期画面に移ります。右上のPerceptionsから**Texture**（テクスチャ、表面の質感）・**Stiffness**（剛性）・**Vibration**（振動）を選べるようです。

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

触覚のテスト用ツールとして、Interhapticsでは**Interhaptics Playerというスマホアプリ**を配布しています。Haptic Composerと連携して触覚刺激を簡単に試せる方法は現状これだけなので、セットでインストールしておくのがいいでしょう。

https://www.interhaptics.com/download/

ダウンロードページをスマホで開き、Testing Appの項目でiOSならTestFlight、AndroidならGoogle Playボタンを押します。iOSの方はTestFlightアプリの導入も必要です。



![](/images/hapticcomposer/sent-success.png)
2つまで

Ctrl+S

テクスチャ

剛性
