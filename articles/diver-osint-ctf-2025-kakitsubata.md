---
title: "[KAKITSUBATA] DIVER OSINT CTF 2025 Writeup"
emoji: "🤿"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - OSINT
  - CTF
  - writeup
published: true
---

Team KAKITSUBATAによるDIVER OSINT CTF 2025のWriteupです。

# 開催概要・制度
* 2025/06/07 (土) 12:00 p.m. (JST) \- 06/08 (日) 12:00 p.m. (JST)（24時間）
* Jeopardy方式
* 問題文は日本語と英語
* 問題はwelcomeに正答した時点から、reportカテゴリを除く全てが解放された
* CTFdに1チーム6人まで参加登録し、チーム戦を行った
* 各問題の点数は、参加者全体の正答数が多いほど減少するdynamic方式だった
* Introduction, easy, medium, hardの4つの難易度が用意された。特に、introductionレベルはOSINT CTFの入門になるような問題として作られた
* ルール・規約の確認であるwelcomeを回答したチーム数は668チームだった

# 結果
6名（meow\_noisy, blackwasan, nomizou, kuzushiki, Тамияс, kn1cht）のチーム"KAKITSUBATA"として参加しました。KAKITSUBATAとしてのCTF参加は昨年2024から引き続き2度目です。2日目の4:41（JST）に**記述式問題以外の全問**を解き終え、その後**記述式問題で200点（300点満点）をいただき13443点・総合2位**となりました。

1位のチームは、記述式含めた全問に正答したうえ、記述式で追加点100点を得たそうです。あっぱれ！

![チームの結果。13443点で2位。](/images/diver-osint-ctf-2025-kakitsubata/ctfd.diverctf.org_team_1.png)
![統計データ。提出の正答率は62.5%](/images/diver-osint-ctf-2025-kakitsubata/ctfd.diverctf.org_team_2.png)

# Writeups
本記事はkn1chtのアカウントで公開しますが、各課題ごとに解答したプレイヤーが執筆しました。見出しに問題名、執筆者、点数、解けたチーム数を入れています。複数名で正答に貢献した問題の場合は、複数の名前を書いている場合があります。

見出しに示した点数は、CTF終了後のものです（点数は競技中の解いた人数に合わせて変動しました）。
また、問題サーバーから文章、画像等のデータを引用しています。


# welcome

## welcome [kn1cht] (10pt / 668 solves)

### 問題

あなたのチームメンバー全員がルールと規約に同意できたら、Discordの \#announcement チャンネルからFlagを見つけて入力してください。
正解すると、ほかの問題がアンロックされます！
Flag形式: Diver25{flag}

If all members in your team agree to the rules and terms and conditions, find the flag from \#announcement channel on Discord and enter it here.
You will see the other challenges when you submit the correct flag\!
Flag Format: Diver25{flag}

（CTF全体のルールに関して説明が他にも記載されていたが省略）

### Flag

Discordで開始と同時にFlagがアナウンスされた。

Diver25{We\_understand\_the\_rules\_and\_how\_to\_submit\_flags\!}

# introduction

## bx [nomizou] (100pt / 469 solves)

### 問題

この写真で見える "BX" という看板の座標を答えなさい。
Give the coordinates of the "BX" signboard visible in this picture.

![](/images/diver-osint-ctf-2025-kakitsubata/bx.jpg)

### 解いた流れ

画像をGoogleレンズにかけると写真左側のマンションが「サニーコート上野」だということがわかったのでサニーコート上野で検索してだいたいの住所を検索して、解答欄上でちょっとピンの位置を動かしてsubmitしました。

![](/images/diver-osint-ctf-2025-kakitsubata/bx-flag.jpg =350x)

### Flag

35.7182818615,139.7810000181

## document [kuzushiki] (100pt / 280 solves)

### 問題

アメリカ海軍横須賀基地司令部（CFAY）は、米軍の関係者向けに羽田空港・成田空港と基地の間でシャトルバスを運行している。2023年に乗り場案内の書類を作成した人物の名前を答えよ。
Flag形式: Diver25{George Washington}

The US Navy Commander Fleet Activities Yokosuka (CFAY) operates a shuttle bus service between Haneda airport/Narita airport and the base for US military personnel. Answer the name of the person who created the document about the boarding location of the bus in 2023\.
Flag Format: Diver25{George Washington}

(Hint) 情報源に記載されている通りの形式で答えてよい。 / You can answer in the format as described in the source.

### 解いた流れ

「CAFY 羽田空港・成田空港」でググると以下のページがヒットした。

https://cnrj.cnic.navy.mil/Installations/CFA-Yokosuka/About/Installation-Guide/Airport-Shuttles/

PDFのリンクが5つあった。この問題では書類の作成者を聞かれているためPDFのメタデータが残っていると推測。
上から順に調べていくと、FAQsのドキュメントに作成者の情報が残っていた。

### Flag

個人名のため省略

## finding\_my\_way [meow] (100pt / 408 solves)

### 問題

34.735639, 138.994950 にある 建造物 の、OpenStreetMapにおけるWay（ウェイ）番号を答えよ。
Flag形式: Diver25{123456789}

Answer the Way number in OpenStreetMap of the building located at 34.735639, 138.994950. Flag Format: Diver25{123456789}

Hint「建造物」は「地物」の一種である / a "building" is categorized into "features"

### 解いた流れ

OpenStreetMapで確認するだけ

https://www.openstreetmap.org/?mlat=34.735639&mlon=138.99495#map=19/34.735643/138.994198

![](/images/diver-osint-ctf-2025-kakitsubata/finding.jpg)

2024年の osprey2 に問題の観点が似ており、「みんなちゃんと復習した?」という運営のメッセージを感じる。

https://github.com/diver-osint-ctf/writeups/tree/main/2024/military/osprey2

### Flag

Diver25{568613762}

## flight\_from [kuzushiki] (100pt / 362 solves)

### 問題

このヘリコプターが出発した飛行場のICAOコード（4レターコード）で答えよ。
Flag形式: Diver25{RJTT}

Answer with the ICAO code (4 letter code) of the airfield from which this helicopter departed.
Flag Format: Diver25{RJTT}

(Hint) データを入念に確認してください。あなたのOSINT能力を期待しています。 / Confirm the data carefully. We expect your OSINT ability.

### 解いた流れ

問題画像を見ると、出発した飛行場の空港コード「OKO」が記載されている。

![](/images/diver-osint-ctf-2025-kakitsubata/flight_from.jpg)

OKOは横田基地にある飛行場のことであり、wikipediaで確認したICAOレコード「RJTY」を提出するも不正解。
wikipediaの記載が間違っているのかと思い、他の資料もチェックしたがICAOレコードに間違いはなさそうだった。

ここで「データを入念に確認してください。」というヒントを思い出す。「本当にOKOから出発しているのか？」と思い飛行機の軌跡を辿ると、立川駅付近から飛んでいることがわかる。

wikipediaから日本の空港のICAOレコード一覧のリストを見つけ、その中を「立川」で検索してヒットしたものが正解だった。

https://ja.wikipedia.org/wiki/ICAO%E7%A9%BA%E6%B8%AF%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AE%E4%B8%80%E8%A6%A7/R

### Flag

Diver25{RJTC}

## hidden\_service [nomizou] (100pt / 354 solves)

### 問題

添付ファイルを確認して、Flagを獲得してください！
See the attached file and capture the flag\!
Flag形式 / Flag Format: Diver25{xxxxxxxxxxxxxxxxx}

![](/images/diver-osint-ctf-2025-kakitsubata/hidden.jpg)

### 解いた流れ

パッと見でダークウェブにアクセスさせる問題だということがわかったものの、URLを読むのが面倒だったのでChatGPTに文字をテキスト化してもらうように依頼。ちょいちょい間違えているところがあったので最後は目視で文字列を修正してtorブラウザでアクセスしたらフラグが表示された。

![](/images/diver-osint-ctf-2025-kakitsubata/hidden_flag.png)

### Flag

Diver25{w3lc0m3\_70\_d4rkw3b\!}

## louvre [kn1cht] (176pt / 181 solves)

### 問題

ルーブル美術館の公共Wi-Fiアクセスポイントのうち、以下の条件を満たすもののベンダーを答えよ。

情報は2025年2月28日に収集されており、オンライン上で確認できる。
ベンダーはBSSIDに準拠して判定せよ。
Flag形式: Diver25{Company Name} (例: Apple Inc.の場合、Diver25{Apple Inc.} となる。)

Answer the vendor of one of the Louvre's public Wi-Fi access points that meets the following criteria.

Information was collected on 28 February 2025\. This can be accessed via online.
Determine the vendor according to the BSSID.
Flag format: Diver25{Company Name} (e.g. If the company is Apple Inc., the flag should be Diver25{Apple Inc.}.)

(Hint 1) Wi-Fiアクセスポイントに関して、有名なサイトがあるはずです。 / There should be a well-known website on Wi-Fi access points.
(Hint 2) BSSIDの仕様について調べてみましょう。 /Find out about the BSSID specifications.

### 解いた流れ

OSINT CTFでWi-FiといえばWiGLEですよね～ということで、WiGLEにログイン（ログインしないといろいろ制限されます）してルーブル美術館を見ます。

![](/images/diver-osint-ctf-2025-kakitsubata/louvre1.jpg)

いや多すぎる！

ここで、WiGLEの検索画面をよく見るとSSIDや期間で絞り込めることがわかります。
今探しているものは、ルーブル美術館の公共Wi-Fiアクセスポイントです。公共なので、SSIDは公表されているのではないでしょうか？　というわけで、「Louvre Wi-Fi」などとググると、公式サイトで「Louvre\_Wifi\_Gratuit」が公式のSSIDであることがわかります。

https://contact.louvre.fr/hc/en-gb/articles/12853523479453-Do-you-offer-WiFi

Date-Rangeも2024-2026とかに絞っていざ検索すると、今度は何も表示されません……。

※ ここで、Advanced Search（[https://wigle.net/search](https://wigle.net/search)）に切り替えてクエリを実行すると、実は2つだけ”Louvre\_Wifi\_Gratuit”が表示されます（終了後に気づきました）。そのBSSID（MACアドレスであることが多い）50:60:28:4E:17:E0からvendorを調べて**“Xirrus, Inc”**にたどり着くというのがどうやら想定解なのですが、筆者はAdvanced Searchの存在を知らずスルーしたまま調査を続けてしまいました。先に調べを進めていたチームの人がAdvanced Searchのスクショをメモに貼ってくれていたのに……。

Date-Rangeをもう少し広めに取ると”Louvre\_Wifi\_Gratuit”はマップ上に表示されます。ということは、ネットワーク機器の更新などがあってここ数年ではSSIDが変わってしまった可能性も考えられます。

そこで、SSIDを限定せずに2025年のデータを見ていくと、”\*Louvre\_WiFi\_Gratuit”というSSIDがあることに気づきました。上とほぼ同じですが、先頭にアスタリスク記号がついています。

![](/images/diver-osint-ctf-2025-kakitsubata/louvre2.png)

”\*Louvre\_WiFi\_Gratuit”で絞り込むと、数十個のデータが表示された状態になりました。ほぼ答えにたどり着いていますが、問題文にある通り2025年2月28日のデータを見つける必要があります。マウスでポチポチ選択していても見つからないのでやり方を変えることにしました。

WiGLEにはAPIが存在し、マイページ（[https://wigle.net/account](https://wigle.net/account)）からAPI tokenを発行できます（Authorization: Basic …に続く文字列）。APIを使用するためのサンプルPythonコードが公開されているので、それを流用しました。

https://github.com/jbjulia/wigle/blob/master/wifi.py

元のスクリプトでは緯度・経度の範囲を絞り込めるようになっています。加えてSSIDでもフィルタをかけたいので、スクリプトを少し編集してパラメータに含めてあげます。

```py:wifi.py
   query_params = {
        "ssid": "*Louvre_WiFi_Gratuit",
        "lat_range_1": 48.86,
        "lat_range_2": 48.8625,
        "long_range_1": 2.3326,
        "long_range_2": 2.34,
        "first": 100,
    }
```

wifi,pyを実行すると数百行のCSVファイルが保存されます。"2025-02-28"でファイル内を検索することでBSSID 90:4C:81:D9:12:01のアクセスポイントの記録が見つかります。

https://maclookup.app/macaddress/904c81

あとはMAC addressからvendorをlookupしてくれるサイト（たくさんある）を使って、Hewlett Packard Enterpriseの製品であることを確認してsubmitです。

しかし、なんとこれがincorrect！

次回、怒りの問い合わせ編。デュエルスタンバイ！

### 解いた流れ2（問い合わせ編）

問題文の条件に合う情報を見つけたことは確実に思われるので、チームメイトにも情報をチェックしてもらい、CTF運営に問い合わせようということになりました。

こうした「合っているはずなのに不正解になる」という問い合わせをすることは時々あります。プレイヤー側の勘違いとして一蹴されて終わるケースもある一方で、運営が問題文の条件に合致することに納得すれば別解としてフラグが追加され正解としてくれることもあります。納得してもらうため、あくまで冷静に合理的な判断理由を漏れなく、簡潔にまとめた方がよいです（運営側が、我々が得た情報が正解といえるかどうかを判断する材料にもなりうるためです）。

実際の問い合わせ文面はこのようなものでした。

```
louvre について、問題の条件を満たすWi-Fi APおよびBSSIDを見つけたと思うのですが、incorrectになります。
これは実際に不正解でしょうか、それとも問題文の条件を満たすため正解と判定できますか？

* 2025年2月28日に収集されたルーブル美術館の公共Wi-FiアクセスポイントをWiGLEで見つけました（スクリーンショット）。
* BSSIDは90:4C:81:D9:12:01でした。
* vendor ID 90:4C:81はHewlett Packard Enterpriseを表します。https://maclookup.app/macaddress/904c81
* 以上から、Diver25{Hewlett Packard Enterprise}を提出しましたが不正解になりました。
```

![](/images/diver-osint-ctf-2025-kakitsubata/louvre3.png)

運営のryo-aさんが確認してくださり、10分ほどで「想定していない回答ではあるが、問題の条件を満たすため正解flagとして扱う」とのお返事をいただきました。
Discordの全体でもFlagの修正がアナウンスされ、我々のチームも正解となりました。

![](/images/diver-osint-ctf-2025-kakitsubata/louvre4.png)

CTFで運営に問い合わせをするというのは、ただ解いているよりもハードルが高く感じる行動かもしれません。もちろん根拠もなくヒントを求めるようなことはNGですが、今回のように問い合わせしたことで有利になるケースもあるので、皆さんもためらわずに運営に聞きまくったほうがいいのではないでしょうか。

### Flag

Diver25{Hewlett Packard Enterprise}

## night\_accident [nomizou, meow] (100pt / 269 solves)

### 問題

動画 / Video:
https://www.youtube.com/watch?v=jHgqCpJNL28

この動画で、車とバスが衝突しそうになった場所はどこか。
In this video, where did the car and bus almost collide?

content warning:
衝突には至らないものの、交通事故のようなシーンがあります
There are scenes that do not result in a collision, but are similar to a road accident.

### 解いた流れ

日本の伝統、バス問題。

まずnomizouさんによりバスはSBS Transit であり、場所はシンガポールであると推察いただいた。
動画を最大の解像度かつ再生速度を最も遅く設定し、分析に必要そうな情報を推測する。
バス番号がくっきりとわかる。動画内の時刻0:00の52、時刻0:04あたりの58というバス番号がわかる。

nomizouさんがバスの番号からルートを照会できる地図サイトを見つけ、52と58の経路を確認いただく。

https://busrouter.sg/?lng=ja\#/services/52

![](/images/diver-osint-ctf-2025-kakitsubata/night_accident.jpg =450x)

共通するルートは 出発地点のBishan Int駅 入口付近。
あとはグーグルストリートビューで似ている場所を探す。

道路のペイントや景観から次の場所を見つける。

https://maps.app.goo.gl/AS5EWdkCYXgisEyC6

### Flag

![](/images/diver-osint-ctf-2025-kakitsubata/night_accident_flag.png =350x)

こんなgeo問にふさわしいターゲットをよく見つけたな、という印象。

## p2t [kuzushiki] (377pt / 112 solves)

### 問題

この写真の左側に掲示されている写真には、ある動物が写っている。その名前を、日本語で書かれている通りに答えよ。
Flag 形式: Diver25{シバイヌ}

The photo posted to the left of this photo shows an animal. Answer its name as it is written in Japanese.
Flag Format: Diver25{シバイヌ}

### 解いた流れ

提供されている写真は以下のとおり。

![](/images/diver-osint-ctf-2025-kakitsubata/p2t.jpg)

まずはGoogle Lensによる画像検索を試すも、撮影場所が一致しそうなものは見つからず。

次に、カラスの写真の右側に文章が映り込んでいるのが気になったため、文章も駆使して特定する方法を考える。
立山は地名なのでは？と推測し、「立山　ハシブトガラス」で検索すると以下のブログがヒットする。

https://yachou.info/murodoudaira-raichou/

雷鳥についての記事であり一見無関係そうに思えたが、念のため一番下まで確認すると問題と同じカラスの写真が見つかった。

（ブログの写真のURL: https://i0.wp.com/yachou.info/wp-content/uploads/2022/09/murododaira-20220919-54-scaled.jpg?resize=1024%2C576&ssl=1 ）

左側に写っている「トビ」が正解だった。

### Flag

Diver25{トビ}

## ship [kuzushiki] (100pt / 363 solves)

### 問題

これは、ある組織が運用する船舶である。もし将来、この船が外国に売却されたとしても、変わらない番号を答えよ。
Flag形式: Diver25{現地語での船名\_番号}（例: 船名が「ペンギン饅頭号」で番号が 1234567 の場合、Flagは Diver25{ペンギン饅頭号\_1234567} となる）
This is a vessel operated by a some organisation. Answer the number that would remain the same if this vessel were to be sold to a foreign country in the future.
Flag Format: Diver25{ship name in \*\*the local language\*\*\_number} (e.g. If the ship name is "ペンギン饅頭号" and the number is 1234567, the flag should be Diver25{ペンギン饅頭号\_1234567}.)

(Hint) 船名には記号を含まない。 / The ship name doesn't contain symbols.

### 解いた流れ

問題文にある「もし将来、この船が外国に売却されたとしても、変わらない番号」はそのまま検索するとIMO番号だとわかる。

https://ja.wikipedia.org/wiki/IMO%E7%95%AA%E5%8F%B7

>  船舶に与えられたIMO番号は廃船になるまで変更されることがなく、所有者や船籍が変更されたとしても変更されない。

次に問題の写真から船を特定する。

![](/images/diver-osint-ctf-2025-kakitsubata/ship.jpg)

右上の「7JVV」の記載から船を特定できるのではと推測し、ググると神鷹丸がヒットした。

https://ja.wikipedia.org/wiki/%E7%A5%9E%E9%B7%B9%E4%B8%B8\_(4%E4%BB%A3)

同記事にIMO番号も併記されており、提出したら正解だった。

### Flag

Diver25{神鷹丸\_9767675}

# geo

## Afghanistan [blackwasan] (414pt / 94 solves)

### 問題

動画 / Video: https://www.youtube.com/watch?v=NWQwx4-MeRg&t=65s

この動画の65～67秒に表示される写真はいつどこで撮影されたものか。
Flag形式: Diver25{撮影場所名\_YYYY-MM-DD}（撮影場所名は英語）
例えば、2025年6月5日に、Camp Darbyで撮られた場合は、Diver25{Camp Darby\_2025-06-05}となる。
When and where was the photo shown at 65-67 seconds in this video taken?
Flag format: Diver25{location name\_YYYY-MM-DD} (location name should be in English)
For example, if it was taken at Camp Darby on June 5, 2025, it would be Diver25{Camp Darby\_2025-06-05}.

### 解いた流れ

当該YouTubeの65秒から、兵士がキャンプでバスケットボールをする静止画が映されている。これの撮影場所と日付を答える問題。

[https://youtu.be/NWQwx4-MeRg?t=65](https://youtu.be/NWQwx4-MeRg?t=65)

アフガニスタンであること以外に、そもそも全く場所が分からないので、とりあえずGoogleに聞いてみる。画像検索でbasketballとキーワードを付加してやると、GettyImagesで同じ写真がヒット。ページに遷移すると、その写真は消されていた形跡があったが、写真家名がLiu Jinであることが分かった。 また、キャンプ地が”Bostick”であろうことも分かった。

![](/images/diver-osint-ctf-2025-kakitsubata/afghanistan.jpg)

再度Googleにて"Camp Bostick Liu Jin"でググると、MediaFaxFotoのページがヒットする。Liu Jinさんが撮影した写真が含まれる。

https://mediafaxfoto.ro/Preview.aspx?Id=3461711

直接このリンクの写真などは該当しないが、このサイトの中にありそうなので、サイトトップに戻り、写真家名：Liu Jinで検索。

https://mediafaxfoto.ro/Photos.aspx?fm=lSearch&q=AFGHANISTAN&pt=0&f=Liu%20Jin&s=5,130,51,301,128,6,122,201

すると、下記のURLに問題と全く同じ写真があったので、右側に書いてあるメタデータを読めば、これが答えとなる。

https://mediafaxfoto.ro/Preview.aspx?Id=3480173

### Flag

Diver25{Camp Bostick\_2009-04-16}

## advertisement [blackwasan] (100pt / 233 solves)

### 問題

記事 / Article: https://web.archive.org/web/20250108154113/https://www.noticiasaominuto.com/mundo/2699746/kyiv-diz-que-russia-usou-como-recrutas-ate-180000-presidiarios
この記事の写真が撮影された場所はどこか。
Where were the photographs in this article taken?

### 解いた流れ

“ТОКИО-CITY”などの文字列を頼りにしながら、画像検索等で、場所はこのあたり（サンクトペテルブルク付近）とわかる。

https://maps.app.goo.gl/3Se55CM8NhrnUFoaA

撮影場所を入れてsubmitしてみるも、ピンが外れてしまったようで、incorrect。

![](/images/diver-osint-ctf-2025-kakitsubata/advertisement1.png =300x)


再度、撮影者の位置関係を整理し、ピンを置きなおして、正解。
![](/images/diver-osint-ctf-2025-kakitsubata/advertisement2.png =300x)

ピンの許容範囲がかなり狭いので、慎重に整理する必要があった。

### Flag

59.943003, 30.278919 付近にピンを置き、CTFdにて送信

## elevator [blackwasan] (499pt / 12 solves)

### 問題

これらの写真はあるホテルの客室内と、その窓からの景色を撮影したものである（撮影日は2024年）。
このホテルに設置されたエレベーターには、2件の故障記録が存在しているものが1基ある。 故障日と、故障記録があるエレベーターの登録番号を答えよ。
Flag形式: Diver25{YYYYMMDD\_YYYYMMDD\_1234567}
例えば、登録番号 "1234567" のエレベーターが2000年11月20日と2020年2月13日に故障していた場合、Flagは Diver25{20001120\_20200213\_1234567} となる。
These photos show the interior of one hotel room and the view from its windows (Taken in 2024).
There is one elevator installed in this hotel for which two failure records exist.
Answer dates of failure and the registration number of the elevator that has a fault history.
Flag Format: Diver25{YYYYMMDD\_YYYYMMDD\_1234567}
For example, if an elevator with registration number "1234567" failed on 20 November 2000 and 13 February 2020, the Flag would be Diver25{20001120\_20200213\_1234567}.
注意 / CAUTION:
ホテルや企業に接触しないでください。公開情報からFlagを入手できます。
DO NOT CONTACT HOTELS AND COMPANIES. YOU CAN GET THE FLAG FROM OPEN SOURCE.

(Hint 1) 故障が報告された日ではなく、故障が発生した日を解答してください。 / Answer the date the fault occurred, not the date the fault was reported.
(Hint 2) 事故に関する画像や映像を閲覧する必要はありません。 / No need to see the image or video of accidents.

![](/images/diver-osint-ctf-2025-kakitsubata/elevator1.jpg =300x)
![](/images/diver-osint-ctf-2025-kakitsubata/elevator2.jpg =300x)

### 解いた流れ
崩れた鉄骨の写真と、東横インに泊まっていることを匂わせる写真がある。
問題は故障したエレベーター番号と故障日の特定だが、まずは東横インがどこなのかを特定する必要がある。

チーム総出で国内の東横イン周辺のストリートビューなど片っ端からを見ていくも、深夜の時点で膠着状態に。

なんとなく解体工事の雰囲気も変なことと、海外にも東横インは何店舗か存在するので１回は調べてもよいか、と入眠一歩手前で着想したのが幸運だった。
東横イン室内に日本語表記があることから、まずはお隣韓国をあたってみる。

解体工事が行われていたこともあわせてGoogle等で調べると、釜山の東横イン滞在レポートを載せている方（個人ブログ）が見つかった。

https://plumeria8.hatenablog.com/entry/toyokoinn-pusan-somyon

その情報から推測すると、問題のホテルは「東横イン釜山西面」ではないかという答えにたどり着いた。

https://www.toyoko-inn.com/search/detail/00221/

一方、解体工事現場と思われる物件（ストリートビューは当時）は、当該東横インの南西方向すぐの区画にあり、これが問題写真１枚目の壁面・街灯の形とぴったり一致していた。そのため、題意のホテルはこのホテルで確定となる。

https://maps.app.goo.gl/o5JhiUscZ8pf4wY4A

ホテルは分かったので次にエレベーターについて探す。
Tripadvisorやその他宿泊情報サイトの口コミをざっと見てみるも、エレベーターに言及しているものはあまりなかった。（めちゃくちゃ待たされた、などの内容のみ）
さすがにエレベーターの故障履歴や検査証を撮影してドカドカとアップロードする観光客もいるわけないか・・・と思ったので、現地語で検索していく方針に。

「동요코인 부산서면점 엘리베이터（東横イン釜山西面店　エレベーター）」でGoogle検索すると、下記のNAVERブログが見つかる。どうやらこの方は韓国一円のエレベーター情報のほかにバス情報などをまとめているらしい。

https://blog.naver.com/keo7071/223573156509

ブログでは問題のホテルでエレベーターに乗る動画が引用されている。エレベーターは2基あり、それぞれに搭乗して上階まで行き、1Fまで戻ってくるという何ともマニアックな内容。この時点でもう間違いなくどこかに情報が隠されている雰囲気だが、YouTubeの概要欄やブログ記事の内容から、1号機のエレベーター番号は8053118、2号機のエレベーター番号は8019148であることが容易に分かる。

https://youtu.be/ezN_xPu7Q2Q

また、このYouTubeの概要欄には、ご丁寧にも韓国の「国家エレベーター情報センター」へのリンクが示されており、こちらで先述のエレベーター番号を入力して調べていくと、題意の故障履歴（2回分）が表示されるので、これが答えとなる。

https://www.elevator.go.kr

![](/images/diver-osint-ctf-2025-kakitsubata/elevator_flag.png =300x)
*写真：エレベーター番号8019-148の故障履歴（日本語翻訳済）*

### Flag

Diver25{20230810\_20241124\_8019148}

## hole [blackwasan] (449pt / 72 solves)

### 問題

この穴があった場所はどこか。
Where was this hole located?

![](/images/diver-osint-ctf-2025-kakitsubata/hole.jpg)

### 解いた流れ

Google lensに通してみると、中国BYDのテスト走行の障害物として使われた穴らしいことが分かった。

https://www.smartprix.com/bytes/byds-yangwang-u9-can-jump-over-potholes-literally-but-how-passive-vs-active-suspension-explained/

その中で、公式の走行動画(YouTube)があったのでそれを見てみる。

https://www.youtube.com/watch?v=zIKAn8yDkpA

山西大同（山西省大同市）の空港とな。

![](/images/diver-osint-ctf-2025-kakitsubata/hole-airport.jpg)

近い見た目のものを大同市内で探していくと、大分中心から外れではあるが、あった。OSM上では"平型关通用机场/灵丘机场"とされている飛行場。

GoogleMapsだとここになる。

https://maps.app.goo.gl/mQYExguWRpGeEJtZ9


Googleの衛星画像

![](/images/diver-osint-ctf-2025-kakitsubata/hole-google.jpg)

一応、EOBrowserでsentinel-2の最新写真で形状を確認し、矛盾がないことを確かめる。

![](/images/diver-osint-ctf-2025-kakitsubata/hole-sentinel.jpg)

YouTube動画にて、0:25付近から穴の位置の説明がある。

[https://youtu.be/zIKAn8yDkpA?t=27](https://youtu.be/zIKAn8yDkpA?t=27)

### Flag

滑走路の形状が一致する場所が1つしかないことから、下記を送信し、正解。

![](/images/diver-osint-ctf-2025-kakitsubata/hole_flag.png =350x)

## night_street [nomizou] (428pt / 86 solves)

### 問題

画像の中心に写っている茶色の2階建ての建物に入る施設の正式名称を現地語表記で答えなさい。
Flag形式: Diver25{施設名}（例: Diver25{お台場海浜公園前郵便局}）
注意: この問題を解く際、外部サイトへの機械的なアクセスやスクレイピングは禁止します。また、それらを行う必要はありません。 ただし、ブラウザから手動で取得できる情報や、公開されているAPIサービスやデータセットの利用は問題ありません。

Answer the official name of the facility that will be housed in the two-story brown building in the center of the image (in local language).
Flag Format: Diver25{Facility Name} (e.g. Diver25{お台場海浜公園前郵便局})
Warning: Mechanical access or scraping of external sites is prohibited when solving this challenge. Nor are you required to do so. However, there is no problem with using public API services, public datasets and information retrieved from the web browser by (literally) your hand.

(Hint) The church-like building on the left is apparently a Japanese restaurant chain.

![](/images/diver-osint-ctf-2025-kakitsubata/night_street.jpg =300x)

### 解いた流れ

当然EXIFなどはなかったので画像から推定することになる。左側は日本人ならおなじみ「長崎ちゃんぽんリンガーハット」右側のなんとかクリニックは画像加工して手ブレを補正できないかしばらく試行錯誤してみたもののどうしても読めない。

長崎ちゃんぽんは全国で564店舗あるらしい。さすが有名チェーン店である。

https://shop.ringerhut.jp/all/

フードコートやショッピングモール内にある店舗も多いため、総当たり数はかなり減りそうではあるがそれでも300～400くらいはあるのでは？1件1分で確認したとして5時間か…と悩む。

写真の雰囲気からおそらく駐車場のある広めの路面店とあたりをつけて一旦上から確認していくが、途中で駐車場がないことになってる店舗にもまあまあ広めの店舗はあることに気づき、おそらくこれだと絞れないと感じて途中で挫折。

しかし総当たりをするなかで、意外とリンガーハットの店舗外観はバリエーションがあることに気づいた。具体的に言うと、トンガリ屋根の位置と屋根の土台部分の形状と、店舗の屋根の形状（水平か傾斜がある）と長崎ちゃんぽんの看板の大きさが店舗によって全然違う。

「リンガーハット　屋根」で画像検索すると様々な店舗外観が表示されるため、そこから見つけられないか確認していく。するとリンガーハット公式サイトの「ピックアップ店舗」というコーナーの第9回「名古屋弥富通店（愛知県）」が酷似していることに気づく。

https://www.ringerhut.jp/pickup/09.php

Googleストリートビューで確認するとまさにドンピシャだった。

https://maps.app.goo.gl/3M6gxSFMABQtfadZ7


隣のクリニック名を確認して解答した。
OSINTは気合と根性。

### Flag

Diver25{弥富通クリニック}

## what3slashes [blackwasan] (446pt / 75 solves)

### 問題

この画像が撮影されたとき、正面に家が3軒、右手に家が1軒あった。
それから少し経った、ある月の初旬にこの場所にもう一度訪れてみた。そのときには、正面左側に家が1軒増えていて、正面右側にもう1軒が建設中であった。その時点で、建設中の家に屋根はなかったが、黒っぽい屋根を作る予定だという。
「ある月の初旬」とは何年の何月のことだろうか。
Flag形式 : Diver25{YYYY/MM} (e.g. Diver25{2025/06})
When this image was taken, there were three houses in front and one on the right.
Some time later, at the beginning of one of the months, I visited this place again. At that time, one more house had been added to the left side of the front and another was under construction on the right side of the front. At that point, the house under construction did not have a roof, but they were planning to build a blackish roof. In which year and month is "the beginning of one of the months"?
Flag Format: Diver25{YYYY/MM} (e.g. Diver25{2025/06})

![](/images/diver-osint-ctf-2025-kakitsubata/what3slashes.jpg)

### 解いた流れ

モンゴル語で書かれたWhat3wordsの場所を示す写真がある。

文字起こしすると⇒///бумба.цогц.бататгав

これを素直に入れてみると、ウランバートル近郊住宅街に連れていかれる。

https://what3words.com/%D0%B1%D1%83%D0%BC%D0%B1%D0%B0.%D1%86%D0%BE%D0%B3%D1%86.%D0%B1%D0%B0%D1%82%D0%B0%D1%82%D0%B3%D0%B0%D0%B2

![](/images/diver-osint-ctf-2025-kakitsubata/what3slashes2.png)


周辺ストリートビューを見てみるも、手前までしかないため覗けない。

https://maps.app.goo.gl/H6NPth8m7zTp8ZTQ8

Google Earth Proでしつこく過去年代写真を確認（真ん中の区画の3つ並びの家群を指していると思われる）。

![](/images/diver-osint-ctf-2025-kakitsubata/what3slashes_earth1.jpg =450x)
*2016/5/31の航空写真 （出典：GoogleEarth）*

![](/images/diver-osint-ctf-2025-kakitsubata/what3slashes_earth2.jpg =450x)
*2016/9/13の航空写真。正面左に家がたつ。  （出典：GoogleEarth）*

![](/images/diver-osint-ctf-2025-kakitsubata/what3slashes_earth2.jpg =450x)
*2018/10/2の航空写真。正面右に家が建設中になる。  （出典：GoogleEarth）*

![](/images/diver-osint-ctf-2025-kakitsubata/what3slashes_earth3.jpg =450x)
*2018/11/27の航空写真。 正面右に家が竣工（黒い屋根）。  （出典：GoogleEarth）*

題意から、「ある月の初旬」は2018/10初旬と推測される。

### Flag

Diver25{2018/10}

## convenience [kuzushiki] (462pt / 63 solves)

### 問題

青森県内に、公園とコンビニ、スーパーマーケットが互いに約100m圏内に存在する場所がいくつかある。また、これはOpenStreetMapで確認可能である。
この条件を満たす 公園 のうち、最南端 のものについて、OpenStreetMap上での Way Number を答えよ。
なお、「公園」の定義は、OpenStreetMap上で "park" (leisure=park) と分類されているものに準拠する。
Flag形式: Diver25{123456789}

There are several locations in Aomori Prefecture (青森県) where a park, a convenience store and a supermarket exist within approximately 100 metres of each other. This can be also confirmed on OpenStreetMap.
Answer the Way Number (on the OpenStreetMap) of the southernmost of these parks that satisfy this condition.
The definition of "park" conforms to that classified as "park" (leisure=park) on the OpenStreetMap.
Flag Format: Diver25{123456789}

(Hint) 情報源に記載されている通りの形式で答えてよい。 / You can answer in the format as described in the source.

### 解いた流れ

「OpenStreetMapで確認可能」とのことからOverpassのクエリを書く問題だとわかる。

https://overpass-turbo.eu/

Claudeに手伝ってもらいつつクエリを作成する。
工夫した点は以下の2つ。

1. nodeやwayではなくnwrで検索する: OpenStreetMapにはnode, way, relationという3つの要素で構成されており、nwrを指定することですべての要素を検索できる。(`node["shop"="convenience"]`のように指定すると抜けが発生する可能性がある)

2. 「公園・コンビニ・スーパー間の距離が100m以内」という条件を分解する: 愚直に書くとクエリが複雑になるため、「100m以内にコンビニとスーパーがある公園」と「100m以内にスーパーがあるコンビニ」を調べ、円が重なるところを特定するようにした。

```
[out:json];

// 青森県のエリアを取得
area["name"="青森県"]->.searchArea;

// 青森県内のコンビニを取得
(
  nwr["shop"="convenience"](area.searchArea);
)->.convenienceStores;

// 青森県内のスーパーマーケットを取得
(
  nwr["shop"="supermarket"](area.searchArea);
)->.supermarkets;

// 青森県内の公園を取得
(
  nwr["leisure"="park"](area.searchArea);
)->.parks;

// 条件を満たす公園を検索
// 各公園から100m以内にコンビニとスーパーの両方がある
nwr["leisure"="park"](around.convenienceStores:100)(around.supermarkets:100)(area.searchArea)->.candidateParks;

// 上の条件ではカバーできないものを追記
// スーパーまでの距離が100m以内のコンビニを検索
nwr["shop"="convenience"](around.supermarkets:100)(area.searchArea)->.candidateConvini;

// 結果を出力
.candidateParks out geom;
.candidateConvini out geom;
```

上記のクエリで検索した結果、最南端のものは上沢巻目公園だった。

https://www.openstreetmap.org/way/556701681#map=19/40.508889/141.543769

### Flag

Diver25{556701681}

## Talentopolis [kn1cht] (472pt / 53 solves)

### 問題

記事 / Article: https://www.guineaecuatorialpress.com/noticias/primera_edicion_de_talentopoli

"Primera edición de Talentopolis" という記事内に登場するステージの位置を答えよ。

Answer the location of the stage appeared in the article titled "Primera edición de Talentopolis".

### 解いた流れ

まず、問題で与えられている[guineaecuatorialpress.com](http://guineaecuatorialpress.com)のページに初めてアクセスすると、Obiang Nguema Mbasogo （オビアン・ンゲマ・ムバソゴ）という人の誕生日祝いのページが表示されました。どうやらこのサイトは赤道ギニア共和国のメディアで、祝われている人は同国の大統領のようです。というわけで、大統領の誕生日について調べる問題かな？と10分くらい考えていたのですが、ページを再読み込みしたらぜんぜん違うニュース記事が出てきて横転しました。
どうやら、このサイトのどこにアクセスしても、最初だけ強制的に大統領の誕生日祝いが流れる仕組みらしいです。大統領の権力が強い国なんですね（婉曲）

気を取り直してPrimera edición de Talentopolisと題された記事を読むと、マラボ（Malabo）のサンフアン地区（San Juan）で2024年9月7日に開催された、若者がダンスや詩などを披露するイベントが開かれたことを伝えるものでした。記事には写真が3枚ほど添付されており、木製の小さいステージがあります。この位置を突き止めることが目標でしょう。

![](/images/diver-osint-ctf-2025-kakitsubata/talentopolis1.jpg)
*（https://www.guineaecuatorialpress.com/noticias/primera_edicion_de_talentopoli より引用）*

ただ、ニュース記事には具体的な建物名はありません。イベントを主催したJPLという団体のFacebook投稿には、場所として”A2 - San Juan (Malabo)”と書かれているものの、A2が何なのかは不明です。Malaboは赤道ギニアの首都のようで、そこまで広くはないもののGoogleストリートビューがないようなので景色を見ながら探すのも難航しそうです。

* https://www.facebook.com/permalink.php?story_fbid=pfbid02xKWfgXcuQoDVYKqUmFyw6NJdM8aRkWSnSkNd4kFCesjTMfqWzf8NTsk9cdTrtuVcl&id=61552561416670

”San Juan de Malabo” などのクエリでGoogleをいろいろ探していると、Talentopolisイベントを伝える別のニュース記事がありました。さらに、記事の出典として記載されていたAHORAEG（これも赤道ギニアのメディア）を検索すると、AHORAEG側の記事も発見できました。

* https://www.revistaequato.com/lanzamiento-del-proyecto-talentopolis-en-malabo-para-jovenes-artistas/
* https://ahoraeg.com/cultura/2024/09/10/inicia-el-proyecto-talentopolis-en-el-barrio-san-juan-de-esta-ciudad-capital/

![](/images/diver-osint-ctf-2025-kakitsubata/talentopolis2.jpg)

*（https://ahoraeg.com/cultura/2024/09/10/inicia-el-proyecto-talentopolis-en-el-barrio-san-juan-de-esta-ciudad-capital/ より引用）*

AHORAEGは景色を広く撮影しているうえ、写真データも高解像度なので手がかりが豊富そうです。

この記事の写真や、guineaecuatorialpressの写真すべてから得られる情報を書き出すと、次のようになります。

1. ステージは、3階建てくらいの建物の端に接するように置かれている
2. ステージの周囲は芝生で、少しだけ樹木がある
3. 建物から数メートル離れると芝生が終わり、白線が引かれた駐車場がある
4. ステージ近くの壁をよく見ると、”A-4”と書かれた銘板がある
5. 建物に対して左奥にも、同じデザインの建物がある。その屋根は緑色
6. ステージに向かって右側には赤っぽい屋根の小屋が並んでおり、さらにそのすぐ奥は樹木が生い茂っている
7. ステージに向かって右奥には、小屋よりも高さのある建物がある。これはステージがある建物とはおそらく違うデザインで、その屋根は灰色っぽい

手がかり4と5から、この建物は同じ見た目の集合住宅が密集している団地のようなものではないかと推測できました。Malaboにストリートビューがないとはいえ、San Juanで検索してピンが立ったあたりあたりの衛星写真を見ればすぐに緑屋根の建物群が見つかります。

![](/images/diver-osint-ctf-2025-kakitsubata/talentopolis3.jpg)

Google Mapsによると、この場所は”Viviendas sociales de SanJuan“（サンフアン社会住宅団地）と呼ばれているようです。日本でいう公営団地のような感じだと思います。

Google Mapsや画像検索で似たような地上風景がないかしばらく探す作業を行ったものの、空振りに終わりました。また、探している建物の番号がA-4であることから、サンフアン団地の配置図や工事資料のようなものがあればそこから建物の位置が分かるかも知れないと考えましたが、見つけられませんでした。

最終的に、ニュース記事の画像から読み取った建物・樹木などの配置の手がかりから、上から見たときの見た目を推定した図と衛星画像を見比べました。

![](/images/diver-osint-ctf-2025-kakitsubata/talentopolis4.png)
*（競技中はもっと雑に手書きで進めました）*

すると、サンフアン団地の建物の中で、この条件を満たすものが1か所だけあります。
ステージは建物の西の端すぐのところにあったはずなので、そこにピンを立てて回答しました。

![](/images/diver-osint-ctf-2025-kakitsubata/talentopolis_flag.png)

### Flag

3.73756, 8.79658 付近

# recon

## 00_engineer [kn1cht] (100pt / 346 solves)

### 問題

東京駅の近くでソフトウェアエンジニアの名札を拾った。おそらく落とし物だろう。

このエンジニアが勤務している会社のWebサイト（トップページ）のURLを答えよ。

Flag 形式: Diver25{https://google.com}

An software engineer's nameplate was picked up near Tokyo Station. This should be a lost item.

Answer the URL of the website (index page) of the company where this engineer works.

Flag Format: Diver25{https://google.com}

![](/images/diver-osint-ctf-2025-kakitsubata/00_engineer.jpg)

recon カテゴリの問題を解答するには、この問題の情報が必要となる。ただし、この問題を除き、どの順序で解答しても構わない。

To answer the challenges in the recon category, information on this challenge is required. However, the challenges can be answered in any order except this one.

(Hint) この名札が東京駅近くで見つかったことから、日本の会社だろう。 / This should be Japanese company because this nameplate was found nearby Tokyo Station, Tokyo, Japan.

### 解いた流れ

reconは、ある会社に関連する情報を次々と突き止めるというストーリー性のあるカテゴリでした。そのためにまずは会社のWebサイトを見つける必要があります。

kodai_snさんが何かのイベントで使ったと思われる名札の写真が与えられました。kodai_snを各種SNSで探すと、Xにアカウントがあることが分かります。

https://x.com/kodai_sn

プロフィールにリンクされている個人サイトで、この人がMagneight Softwareに勤務していることも分かります。

https://kodai-sn.github.io/

Magneight SoftwareをGoogle検索して出てきたWebサイトがFlagです。

### Flag

Diver25{https://magneight.com}

## 01_asset [kn1cht] (473pt / 53 solves)

### 問題

"00_engineer" の問題で見つかった会社のCEOが持っていたスマートフォンの資産番号を答えよ。
Flag 形式: Diver25{ABCD-12345}

Answer the asset number of the smartphone owned by the CEO of the company found in the "00_engineer" challenge.
Flag Format: Diver25{ABCD-12345}

### 解いた流れ

[https://magneight.com/about.html](https://magneight.com/about.html) を見ると、CEOはMizuki Sekozakiさんのようです。

一方、kodai_snさんのXの投稿では、CEOがGitの練習をしてくれたとあります。

https://x.com/kodai_sn/status/1914562813928071334

GitHubのアカウントkodai-snを見ると、fork-practiceというリポジトリがあり、1件forkされています。

https://github.com/kodai-sn/fork-practice

forkした先のアカウントはmizuki1206edelweissでした。

https://github.com/mizuki1206edelweiss/fork-practice

同じIDのInstagramのアカウントも見つかるので、投稿を眺めるとスマホの写真の投稿が数件あるのが分かります。

https://www.instagram.com/mizuki1206edelweiss/

この中にCEOのスマホの情報があるかもしれないのですべてを見ます。注意点として、Instagramにログインしていなければすべてを閲覧できません。
また、Instagramでは複数の画像・動画が1投稿に入っている場合があり、その場合は投稿を開いて画像の右端に出る矢印ボタンを押さなければなりません（複数枚あることを示すアイコンやボタンは見落としやすいので調査時は注意）。

すると、今のスマートフォンのバッテリーが劣化したため買い換えたい、というような投稿にiPhoneの背面と、資産番号シールのようなものが写りこんでいるのを発見しました。

https://www.instagram.com/p/DHAMO0zPYjt/?img_index=2


シールにはASSET No. MN24-P113と書かれており、これがFlagです。

### Flag

Diver25{MN24-P113}

## 02_recruit [kn1cht] (488pt / 37 solves)

### 問題

"00_engineer" の問題で見つかった会社で、採用を担当していると思われる人物の氏名を答えよ。
Flag 形式: Diver25{Shigeru Ishiba} (ローマ字表記)

Answer the name of the person you think is in charge of recruitment at the company found in the "00_engineer" challenge.
Flag Format: Diver25{Shigeru Ishiba} (In Latin alphabet. NOT Hiragana, Katakana, Kanji etc.)

### 解いた流れ

[https://magneight.com/contact.html](https://magneight.com/contact.html) を見ると、Careersの項目に「今は人材を募集していない」と書いてあります。

では以前は募集していたかも、ということで各種Webアーカイブで[magneight.com](http://magneight.com)を検索すると、4月時点の同ページが見つかりました。

https://archive.is/sD2R0

We are hiring!とのことで、ソフトウェアエンジニアを募集するGoogle Documentがリンクされています。

Google Documentの所有者を知る方法は色々とある（Google Driveのshared with me, GHuntなど）ようですが、筆者はxeuledocを使用しました。

```sh
$ xeuledoc https://docs.google.com/document/d/1aKBTbwtt01StVTf76JRCZyL0mp4hn5FZkFwPJ56xZRc
[+] Owner found !

Name : makoto.u.sunflower
Email : makoto.u.sunflower@gmail.com
Google ID : 14204889817436516997
…
```

見つけたGmailアドレスをGHuntなどでさらに調べると、このアカウントがGoogle Mapsにレビューを投稿していることが分かり、そこから氏名も分かります。

```sh
$ ghunt email makoto.u.sunflower@gmail.com
…
🗺️ Maps data

Profile page : https://www.google.com/maps/contrib/100984246270086465803/reviews

[Statistics]
Reviews : 3
Photos : 4
Answers : 19
```

本問は筆者がfirst bloodを獲得しました。

### Flag

 Diver25{Makoto Uchigashima}

## 03_ceo [kn1cht] (302pt / 142 solves)

### 問題

"00_engineer" の問題で見つかった会社の、CEOのメールアドレス（Gmail）を答えよ。
Answer the email address (Gmail) of the CEO of the company found in "00_engineer" challenge?
Flag 形式 / Flag Format: Diver25{example@gmail.com}

### 解いた流れ

01_assetを解く途中で見つけたCEOのGitHubリポジトリが怪しそうなのでcloneします。Gitではコミットログにメールアドレスが入るため、メールアドレスを隠しているつもりでも勝手に見られて氏名を推測されたり迷惑メールを送られたりすることはよくあります（筆者も……）。

```sh
$ git clone https://github.com/mizuki1206edelweiss/fork-practice
$ cd fork-practice
$ git log
commit 803a933d2b63da88e57433142c4d78a7837e51e8 (HEAD -> main, origin/main, origin/HEAD)
Author: mizuki1206edelweiss <mizuki1206anemone@gmail.com>
Date:   Mon Apr 21 17:55:41 2025 +0000

    add "YUKI-ONNA" story
…
```

mizuki1206edelweissさん（CEO）のアドレスを入手できました。

### Flag

Diver25{mizuki1206anemone@gmail.com}

## 04_internal [kn1cht] (416pt / 93 solves)

### 問題

"00_engineer" の問題で見つかった会社では、Webブラウザからアクセス可能な社内用のDevOpsプラットフォームが運用されている。
そのシステムのバージョン名を、表記されている通りに答えよ。
Flag 形式: Diver25{135.0.1-f2+nightly}

The company found in the "00_engineer" challenge operates an internal DevOps platform which can be accessed from web browsers.
Answer the name of the version of the system as it is displayed.
Flag Format: Diver25{135.0.1-f2+nightly}

### 解いた流れ

問題を読む限り、[magneight.com](http://magneight.com)のサーバーで何かしらサービスが動いていて、それを暴くことが目標のようです。チームメイトがgitlabなどを探してくれていましたが見つかっていない状況でした。

サーバーについて糸口を掴むためにnmapを撃ちます（他のチームのWriteupによると、shodanやCensysを見るのでもよいようです。そちらのほうがpassiveに調査できてよさそう）。

```
$ nmap magneight.com
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-06-07 15:06 JST
Nmap scan report for magneight.com (202.212.71.93)
Host is up (0.0050s latency).
rDNS record for 202.212.71.93: 202-212-71-93.indigo.static.arena.ne.jp
Not shown: 993 closed tcp ports (conn-refused)
PORT     STATE    SERVICE
22/tcp   open     ssh
25/tcp   filtered smtp
80/tcp   open     http
139/tcp  filtered netbios-ssn
443/tcp  open     https
445/tcp  filtered microsoft-ds
3000/tcp open     ppp
```

3000が開いているのは怪しい気がするので [http://magneight.com:3000/](http://magneight.com:3000/) を開くとGiteaというGitサービスが公開されていました。 ~~~~社内のGit公開するなよ~~~~

![](/images/diver-osint-ctf-2025-kakitsubata/04_internal.png)

左下にバージョン番号があり、これがflagです。

なお、このGiteaからはいくつか情報が拾えました。ここで判明する別のドメインは、05\_designerを解くにあたって必要です。

* vUeG624aAV9pというユーザがいる
* vUeG624aAV9p/git-testという空のリポジトリがある（cloneしても履歴ゼロ）
* vUeG624aAV9pのRSSフィードが [http://magneight.com:3000/vUeG624aAV9p.rss](http://magneight.com:3000/vUeG624aAV9p.rss) にあり、その中に `<link>[https://gitea.magn8soft.tokyo/vUeG624aAV9p](https://gitea.magn8soft.tokyo/vUeG624aAV9p)</link>` という部分がある
* 上記から、この会社がmagneight.comのほかにmagn8soft.tokyoという別ドメインも持っていることがわかる

### Flag

Diver25{1.24.0+dev-303-gd88b012525}

## 05_designer [kn1cht -> kuzushiki -> Тамияс] (500pt / 7 solves)

### 問題

"00_engineer" の問題で見つかった会社のWebサイトを手がけたデザイナーの連絡先（メールアドレス）を答えよ。
Flag 形式: Diver25{foo.bar@example.com}

Answer the contact email address of the designer who worked on the company's website found in the "00_engineer" challenge.
Flag Format: Diver25{foo.bar@example.com}

### 解いた流れ

[kn1cht / kuzushikiパート]
kodai_snさん、mizuki1206edelweissさんがデザイナーについてSNSで言及していて、写真の場所もすぐに調べられるものの、そこからデザイナーの身元が分かるわけではありません。
また、CEOのメールアドレスをEpieosやGHuntにかけると公開されたGoogleカレンダーが見つかりますが、これも結局回答に使うことはありませんでした。

そこで、04_internalで判明した別ドメインについて調べました。CTF開催日現在ではmagn8soft.tokyoは[magneight.com](http://magneight.com)に301リダイレクトされます。ただ、CEOは5月11日の投稿で、デザイナーがWebデザインの全て、ロゴからティザービデオまでを手掛けたと書いているのに、現在のサイトにはビデオはありません。実は別のWebサイトがあったのではないでしょうか？

https://www.instagram.com/p/DJgOlXVJg8t/

>  She did everything from the web design to the logo and teaser video:)

ここまでで、magn8soft.tokyoのアーカイブが残っていないか探したところ、Wayback Machineに3月9日のアーカイブがあるのを発見しました。

https://web.archive.org/web/20250309135450/https://magn8soft.tokyo/

確かに動画が流れています。

![](/images/diver-osint-ctf-2025-kakitsubata/05_designer.jpg)

ただ、動画（magneight-teaser.mp4）をダウンロードしてプロパティを見ても特に有益と思われる情報はないように見えました。

そうこうしているうちに、kuzushikiさんから「exiftoolで動画ファイルを調べると様々なメタデータが表示され、おそらくAdobe Premiereで制作されたのではないか」という指摘をいただきました（ありがとうございます！）。手元でexiftoolを実行してみると、Adobeに関係するソフトウェア名以外に、動画制作ソフトウェアが残したと思しきパスがあり、作成者のユーザ名がcallalily0629izuharaであることが分かりました。

```
$ exiftool magneight-teaser.mp4
...
Pantry Ae Project Link Full Path: /Users/callalily0629izuhara/Documents/footage/magneight.aep
...
Mac Atom Posix Project Path     : /Users/callalily0629izuhara/Documents/Adobe/Premiere Pro/23.0/Magneight_2025.prproj
```

ここからメールアドレスにたどり着かなければなりません。callalily0629izuharaに@gmail.comを繋げたアドレスはどうやら存在しないようでした。whatsmynameなどで”callalily0629izuhara”というIDのSNSアカウントがないか調べても出てこない……とチームの皆で唸っていたところ、ТамиясさんがBlueSkyにアカウントがあることを発見してくださいました。

[Тамиясパート]
チームメンバーが着手していたところに参戦しました。
動画のExifからユーザ名がわかっていたので、blueskyでそのまま検索しました（リンクは省略）。
※ blueskyにはログイン状態でないと情報を公開しない設定があるため、検索する際はログインした方が無難です。

blueskyのアカウントのプロフィール欄にlit.linkへのリンクが掲載されているため、そこを見るとメールアドレスが見つかります。

### Flag

Diver25{izuhara.calla.lily@outlook.com}

## 06_leaked [kuzushiki] (499pt / 12 solves)

### 問題

"00_engineer" の問題で見つかったソフトウェアエンジニアのパスワードが漏洩して公開されてしまったらしい。彼はパスワードを変更して事なきを得たようだが、漏洩したパスワードは何だっただろうか。
Flag 形式: Diver25{his\_password}

The password of the software engineer found in the "00_engineer" challenge has apparently been compromised and became public.
He seems to have changed his password and got away with it, but what was the compromised password?
Flag Format: Diver25{his\_password}

### 解いた流れ

kodai_sn氏は6月7日朝の投稿で、パスワードが漏れてしまったかもしれないと投稿しています。

https://x.com/kodai_sn/status/1931143317099819175

[kn1cht] GitHubやGiteaのリポジトリにはこれまで回答に使用していないものが多数あり、それらを使用するのではないかと考えていました。[kodai-sn.github.io](http://kodai-sn.github.io)のコミットログには2種類のメールアドレス（`kodaisn.development@gmail.com`, `shinonomekodai@gmail.com`）があるものの、それらのGoogle検索や [haveibeenpwned.com](http://haveibeenpwned.com) 等のパスワード流出チェックツールではいずれも情報がヒットしませんでした。

そうこうしているうちに、kuzushikiさんが「pastebinにあったよ～」と教えてくださり、そこにパスワードが載っていたためそのまま正解になりました。そもそもpastebinに触れてなさすぎて、ログインするとpastebinの中で検索ができるのを知らなかった……。

![](/images/diver-osint-ctf-2025-kakitsubata/06_leaked.png)

https://pastebin.com/eHzc8rmB

### Flag

Diver25{1\_4m\_fr0m\_h0kk41d0\_4nd\_l0v3\_54un4}

# transportation

## 36\_years\_ago [kuzushiki -> kn1cht] (347pt / 125 solves)

### 問題

このニュース動画に映っている航空機に、1989年8月時点で割り当てられていたトランスポンダのMode Sコードを16進数表記で答えてください。
https://www.youtube.com/watch?v=OvR2O_Vpwc0
Flag形式: Diver25{1234AB}
content warning: この映像は軽微な航空事故の様子を含みます。

Answer the transponder Mode S code assigned to the aircraft shown in this news video as of August 1989 in hexadecimal notation.
[https://www.youtube.com/watch?v=OvR2O_Vpwc0](https://www.youtube.com/watch?v=OvR2O_Vpwc0)
Flag Format: Diver25{1234AB}
content warning: This video contains footage of minor aviation accident.

### 解いた流れ

セスナ機の事故に関する事故のニュース映像が与えられました。私が解き始める前、kuzushikiさんが動画からJA4098という機体番号を読み取り、機種（Cessna 172P）も特定してくださっていました。

番号でググると、この機体の来歴が年表で閲覧でき、そのもっとも古い時期の項目には新規(N9768L)とあります。

https://jasearch.info/aircraft_hist.html?r_number=JA409

「N9768L "mode s"」でググると、この機体は1989年9月22日に米国で登録解除されていて、そのMode SコードはAD9D6Aだったことが分かります。

https://airport-data.com/aircraft/N9768L.html

ただ、問題文に書いてあった1989年8月の情報を見つけなければ確定情報とは言えないのではないか？と思って少し調べたが難航してしまったので、とりあえずで提出したら正答でした。深く考えすぎだったかもしれません。

### Flag

Diver25{AD9D6A}

## air2air [blackwasan -> kn1cht] (495pt / 24 solves)

### 問題

動画 / Video:
https://www.youtube.com/watch?v=wUa5TGj6uxc
（無音です / No audio）

この動画は2025年3月11日（現地時間）に撮影された。遠くに映っている航空機のコールサインと機体記号を答えよ。
（撮影者が搭乗している航空機ではない）
Flag形式: Diver25{コールサイン\_機体記号}（例: Diver25{ANA0183\_JA381A}）

This video was taken on 11 March 2025 (in local time).
Answer the callsign and registration number of the aircraft seen in the distance.
(Not the aircraft the person who shot the video is on board)
Flag Format: Diver25{CALLSIGN\_Registration}（Example: Diver25{ANA0183\_JA381A}）

(Hint) 有料の情報源を使う必要はない。 / No need to use paid information.

### 解いた流れ

blackwasanさんが、既に撮影者がSTARLUXという航空会社の機体に乗っていること、接近した別の機体をADS-B Exchangeから探すのがおそらく解放であるということまで判明させていました。
ただ、その過程で挙がったDiver25{CPJ007\_VP-CSY} や Diver25{HKE687\_B-KKJ} は不正解とのことで、新たな視点で分析すべくkn1chtが引き継ぎました。

まず、撮影者が乗っている機体はAirbus A350-900と考えられます。
与えられた動画の最後の方に「40A」という座席番号が写りこんでおり、さらに主翼の後ろ側に撮影者が座っていることが見て取れます。STARLUXのWebサイトから、40A席が主翼の後端当たりにあるのはA350-900だけです。

https://www.starlux-airlines.com/en-VN/booking/other-optional-service/seat-selection/a350-900

（どうやらウイングレットを見るだけで機種を特定できるすごい人もいたらしい）

STARLUXが保有するA350-900は、Planespottersによると10機のみです。

https://www.planespotters.net/airline/STARLUX-Airlines

これまで不正解が続いていたことから、なるべく漏れなく情報を拾うべきではないかと考え、念のため全部のHEX Codeを控えてから、撮影当日の動きをADS-B Exchangeの機能で確認しました。

例えば、”8991E8”というHEX Codeの機材の2025年3月11日の移動経路は以下のようなURLで閲覧できます。

`https://globe.adsbexchange.com/?icao=8991E8&lat=28.848&lon=126.205&zoom=7.4&showTrace=2025-03-11`

https://globe.adsbexchange.com/?icao=8991E8&lat=28.848&lon=126.205&zoom=7.4&showTrace=2025-03-11

![](/images/diver-osint-ctf-2025-kakitsubata/air2air.jpg)

日本の方向から台湾桃園国際空港（TPE）に向かっており、沖縄の北西を通過するルートだったものは、ADS-B Exchangeで確認できる限り次の3便でした（ADS-B Exchangeの時刻はUTCで、日本時間にするには9時間足すことに注意）。

* SJX823 沖縄通過はUTC06:30ごろ
* SJX801 沖縄通過はUTC07:15ごろ
* SJX803 沖縄通過はUTC09:00ごろ

SJX801についてはblackwasanさんが既に見ていてどうやら違いそうであることが分かっていたので、ADS-B ExchangeのPlayback機能でSJX823やSJX803を眺めると、別の機体（SKY8026）とかなり近づいたタイミングを見つけました。

![](/images/diver-osint-ctf-2025-kakitsubata/air2air-sky.png)

拡大すると、SKY8026はSJX803の左側少し前方に位置しており、動画で見えた位置関係と辻褄が合いました。コールサインはSKY8026、機体記号はJA73NYなので、つなぎ合わせればFlagになります。

### Flag

Diver25{SKY8026_JA73NY}

## listen [kn1cht] (473pt / 53 solves)

### 問題
音声 / Audio: https://drive.google.com/file/d/1s_HC_S_9szbkupw9jQo4Zsvkoj73Qd_6/view?usp=sharing

この航空管制のやり取りが録音された空港のICAOコード（4レターコード）を答えよ。
Flag形式: Diver25{RJTT}

Answer the ICAO 4 letter code of the airport where this air traffic control communication was recorded.
Flag Format: Diver25{RJTT}

(Hint) 音声の冒頭は無音です / The beginning of the audio is silent.

### 解いた流れ

航空無線を記録した5分41秒間の音声が与えられました（出典：LiveATC CYEG-Del-Gnd-Misc-Jan-18-2025-0500Z ただし、問題の都合上出典は競技終了まで非公開）。

筆者は航空の素人で、ノイズが入りまくった英語の無線のやり取りを聞き取るのは無理と早々に判断し、NotebookLMに投げ込んで文字起こしと解説を依頼しました。

![](/images/diver-osint-ctf-2025-kakitsubata/listen.png)

NotebookLMでも聞き取りできなかった部分や文字起こしのミスが多々あったものの、Departureの周波数が133.65であることや、Runway（滑走路） 02への着陸が行われたことなどが判明しました。

ここで、文字起こしを過信しすぎてRunway 02と33がある（おそらく周波数を滑走路と間違えて文字起こしされていた）と誤解し、周波数や滑走路番号が符合しそうな空港としてInyokern Airport (KIYK)を見つけてsubmitしたところ、不正解でした。

改めてNotebookLMの文字起こしをベースに、自分でも無線を聴き直してより確度の高い情報を得ようと試みました。半分未満しか結局聞き取れていませんが、以下の情報は確度が高いと判断しました。

* この空港には、滑走路 02 と滑走路 30 がある
* 無線の周波数は、departure: 133.65 ground: 121.7
* 誘導路 AD (alpha delta), PA (papa alpha)?, B1 (bravo one) がある

"airport" "133.65" "121.7" "runway 02" "runway 30" で検索し、出てくる空港が1つしかないこと、誘導路も一致するものがチャートにあることを確認しました。

本問は3回しか挑戦できない制限が設定されており、既に1回誤答を送信してしまっていたため、チームの人にも答えが合理的であることを確認していただき、提出しました。

終了後他のプレイヤーの発言を見てもLLMに丸投げした人が多かったようです。自分の耳で無線を聴くだけで解けた人はすごい……。

### Flag

Diver25{CYEG}

## next_train [kuzushiki] (100pt / 245 solves)

### 問題

この音声が録音された駅はどこか。駅名を答えよ（日本語でも英語でも可）。
（駅名は鉄道会社の公式サイトやWikipediaに記載されている表記とする）
Flag形式: Diver25{京都駅}

Which station was this sound recorded at? Answer the name of the station.
(The station name should be as it appears on the official website of the railway company or on Wikipedia.)
Either Japanese or English are okay.
Flag Format: Diver25{Kyoto Station}

### 解いた流れ

音声を聞くと、駅内アナウンスから以下の単語が聞き取れる。

- JR東日本
- 15番線
- 14:16分発
- 普通 久里浜行き

15番線で横須賀線が通る駅、という条件で色々調べると品川駅がヒットする。その後時刻表から14:16発の電車があることも確認でき、確信を持てたので提出したところ正解だった。
おそらく最も聖地巡礼しやすい問題なのではないだろうか？

### Flag

Diver25{品川駅}

## platform [kuzushiki] (136pt / 192 solves)

### 問題

この写真が撮影された駅はどこか。駅名を答えよ（日本語でも英語でも可）。
（駅名は鉄道会社の公式サイトやWikipediaに記載されている表記とする）
Flag形式: Diver25{京都駅}

Which station was this photograph taken at? Answer the name of the station.
(The station name should be as it appears on the official website of the railway company or on Wikipedia.)
Either Japanese or English are okay.
Flag Format: Diver25{Kyoto Station}

(Hint 1) 10両 9号車乗車口 means No.9 car's door of 10-cars train
(Hint 2) 弱冷房車 means mildly air-conditioned train car

### 解いた流れ

問題の写真は以下のとおり。

![](/images/diver-osint-ctf-2025-kakitsubata/platform.jpg =350x)

まず、オレンジ色の車両表示から中央線（もしくは青梅線）のホームだと推測できる。
次に「らいと」という看板が特徴的だと思い、「らいと＋駅名」で調べていくと牛浜駅がヒットした。

![](/images/diver-osint-ctf-2025-kakitsubata/platform_ushihama.jpg =450x)

### Flag

Diver25{牛浜駅}

## sanction [kuzushiki] (375pt / 112 solves)

### 問題

2024年10月25日、制裁下にあるロシア船籍のRORO船「ANGARA」がある港湾に停泊していることが衛星画像で確認された。
停泊位置を答えよ。

On 25 October 2024, satellite imagery confirmed that the Russian-flagged RORO vessel "ANGARA", which is under sanctions, was anchored in a certain port.
Answer the position of the anchorage.

### 解いた流れ

「ANGARA roro」などのキーワードでググると船の情報はすぐに見つかる。

https://www.vesselfinder.com/?imo=9179842

しかし「2024/10/25にどこに停泊していたか」という肝心な情報がなかなか見つからず苦戦した。
[Marine Traffic](https://www.marinetraffic.com/en/ais/home/centerx:-12.0/centery:25.0/zoom:4) を始めとした船の停泊履歴を確認できるサイトを巡回し、また10/25時点のアーカイブが残っていないかを[Wayback   Machine](https://web.archive.org/) などのアーカイブサービスで確認したが全く見当たらなかった。

ここで方針を変え、どこかの調査レポートなどに載っているのではないかと推測し、片っ端からそれらしいレポートを探していく。

最終的に以下のレポートの18ページ目に2024/10/25のANGARAの衛星画像が載っているのを発見した。

https://www.mofa.go.jp/files/100853978.pdf

### Flag

フラグ文字列ではなく地図上のポイントで答える形式であり、以下のポイントが正解だった。

![](/images/diver-osint-ctf-2025-kakitsubata/sanction.png =400x)

# history

## bridge [meow] (263pt / 155 solves)

### 問題

動画 / Video:
https://www.youtube.com/watch?v=fRMi8TXQRuo
（無音です / No audio）

この動画で列車が通過した橋梁は、ある災害で損傷した後に架け替えられたものである。架け替えに際して、他の橋梁の構造物が流用されたことがある文献に示されている。その流用元の橋梁名を答えよ（この橋の名前ではない）。
Flag形式: Diver25{橋梁名}（例: Diver25{日本橋川橋梁}）

The bridge that the train passed over in this video was replaced after it was damaged in one of the disasters. A document shows that structures from other bridges were diverted during the replacement.
What is the name of the bridge from which it was diverted? (NOT the name of THIS bridge)
Flag format: Diver25{The name of the bridge in Japanese} (Example: Diver25{日本橋川橋梁})

(Hint 1) The flag doesn't contain the train line name such as 山手線 (Yamanote line) / Flagに「山手線」のような路線名は含みません。
(Hint 2) OCR of kanji characters is sometime tough. When you are sure about your answer but you get incorrect, you might need to use other OCR tools. / OCRでは漢字が誤認識されることがあります。もし答えに自信があるのにincorrectとなった場合、別のOCRツールを使ってみてください。
(Hint 3) The word "bridge" is translated as "橋 (はし, hashi)", "橋梁 (きょうりょう, kyoryo)" or "橋りょう (きょうりょう, kyoryo)" in Japanese.
(Hint 4) Documents in Japanese might use regnal year to describe year. For example, this year (2025) is 令和7年 (Reiwa 7).

>[機械翻訳]日本語の文書では、年を表すのに「令和年」が使われることがあります。例えば、今年（2025年）は「令和7年」です。

### 解いた流れ

電車の車窓から撮影したと思われる動画である。(なおmeowは最初車から映したものだと勘違いしていた。nomizouさんから「にしては同じスピードで走ってない?」といわれ軌道修正することができた。)
橋の場所を特定するにも、この場所を特定する必要がある。
解像度を4kに上げ、再生速度を0.25倍に落とし、手がかりを調査することとした。

開始0秒 に手がかりになりそうなものが映っている。

![](/images/diver-osint-ctf-2025-kakitsubata/bridge.jpg)

ただし、「園芸資材 千★農園」の★の部分がわからない。
「歳」や「蔵」の異体字かと思い、調べたが出てこない。

「漢字手書き検索」で調べても出てこない。

https://mojinavi.com/tegaki

![](/images/diver-osint-ctf-2025-kakitsubata/bridge_tegaki.png)

他の手がかりがないかを探す。動画 0:28 に特徴的な看板があった。

![](/images/diver-osint-ctf-2025-kakitsubata/bridge_kanban.jpg)

文字を推測してgoogle検索し、サジェストの力も借りてNaughties Dstという文字であり、スロット屋さんであることがわかる。このお店の住所は 熊本県熊本市中央区黒髪7-29 である。

座標から 豊肥本線 上の橋であることがわかる。

https://maps.app.goo.gl/75yendGCdV8N4Fb4A

橋をGoogleストリートビューで確認すると、橋の下にJRの看板があり、”ここは「第二白川橋りょう」です”と書かれている。

https://maps.app.goo.gl/gnKrAEpPjqsRycL97

そこから、nomizouさんと、第二白川橋梁の歴史を調査することにした。
その結果、昭和28年に洪水が起きたことがわかる。

熊本・白川における橋梁変遷史

https://www.jstage.jst.go.jp/article/journalhs1990/18/0/18_0_1/_pdf/-char/ja

これが問題文の「災害で損傷した」に当たると考えられる。
しかし、他の橋梁を流用したという記述が見つからない。

なお、白川の橋梁に関する歴史は↑だけでなく、非常に数が多くヒットし、ページ数も多く、それらを調べては違うということを繰り返していた。

検索クエリを見直し、問題文から単語を借りることに。"第二白川橋梁 流用" で調べたところ、次のソースがみつかった。

熊本県下における近代橋梁の発展史に関する研究

https://kumadai.repo.nii.ac.jp/record/23247/files/24-0097-1.pdf

> 大阪城東線の渡川橋梁を転用し

とある。

### Flag

Diver25{澱川橋梁}

## internment [meow, kn1cht, blackwasan] (478pt / 48 solves)

### 問題

著名な木曜島の真珠採りダイバーであった藤井富太郎氏は、第二次世界大戦中、強制収容されました。彼が釈放された収容所と釈放の年月日を明らかにしてください。
Flag 形式: Diver25{YYYY-MM-DD\_キャンプ番号\_キャンプ地名\_州名}

例えば、「2025年6月1日に、キャンプ番号7番のDiver州Daibaキャンプ」から釈放されたならば、 Diver25{2025-06-01\_7\_Daiba\_Diver} となります。なお、キャンプ番号がない場合はX（大文字のX）を入れてください。例えば、Diver25{2025-06-01\_X\_Daiba\_Diver} となります。

Tomitaro Fujii(藤井富太郎), a prominent Thursday Island pearl diver, was interned during World War II. Please identify the camp from which he was released and the date of his release.
Flag Format: Diver25{YYYY-MM-DD\_CampNumber\_CampPlaceName\_State}

For example, if he was released from the Camp 7 (Daiba Camp) in Diver State, on June 1, 2025, the flag should be Diver25{2025-06-01\_7\_Daiba\_Diver}. If the camp doesn't have a number, please use X (capital Latin alphabet X). For example, Diver25{2025-06-01\_X\_Daiba\_Diver}.

### 解いた流れ

[meowパート]
木曜島はオーストラリアの島。名前で調べると有料の書籍が出てくる。

『最後の真珠貝ダイバー 藤井富太郎』

https://amzn.asia/d/7xtoz5K

ご多分に漏れず私も電子書籍を購入しました。

読んでいると、
「第8章 ヘイ収容所での抑留」というパートがあるので、このあたりを読んで、断片的に

ヘイ収容所
N.S.W → ニューサウスウェールズ州
釈放日: 1946年3月1日
第6捕虜収容所

フラグを構築してサブミットする。

Diver25{1946-03-01\_6\_Hay\_New South Wales}
↓

![](/images/diver-osint-ctf-2025-kakitsubata/hay-incorrect.png)

まぁ、そんな甘くないよね。
ということで、もう一度見直すと p.70 に以下の記載がある。
「トミさんは、タツラの捕虜収容所から、釈放のための明確な指示を受けていました。彼は12月10日〜」

このあたりで日本時間 2:00 ごろであり、meowの眠気が限界だったため、まだ起きている他のメンバーにバトンをパスした。

[blackwasan&kn1cht  パート]

meow氏から引継ぎ、Google検索。Tommitaro、Tommy、Fujii、Pearl Diverなどあらゆるキーワードを想定し検索するも有用情報は出てこない。
先ほどの有料書籍を見直すと、「12月10日にバスで出発することになっていた」という記述があり、釈放日が違うのではないか、ということで、Diver25{1946-12-10\_6\_Hay\_NewSouthWales}を試すも、無情にもincorrect。
その後、うんうん言いながら首をひねっていると、kn1chtがオーストラリア公文書館のリンクを発見。

https://www.naa.gov.au/explore-collection/immigration-and-citizenship/alien-registration-records

その中に、”To search for records of a person who may have been interned or classed as an alien, go to RecordSearch and enter the name you are searching for.”という記述があり、つまりは過去に捕虜として収容された人物の情報が参照できるとの情報が。

https://recordsearch.naa.gov.au/

すかさず、Tomitaroで検索すると、予想通り藤井富太郎氏のレコードが出てくる。View digital copyなるボタンを押すと、35枚もの当時の書類のデジタルデータが表示される。どうやら釈放日はあっていたらしいが、収容所がHayではなくTatura(Victoria州)であったらしい。

https://recordsearch.naa.gov.au/SearchNRetrieve/Interface/ViewImage.aspx?B=768768

![](/images/diver-osint-ctf-2025-kakitsubata/internment.png)
*Discordでエンドロールが流れる様子*

### Flag

Diver25{1946-12-10\_3\_Tatura\_Victoria}

### 余談

ちなみに、収容所番号がNo.4とも取れる画像もあったが、収容所番号は4でも正解とのこと。実際のところ、藤井氏はどこかの時点でHay収容所からTatura収容所に移動したと考えられるが、その詳しい経緯は判断できていない。

書類のpage 11には、「Camp No. 4 Taturaに請求したい物品はこれで以上です」などと書かれたカードがある。また、page 13には、1946年6月2日の日付でNo 4 Internment Campから送られたと思しき藤井氏に関係する書類がある。これらのことから判断するに、藤井氏は釈放以前の一定の期間をTatura地域の収容所のNo. 4（場所はRushworth）で過ごしていたのではないかと推察される。そこまでは結局競技中には読み取れなかったわけだが……。

https://www.taturamuseum.com/world-war-2-camps

# company

## bid [Тамияс -> kuzushiki] (491pt / 31 solves)

### 問題

2023年、オマーンにおいて、ある施設に関連するニュースが報じられた。
https://www.youtube.com/watch?v=TFdubskF9Kw

その後、2024年10～11月に、この施設に関連すると推定される、井戸およびパイプラインを建設するための入札が実施された。
このとき、3位の金額で入札した企業のCEOの名前を、Webサイトに掲載されている英語表記で答えよ。
Flag形式: Diver25{Kelly Ortberg}

In 2023, news related to a facility was reported in Oman. https://www.youtube.com/watch?v=TFdubskF9Kw

A tender was subsequently issued in October-November 2024 for the construction of wells and pipelines, presumably associated with this facility. Answer the name of the CEO of the company that bid for the third highest amount in this tender.
(In English. Also follow the format in the company's official site.)
Flag Format: Diver25{Kelly Ortberg}

### 解いた流れ

先にТамиясさんが取り組んでおり、入札の情報を特定していただいていた。

https://etendering.tenderboard.gov.om/product/nitParameterView?mode=public&tenderNo=62644&PublicUrl=1&CTRL_STRDIRECTION=LTR&encparam=mode,tenderNo,PublicUrl,CTRL_STRDIRECTION,randomno&hashval=a34c6c2faa1d2a895fd3b37806bafc76958a9ed9396aad8afe5dd7ef034977f2

このサイトのトップページにアクセスして日本語に翻訳すると「入札者とベンダー」という見出しがあったため、入札情報の詳細を確認できそうだと推測。

サイトを巡回して機能を一つずつ確認していくと、当該入札 (入札No. 1190/2023/MAFWR/DGAWRDK-94- Recall \- 1) の結果が見つかった。

![](/images/diver-osint-ctf-2025-kakitsubata/bid.png)

3位の企業はGolden Sands Transport Services LLCであり、企業ページにCEOの名前が載っていた。

### Flag

個人名のため省略

## expense [nomizou -> Тамияс] (499pt / 13 solves)

### 問題

2025年1月、São Miguel do Guamáの市長が、ある食品関連会社にクレジットカード決済で支払った金額（現地通貨）を答えよ。
Flag形式: Diver25{1234.56}（通貨記号/コードは不要）

Answer the amount (in local currency) paid by the mayor of São Miguel do Guamá to a food-related company by credit card payment in January 2025\.
Flag Format: Diver25{1234.56} (Currency symbol or code not required)

### 解いた流れ

[Тамиясパート]
チームメンバーが着手していたところに参戦しました。
以下の情報をメンバーが見つけてくれていたため、お金の流れもここにあるだろうなという仮説のもと調べ始めました。

>  市に透明性ポータルというものがあるらしい
>  [https://saomigueldoguama.pa.gov.br/portal-da-transparencia/](https://saomigueldoguama.pa.gov.br/portal-da-transparencia/)

このページの中からお金の匂いがするリンクを優先的に辿っていき、2025年1月の期間で問題文に該当する情報をピックアップしていきました。
※サイトを読むだけなので、特別変わったことはしていません。
すると、以下のリンクにたどり着きます。

https://www.governotransparente.com.br/acessoinfo/45709485/consultarpagordemcronologica?ano=9&credor=-1&page=7&datainfo=%22MTIwMjUwNjA3MTMyMlBQUA==%22&inicio=01/01/2025&fim=31/01/2025&unid=-1&valormax=&valormin=&tipo=15&order=12&typeor=DESC&orgao=-1&elem=-1&covid=false

この一覧の中で、Empenho Unidade gestora Históricoが10010004の行に、BURITI DISTRIB UIDORA DE ALIM ENTOS LTDAという食品関連会社らしき情報があります。
ここに書いてある金額がFlagでした。

※問題文のクレジットカードやら市長もしくはそれに相当する情報がないため、submitするのを躊躇していたら、メンバーからとりあえず間違ってるか確認のためにsubmitしようと後押しされて投入しました。もう少し確実な情報があるのかを知りたいので、他チームのwriteupが楽しみです。

### Flag

Diver25{1.593.081,94}

# hardware

## phone [kuzushiki] (441pt / 78 solves)

### 問題

2016年7月23日～24日、この携帯電話の発売に先立ってEMI試験が行われた。試験は三重県の会社が実施したようだ。その試験に供された端末のシリアル番号を答えよ。
シリアル番号に / や \- といった記号を含む場合、その記号も含めて記載すること。
Flag形式（例）: Diver25{123-45/6789-0}

An EMI test was conducted prior to the launch of this mobile phone on 23-24 July 2016\. The test was conducted by a company in Mie Prefecture (Mie-ken), Japan.
Answer the serial number of the specific mobile phone that was subjected to that test.
If the serial number contains symbols such as / and \-, include them.
Flag Format (example): Diver25{123-45/6789-0}

(Hint) シリアル番号は情報源に記載されている通りでよい。 / The serial number should be as listed in the information source.

### 解いた流れ
写真から、ケータイの型番がdocomo SH-01Jであることがわかる。

https://www.docomo.ne.jp/support/product/sh01j/index.html

あとはどうやってEMI試験の情報を調べるかだが、事前にkn1chtさんから「FCCを調べれば良さそう」とアドバイスをいただいたため、FCC (Federal Communication Commissionの略で、アメリカ合衆国の米国連邦通信委員会のこと) から辿っていく方針とした。

docomo SH-01JのFCC ID「APYHRO00240」は以下のブログに載っていた。

http://blogofmobile.com/article/68417

FCCの試験結果のDBにて、FCC ID「APYHRO00240」の情報を探していく。

https://fcc.report/

検索画面が見当たらず戸惑ったが、パスパラメータがFCC IDになっていると推測し以下のパスにアクセスするとレポートの一覧が得られた。

https://fcc.report/FCC-ID/APYHRO00240/

レポートの数が多く一つずつ見ていくのは厳しいと思い、Document TypeがTest Reportのものに絞って見ていく。
EMI試験に関する物が見つかり、シリアル番号もそこに記載されていた。

https://fcc.report/FCC-ID/APYHRO00240/3109241


### Flag

Diver25{004401/11/583099/0}

## UART [kn1cht] (495pt / 24 solves)

### 問題

https://www.office-partner.de/tp-link-archer-ax20-12778639

この商品のイーサネットスイッチコントローラに直接UARTでアクセスを試みたい。どの部品の、どのピンにアクセスすればいいだろうか。
PCB上の部品番号 と、その部品の UART RX / UART TX ピン番号 を答えよ。ピン番号は部品の仕様に準拠せよ。
なお、ピンヘッダやコネクタが利用可能な場合でも、イーサネットスイッチコントローラのピン番号を答えてほしい。
Flag形式: Diver25{PCB上の部品番号\_UART RXピン番号\_UART TXピン番号}
（例えば、イーサネットスイッチコントローラが T21 という部品番号で、RXのピン番号が120、TXのピン番号が150であれば、Diver25{T21\_120\_150}となる）

I want to try to access the Ethernet switch controller of this product directly via UART. Which component and which pin should I access?
Answer the part number on the PCB and the UART RX / UART TX pin number of the component. The pin number should follow the specification of the component.
Note that even if pin headers and connectors are available, answer the pin numbers of the Ethernet switch controller. Flag Format: Diver25{Part Number on PCB\_UART RX pin number\_UART TX pin number}
(If the Ethernet switch controller has part number T21 on PCB, RX pin number is 120 and TX pin number is 150, the flag should be Diver25{T21\_120\_150}.)

(Hint) ハードウェアにも複数のバージョンが存在する点には注意しよう。いくつかのサイトでは商品名にバージョンが記載されているはずだ。 / Note that there are multiple versions of the hardware. The version name is written in the product name on some websites.

### 解いた流れ

TP-Link Archer AX20 AX1800 V2 Dualband Wi-Fi 6 Router という製品のEthernet switch controllerを特定する課題です。

これに着手する少し前、チームメイトが”phone”を解いていたとき、「EMI試験ならどうせFCCじゃないか」などと適当なことを言っていたら、本当にFCC申請のための試験資料に回答があったそうです（筆者はハードウェアを製造するスタートアップ企業にいる都合上FCCの資料に触れた経験がありました）。UARTも通信関連の製品なので、同じくFCCの情報で解けるのではないかという話になり、筆者が担当しました。

FCC関連の情報を探そうということで"Archer AX20 AX1800 FCC"などでGoogle検索していると、WikiDevi.Wi-Cat.RUというサイトにFCC ID: TE7AX20（https://fcc.report/FCC-ID/TE7AX20 ）という情報が記載されています。

https://wikidevi.wi-cat.ru/TP-LINK_Archer_AX1800

このままTE7AX20のFCC資料を見たくなりますが、与えられた通販サイトで”V2”というバージョン番号が製品名に入っていること、本問のヒントでも「ハードウェアにも複数のバージョンが存在する点には注意」と書かれていることから、V2については別のFCC資料があり、そちらが真の答えではないかと推測しました。

そこで、fcc.reportのTP-Linkの申請一覧（https://fcc.report/company/Tp-Link-Technologies-Co-L-T-D ）で”TE7AX20”を検索すると、TE7AX20V2を発見できます。

https://fcc.report/FCC-ID/TE7AX20V2/

10\. Internal PhotosのPDFファイルを見ると、いくつか基板上のチップ部品の拡大写真があります。チップに書かれている型番を調べていくと、U7という部品番号で RealTek RTL8367S-CG Ethernet switch が搭載されていることが分かりました（写真は転載していいのか不明なので貼りませんが、PDFの4ページ目にあります）。

https://www.lcsc.com/product-detail/Ethernet-ICs_Realtek-Semicon-RTL8367S-CG_C2760849.html

電子工作に触れたことがある方や本職の方であれば重々承知のとおり、部品のピン番号はデータシートに書いてあります。例えばLCSCという電子部品商社のサイトでデータシートが手に入るので、p. 17の”Pin Assignments”を見ればUART\_RXが45番、UART\_TXが46番と分かります。


得られた情報を繋げばFlagです。

### Flag

Diver25{U7\_45\_46}

# military

## object [Тамияс] (359pt / 120 solves)

### 問題

69.216246, 33.378242 には大きな構造物が存在する。この構造物のプロジェクト番号および、構造物の名称（固有名詞）を現地語で答えよ。
Flag形式: Diver25{プロジェクト番号\_名称}（例: Diver25{955А\_Борей-А}）

There is a large object at 69.216246, 33.378242. Answer the project number and particular name of that object.
Flag Format: Diver25{PROJECT NUMBER\_NAME} (e.g. Diver25{955А\_Борей-А})

### Flag取るために気をつけたこと

問題を解く上で気にかけていたことは以下のとおりです。

- 問題文でわからない言葉を明確にする
- その情報がありそうな言語で検索する

### 解いた流れ

まず、問題文でわからないことを潰していきました。

1. 69.216246, 33.378242 緯度経度はどっち？
2. 構造物って何？
3. プロジェクト番号って何？

1については、緯度経度を入れ替えた２パターンをGoogle Mapで検索しました。
すると、以下のリンクの通り構造物らしきものが確認できるのは一つだとわかります。

https://maps.app.goo.gl/Y5RrdqsqHhd9cDEe9

この周辺がどんな場所であるかをMap上で確認すると、Olenya Guba Naval Baseと表記された場所があるため、海軍基地の構造物の可能性があることがわかります。

海軍基地の名称をロシア語に変換して、検索すると以下の記事が見つかります。
　Google検索キーワード：Военно-морская база Оленья Губа

https://ru.thebarentsobserver.com/rossijskie-razvedyvatelnye-podvodnye-lodki-s-etoj-sekretnoj-bazy-stali-case-poavlatsa-v-rajone-globalnyh-podvodnyh-linij-svazi/228929

次に、プロジェクト番号が何を意味するかを調べました。
今回のフラグには関係ないと思いますが、ロシアの海軍に関する情報を探っていくとwikipediaのリンクが見つかります。

https://ru.wikipedia.org/wiki/%D0%A1%D0%B5%D0%B2%D0%B5%D1%80%D0%BE%D0%BC%D0%BE%D1%80%D1%81%D0%BA_(%D0%BF%D1%83%D0%BD%D0%BA%D1%82_%D0%B1%D0%B0%D0%B7%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F)

この中で、проектаというワードがproject（プロジェクト）に相当することがわかり、大型対潜艦ごとにつけられた番号のようなものが問われているということがわかります。

ここから、先程Google検索で見つけた記事のリンクの内容や周辺情報を漁っていきました（内容は省略）。この記事を読んでいくと、この問題で問われている構造物の情報が掲載されているリンクが見つかります。

http://wikimapia.org/#lang=en&lat=69.215740&lon=33.377066&z=16&m=w&show=/3311806/%D0%9F%D0%94-72

ここで、ПД-72というワードをもとにGoogleで検索すると、更に以下のリンクが見つかります。

http://militaryrussia.ru/blog/archive/military.tomsk.ru/blog/forum/topic-822.html

пр.13560というワードが出てきますが、Проект（プロジェクトの頭２文字をとったもの）とわかります。

あとは、ПД-72が構造物の名称であるかを確認した上でsubmitしました。

### Flag

Diver25{13560\_ПД-72}

## worker [Тамияс] (499pt / 12 solves)

### 問題

"object" の問題で示された構造物で仕事をしていた1964年生まれのある人物は、Akhtubinskに居住しているとされる。この人物が1987年～2009年にかけて勤務していた組織のOGRNを答えよ。
Flag形式: Diver25{1234567890123}

A person born in 1964 who worked at the object indicated in the "object" challenge is said to reside in Akhtubinsk. Answer the OGRN of the organisation where this person worked between 1987 and 2009\.
Flag format: Diver25{1234567890123}

### Flagを取るための進め方

objectからの継続問題として作成されているようだったので、object問題で気をつけたことを引き続き念頭において取り組みました。
今回の問題は、人物調査ということが文面でわかるため、まずは問われている人物を明らかにし、勤務していた組織の情報を探すという流れで進めました。

### 解いた流れ

ロシアの方である可能性が高かったため、objectで得られた情報と問題文のキーワードをロシア語に変換し、Yandexで検索しました。
　Yandex検索キーワード："ПД-72" "1964" "Ахтубинск"

すると、namebook clubというサイトでプロファイルが一致する人物の情報へのリンクが見つかります（リンクは掲載しません）。また、この情報の中にVKのリンクがあり、プロフィール上Lead moreを確認すると以下の情報が見つかります（最初に見つけたリンクでも確認できます）。

Military Service
Branch/Unit: 26273
Russia, 1987-2009

これで、問われている内容が明確になりました。
後は、以下のキーワードで検索することでFlagとなるOGRNが見つかります。
　Yandex Search Key: "Ахтубинск" "26273" "ОГРН"

https://www.rusprofile.ru/id/9757601

### Flag

Diver25{1023000507936}

## radar [blackwasan] (481pt / 45 solves)

### 問題

画像 / Image: https://drive.google.com/file/d/1hHoRFkG3ZpiQvgBN5wN9BMIkiF5WSIB1/view?usp=sharing

画像に写っているものは、福建省に存在する、中国人民解放軍東部戦区のレーダー施設であると考えられる。
公式な情報は明らかにされていないものの、オンライン上で中国の軍事を分析している人々がこの基地を運用する部隊と分隊の番号を特定しているらしい。
もちろん、オンライン上の情報だけを鵜呑みにするわけにはいかないが、ひとまず検討する価値はあるだろう。
その情報を探り当て、教えてほしい。
Flag形式: Diver25{部隊番号\_分隊番号}（例えば12345部隊の56分隊のとき、Diver25{12345\_56} となる）

The facility in the image is believed to be a radar facility in the Chinese People's Liberation Army's Eastern War Zone, located in Fujian Province.
Although no official information has been revealed, people analysing PLA on the internet have apparently identified the unit and squad numbers that operate this base. Of course, we cannot just rely on those information on the internet, but it is worth considering at least.
Flag Format: Diver25{Unit Number\_Squad Number} (e.g. If the answer is Squad 56 of Unit 12345, the flag should be Diver25{12345\_56})

![](/images/diver-osint-ctf-2025-kakitsubata/radar.jpg)
*Image source: Google Earth*

### 解いた流れ

すでにチームメンバーによって中国福建省福州市の東シナ海沿岸に存在することが特定されていたので、
「Chinese People's Liberation Army Eastern Battle Area radar site」等でGoogle検索。

その過程でアメリカ空軍大学（Air University）の中国空軍に関する調査レポートがあり、試しにのぞいてみた。

![](/images/diver-osint-ctf-2025-kakitsubata/radar-base.jpg)
*出典：CHINA’S AIR DEFENSE RADAR INDUSTRIAL BASE*

https://project2049.net/wp-content/uploads/2018/06/Stokes_China_Air_Defense_Identification_System_PLA_Air_Surveillance.pdf

図４に南方のレーダー基地指揮系統図みたいなのがあり、気になるのでソースを当たる。

引用文献23番：

- 23 “Fuzhou Base” [福州基地], Redtsai, 2 December 2019, https://redtsai.blogspot.com/2019/12/blog-post_53.html;
Jaidev Jamwal, “Chinese Armed Forces ORBAT Party 7: Radars and SAMs,” Hey, Traveler, Will You Remember
Me? [अरे यायावर रहेगा याद?], 31 August 2022, https://jjamwal.in/yayavar/chinese-armed-forces-orbat-part-7-radars-
and-sams/; Joseph.W 約瑟 (@JosephWen___), Twitter post, 23 August 2023, 10:02PM,
https://twitter.com/JosephWen___/status/1694545623968841952.


このブログURLから、下記の情報が出てくる。

福州基地 - blogspot
https://redtsai.blogspot.com/2019/12/blog-post_53.html

当該レーダーサイトを管轄していそうなのはこれだが・・・もう少し調べる。
雷達兵第4旅（94620部隊）

より調べると海軍系のレーダー部隊もあるらしい。
海軍雷達觀通部隊 - blogspot
https://redtsai.blogspot.com/2019/12/blog-post_97.html

それらのうち、この分隊が住所的にドンピシャであることが分かった。
☆東海艦隊觀通第一旅（92985部队//厦门市同安区轮山路2号）
92985部队84分队 福建省福州市長樂市下沙村

"92985部队84分队"で調べるとこんな謎の解説動画も出現する。概要欄に詳しい記載があり、読み取る。

https://www.youtube.com/watch?v=9kPH9wPPuBc

[YouTubeの概要欄]
- *解放軍 福建省 福州市 長樂區 下沙村*
- *岸島一團92383部隊*  **⇒これは本部の指揮系統？**
- *寨下反艦導彈陣地 珠山雷達站（OTH-SW)　***⇒レーダーサイトの名前①**
- *超視距表面波雷達（OTH-B)　***⇒レーダーサイトの名前②　※問題の画像に映っているもの**
- *接收站 92985部隊84分隊 下沙村基地　***⇒これが現地の分隊**

ちなみに、随所で引用されているXアカウントがあり、現地情報筋？と思われるほど、人民解放軍の基地情報を提供していた。

https://x.com/JosephWen___

### Flag

Diver25{92985_84}

# report

## unknown_aircraft (300pt / 1 solves)

### 問題
記述問題
これらは2025年5月10日 19:00UTC ごろに Flightradar24 上で確認された航空機の様子です。
画面上で選択されている航空機（赤いアイコンの航空機）について、公開情報から分かる範囲で、調査を行って説明してください。
記述に関する詳細は添付しているルール（PDF）を参照してください。

Descriptive Form Challenge
These are the aircraft identified on Flightradar24 at 19:00UTC on 10 May 2025.
Please investigate and describe the aircraft selected on the screen (aircraft with red icons), as far as you can tell from the publicly available information.
See attached rules (PDF) for further information on the description.

![](/images/diver-osint-ctf-2025-kakitsubata/unknown_aircraft_01.jpg)
![](/images/diver-osint-ctf-2025-kakitsubata/unknown_aircraft_02.jpg)

### 感想（簡易版）
medium難易度の問題を完答したら開放されることが事前にアナウンスされていた記述式問題。渤海（中国・山東半島の北西の湾）でFlightrader24の画面に表示された6つの航空機の正体を突き止めるというものでした。

他の問題を解き終えたときにはすでに深夜で全員眠く、記述の調査をできるテンションではなかったため各自睡眠をとり、翌朝起きた人から調査をゴリゴリ進めて共有メモに書いていく作戦をとりました。最後の1時間でレポートをGoogle Documentにまとめ、PDFとして提出しました。

結果は300点満点の200点。部分点があるのはありがたいですね。レポートのまとめ自体がギリギリになり、最後の結論の部分で調べが足りない部分があるなあと思いながら提出したので納得の結果です。

### 提出したレポート
[KAKITSUBATA_unknown_aircraft.pdf](https://github.com/kn1cht/kn1cht-zenn-contents/tree/main/images/diver-osint-ctf-2025-kakitsubata/KAKITSUBATA_unknown_aircraft.pdf)
（PDFファイル）

本問の調査はそれだけで普通の問題数問分くらいの分量になるため、本記事では調査過程を取り扱いません（今後公開するかもしれません）。皆さんもぜひ調べてみてください！
