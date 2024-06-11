---
title: "[KAKITSUBATA] Diver OSINT CTF 2024 Writeup"
emoji: "🤿"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - OSINT
  - CTF
  - writeup
published: false
---

# 開催概要・制度
- 2024/06/08 (土) 12:00JST - 06/09 (日) 12:00 (JST)（24時間）
- Jeopardy方式
- 問題文は日本語と英語
- 問題はwelcomeを正答した時点からほぼ全て解放された（一部の連続した問題にはunlockが存在した）
- CTFdに**1チーム6人まで**参加登録し、チーム戦を行った
- 各問題の点数は、**参加者全体の正答数によって変動**した
    - 例えば、ある問題を解いた人が非常に少ない間は500点が得られても、その後正答チームが増えると点数が減少した
    - これによって、より難しい問題を多く解けたチームが上位に入れる仕組みになっていた
- Introduction, easy, medium, hardの4つの難易度が用意された。特に、introductionレベルはOSINT CTFの入門になるような問題として作られた
- 公式によると、842名のプレイヤーと484チームが参加したとのこと

# 結果
5名（meow_noisy, blackwasan, `_roku_`, kuzushiki, kn1cht）でチーム”KAKITSUBATA[^1]”を結成し、参加しました。
**2日目の11:12までに全問（35問）を解き、1位となりました。**

[^1]: チーム名の由来について: 日本のCTFなので和風な響きの言葉にしようということで、ちょうど仲夏の時期のCTFだったことから、仲夏の季語かつ「幸せは必ず来る」という縁起の良い花言葉を持つカキツバタを選びました。


![チームの結果。11261点で1位。](/images/diver-osint-ctf-2024-kakitsubata/diver24-team.png)
![順位表](/images/diver-osint-ctf-2024-kakitsubata/diver24-scoreboard.png)

# Writeup
本記事はkn1chtのアカウントで公開しますが、各問題ごとにflagを入力したプレイヤーが執筆しました。見出しに問題名、難易度、執筆者の名前、点数、解けたチーム数を入れています。複数名で正答に貢献した問題の場合は、複数の名前を書いている場合があります。

見出しに示した点数は、CTF終了後のものです。また、問題サーバーから画像等を引用しています。

# welcome
本カテゴリの難易度は全て”welcome”。

## welcome [meow_noisy] -10point 453 Solves

>DIVER OSINT CTFへようこそ！
>ルールのページからFlagを見つけて入力してください。
>正解すると、ほかの問題がアンロックされます！
>
>Welcome to the DIVER OSINT CTF!
>Please find the flag on the rules page and enter it here.
>You will see the other challenges when you submit the correct flag.

これを解かなければ他の問題が表示されない。
ルールページ内の一番最後にフラグが掲載されている。
https://ctfd.diverctf.org/rules

`Diver24{ganbarou!}`

# introduction
本カテゴリの難易度は全て”introduction”。

## 246 [blackwasan] - 100point 143 Solves

>画像が撮影された場所から最も近い交差点名を答えよ。
>Flag形式: Diver24{交差点名}
>
>What is the name of the intersection closest to where the image was taken?
>Flag format: Diver24{intersection name}
>
>![](/images/diver-osint-ctf-2024-kakitsubata/246.jpg)

問題名が「246」なので、天下の国道246号の話と思いきや、県道246号。民家に鎮座した白アクアと、はるか先に見える吊り橋が写っている。白アクアのナンバーで地域名を絞ろうとするが微妙に読めない・・・千葉？

![](/images/diver-osint-ctf-2024-kakitsubata/246-car.png)

とりあえずナンバーは放っておいて、吊り橋がかかるということはなんとなく地形が険しい海沿いっぽいので、~~リアス式~~ リアス海岸や小島が含まれる県（岩手・三重とか瀬戸内あたり）を探ることに。47個調べるよりは労力が少なくて済む。

県道の番号と色が見やすいので、マピオンなどを使うと探しやすい。
https://www.mapion.co.jp/

そうすると九州と本州を結ぶ「関門橋」にほど近い、山口県下関市の長府エリアに山口県道246号を見つける。「関門橋」というと、なんとなく門司港あたりのキラキラしたイメージがあったが、反対側の長府からだとこういう景色もありなんということで、「右に曲がるとこの県道に入る」地点を探していくとビンゴ。

![](/images/diver-osint-ctf-2024-kakitsubata/maeda.jpg)

そういえば、この関門地域では新たな架橋計画が具体化に向け動き出している（下記ニュースなど）ので、動向を追っていきたい。
https://news.yahoo.co.jp/articles/50b98cd45fc15916ec880af8b93ee03b869e8658

`Diver24{前田}`

## office [kn1cht] - 100 point 229 Solves
>ファイルから得られるFlagを入力してください。
>Input the flag obtained from the file.


mudai.odtというファイルが渡される。Microsoft Officeが入っている環境で開くとWordアイコンが出ることからもわかるが、ODTファイルはOpenOfficeなどで使われている文書フォーマット。

![](/images/diver-osint-ctf-2024-kakitsubata/mudaiodt.png)

Officeファイルが渡されるCTFの定番テクニックとして、**拡張子をZIPに変えて解凍**すると中身が見えるというものがあるので、mudai.zipとしてあげると中身が見える。
中のファイルを見ると、Thumbnails/thumbnail.pngにFlagが書かれているので、書き写す。

![](/images/diver-osint-ctf-2024-kakitsubata/thumbnail.png)

`Diver24{World Ocean Day}`

## chain [kn1cht] - 100point 110 Solves
>この看板の店の電話番号を答えよ。
>Flag形式: Diver24{0123456789}
>注意: 電話を掛けてはならない。Web上の情報から調査せよ。
>
>Answer the phone number of this restaurant.
>Flag format: Diver24{0123456789} Note: DO NOT MAKE ACTUAL PHONE CALL. Search on the web.
>(Hint) As the flag format shows, it should be the Japanese domestic phone number format.
>(3 attempts)
>![](/images/diver-osint-ctf-2024-kakitsubata/chain.jpg)

日本の焼き鳥チェーン店である鳥貴族の看板の写真が与えられる。鳥貴族は日本全国に600店舗以上あるらしく、総当たりで解くことは困難（実際に、チームメイトが4階にある鳥貴族を1つ1つ見て回ってくれたが、正解には至らなかった）。

ところで、この写真に写っているのは雑居ビルの店舗を示すパネルだと思われるが、鳥貴族以外が空白なのは通常では考えにくく、どこか廃れた印象を受ける。
おそらく、このビルは閉店が相次いでいて、鳥貴族自体も閉店してしまって存在しないのではないか？と推理した。

そこで、Xで「鳥貴族 閉店」を検索した。なお、この画像の**EXIF情報に2024年2月4日という日付がある**ので、そのあたりの期間を狙う。

https://x.com/search?q=%E9%B3%A5%E8%B2%B4%E6%97%8F%20%E9%96%89%E5%BA%97%20until%3A2024-02-25&src=typed_query&f=live

鳥貴族 広尾店が閉店したという声がいくつかあるので、Googleで画像検索すると問題にそっくりの場所の写真が出てくる。

https://www.cookdoor.jp/dtl/00000000000080001878/

このページに電話番号も書かれているので、Flag formatに合わせて入力すると正答。

`Diver24{0364593392}`

## dream [kuzushiki] - 100point 255 Solves
>画像に写っているパイプオルガンがある施設の郵便番号を答えなさい。
>Flag形式: Diver24{123-4567}
>
>Give the postal code of the facility with the pipe organ shown in the image.
>Flag format: Diver24{123-4567}
> ![](/images/diver-osint-ctf-2024-kakitsubata/dream.jpg)


写真から場所を特定したいので**Google Lens**を使ってみる。Google Lensは写真の特定箇所にフォーカスをあてて検索することができる優秀なツールである。以下のようにオルガンを囲った状態で検索すると、検索結果のトップに**ポップタウン住道オペラパーク**が出てきた。

![](/images/diver-osint-ctf-2024-kakitsubata/dream-lens.jpg)


あとは施設名をググって郵便番号を提出した。

`Diver24{574-0046}`

施設名をそのまま答える形式でも良かったと思うが、郵便番号をフラグにしたのは回答の表記揺れを防ぐためだろうか？

## serial [kn1cht] - 100point 223 Solves

>これらの動画の背景に映っている航空機のシリアル番号は何か？ シリアル番号が123456の場合、Flagは Diver24{123456} となる。
>
>What is the serial number of the aircraft in the background of these videos? If the serial number is 123456, the flag will be Diver24{123456}.
>
>https://www.tiktok.com/@ana_allnipponairways/video/7318648417620741377
>https://www.tiktok.com/@ana_allnipponairways/video/7338422699301145857

着ぐるみが航空機の前で踊っているTiktokの動画が与えられる。一時停止しながらよく見ると、**JA222A**という機体番号が読み取れる。

![](/images/diver-osint-ctf-2024-kakitsubata/ja222a.png =250x)

JA222A自体はregistration number（通称：レジ番）と呼ばれるもので、シリアル番号ではない気がするので探す。Flightrader24ではSERIAL NUMBER (MSN)の項目は会員しか見られない状態だったが、別のサイトにManufacturer Serial Number (MSN)として書いてあるのを見つけてフラグを得る。

https://www.planespotters.net/airframe/airbus-a320neo-ja222a-all-nippon-airways/3v6npy

`Diver24{9580}`

## ad_directiare [meow_noisy] - 257 point 79 Solves
>この名刺の人物が、東京出張時に食べた昼ご飯の値段を答えよ。
>Flag形式: Diver24{値段}
>1000円の場合、Flagは Diver24{1000} となります。
>
>Answer the price of the lunch this person on the business card had on a business trip to Tokyo.
>Flag format: Diver24{price}
>If it is 1000 JPY, the flag should be Diver24{1000}
>![](/images/diver-osint-ctf-2024-kakitsubata/ad_directiare.jpg)

まず、タイトルの意味の調査。

![](/images/diver-osint-ctf-2024-kakitsubata/addirectiare-word.png)

あまり関係がなさそう。

とりあえず名刺に書かれている情報(よねくらデザイン事務所、yone.junなど)で検索するが、オフィシャルな情報がヒットしない。

gmailアカウントがあるので反射的にEPIEOSを実行。

![](/images/diver-osint-ctf-2024-kakitsubata/addirectiare-mail.jpg =520x)


google mapで投稿している。問題用の架空のアカウントのようだ。

https://www.google.com/maps/contrib/104607974422086075165

![](/images/diver-osint-ctf-2024-kakitsubata/addirectiare-map.jpg =300x)

ご飯の投稿内容を確認。値段がわかる。

![](/images/diver-osint-ctf-2024-kakitsubata/addirectiare-review.jpg =300x)

フラグ
`Diver24{4400}`

# crypto
## leak (easy) [kn1cht] - 100point 211 Solves
>先月、日本の会社で大規模なBTCの不正流出が起きた。
>流出先となっているウォレットアドレスを答えよ。
>Flag形式: Diver24{ウォレットアドレス}
>
>Last month, there was a large-scale unauthorized outflow of BTC from a Japanese company.
>Identify the wallet address where the outflow went.
>Flag format: Diver24{wallet address}

言及されているのは、日本のネット上で大きな話題となったDMM Bitcoinの事件。

https://www.coindesk.com/business/2024/05/31/japanese-crypto-exchange-dmm-bitcoin-suffers-305m-hack/

Xなどで詳しい人が解説してくれているので、流出先のアドレスをコピペ。

@[tweet](https://x.com/full_investdfx/status/1796537346558726234)

`Diver24{1B6rJRfjTXwEy36SCs5zofGMmdv2kdZw7P}`

## Akeome (hard) [kn1cht] - 500 point 4 solves
>以下の人物が2017年に始めたyoutubeチャンネルを見つけてください。
>Find youtube channels started in 2017 by the following persons.
>https://bitcointalk.org/index.php?action=profile;u=44692
>
>Flag例 / Flag example: Diver24{@RickAstleyYT}
>
>(Hint) 仮想通貨の取引記録を調査する必要はない / No need to investigate the transaction history of cryptocurrency

渡されたBitcoin TalkのURLからいくつかのページを見るとこの人物が**Bitcoin Fogという暗号通貨ミキシングサービス**（暗号通貨の取引をランダムに混ぜるため、取引の追跡を難しくする技術）を運営していたことがわかる。
また、Google検索すると、Bitcoin Fogの運営者として資金洗浄に関わった疑いで**Roman Sterlingov**という人が2021年に逮捕されたことが分かる。

https://protos.com/bitcoin-fog-crypto-mixer-arrested-money-laundering-google-helped-feds/

Bitcoin Fogの宣伝に使われたTwitterアカウント（@BitcoinFog）は今でも閲覧できるが、各所で検索してもBitcoin Fogの運営を名乗るようなYouTubeアカウントは見つからなかった。

そこで、**Bitcoin Fogとは関係なく、個人的な活動でYouTubeを利用**していたのではないかと考え、Sterlingovの個人情報がインターネット上にないかどうか探した。すると、逮捕に関する記事の中で、Bitcoin Talkへの登録に使われたメールアドレス `shormint @ hotmail.com` を捜査当局が見つけたという話が出てくる。
このアドレスで検索すると、米国の捜査当局が作成したBitcoin Fog事件に関するスライド資料（https://ia904506.us.archive.org/34/items/gov.uscourts.dcd.232431/gov.uscourts.dcd.232431.189.1.pdf ）が出てくる。この資料は、捜査員が匿名で活動しているBitcoin Fogの運営者にたどり着く過程を説明していてCTF関係なく面白いので、ぜひ一読してほしい。

この資料には、Sterlingovが使用した疑いのあるメールアドレスとして`heavydist @ gmail.com`や`plasma @ plasmadivision.com`など6個ほどが登場する。メールアドレスからYouTubeアカウントが判明する可能性を考え、EpieosやGHuntにそれぞれを入力した。しかし、いくつかは実際にGoogleアカウントであることが分かっただけでYouTubeの情報は得られない。

2日目の6月9日の朝を迎え、チーム全員で調査しても進展が見られなかった。何か確認しそこねているキーワードがあるのではないかと考え、ふと先程の捜査資料スライドの上にある **Case 1:21-cr-00399-RDM** をGoogle検索した（米国において訴訟に付けられる番号らしい）。検索の上位に出てくるCourtListenerというサイトには、Sterlingovの裁判で使われた資料の多くがPDFとText形式で公開されている。

https://www.courtlistener.com/docket/59988850/united-states-v-sterlingov/

司法手続きが進む中で、YouTubeが言及されたことがあったのではないか？と考え、全部で400件くらいある資料の中から証拠を示していそうなものを気合で上から読んでいった。すると、”Supplemental MOTION for Release of Funds by ROMAN STERLINGOV“（Sterlingovによる資金を解放する追加の動議）という資料の中でYouTubeという文言が出てくる。

https://www.courtlistener.com/docket/59988850/101/1/united-states-v-sterlingov/

>27. We ran the studio and made these videos for 2-3 years. In 2017, we started putting videos
>    up on YouTube under the name “Technology of Winning” @technologyofwinning8592:
>    https://www.voutube.eom/@technologvofwinning8592/videos

前後の内容を読むと、Sterlingovが自分の過去の活動をふりかえり、Bitcoin Fogとは関係ありませんということを主張するものだった。
すなわち（この証言が正しいと信じるとして）このアカウントが答え。

`Diver24{@technologvofwinning8592}`

裁判資料に気づくまでメールアドレスの調査やSNSの調査など様々な選択肢があり、このCTFの中でも有数の時間のかかる問題だったと思います。その分、気づいたときは爽快でした。

# geo
## imagetrack (easy) [blackwasan] - 100point 146 Solves
>画像の撮影された郷土料理屋の店名を答えよ。
>Flag形式: Diver24{店名}
>
>Please provide the name of the local cuisine restaurant where the image was taken.
>Flag format: Diver24{restaurant name}
>![](/images/diver-osint-ctf-2024-kakitsubata/DSC_8886.jpg)

EXIFを確認。

![](/images/diver-osint-ctf-2024-kakitsubata/imagetrack-exif.png)

この場所をGoogleMapsに入れると、三宮駅の北側にピンが刺さる。

https://maps.app.goo.gl/AQVcsQSqggiNGeGz8

ちょうどその近くに郷土料理屋を名乗る店があるので、メニューを見ると「open air」取り扱いあり。

ちなみに「open air」は神戸発の地ビールみたいなので、飲んでみたいですね（おいしそう）。

オープンエア湊山醸造所

https://www.openair.beer/

![](/images/diver-osint-ctf-2024-kakitsubata/imagetrack-karasu.jpg)

`Diver24{郷土料理 からす}`

## chiban (medium) [blackwasan] - 184 point 90 solves
>画像の中央に写っている道路の地番は何か。
>たとえば住所が 仙台市宮城野区二十人町 303-8 の場合、Flagは Diver24{303-8} となる。
>また、地番には数字だけでなく漢字が含まれることもある。その場合は漢字表記のまま解答せよ。
>
>What is the land lot number (地番, chiban) of the road in centre of image? Answer in Japanese.
>For examle, if the address is 仙台市宮城野区二十人町 303-8, the flag should be Diver24{303-8}.
>Also, some land lot number includes not only numbers but also Kanji (Chinese characters). In that case, please answer as is in Kanji.
>![](/images/diver-osint-ctf-2024-kakitsubata/chiban.jpg)

サンディといえば関西の激安系？スーパー（筆者が関西在住経験ありのため、たまにお世話になっていた）。写真の「7号店」という文字が気になるのでググる。

TODO: image

どうやらサンディは出店順の号数を出しているらしい。マイナビパートの募集に双葉店（大阪府茨木市）が7号店である旨の記載がある。

![](/images/diver-osint-ctf-2024-kakitsubata/chiban-google.jpg)

Google Mapsで確認するとここの道路が写真に写っている 。
https://maps.app.goo.gl/UTTu7B3YEtiRAp3c9


「画像の中央に写っている道路」が奥か手前か怪しいが、筆が入り組んでいそうなので、mapple法務局ビューア（https://labs.mapple.com/mapplexml.html）で確認 。土地の登記を地図上で見れるツールなので、権利関係の調査に便利。

![](/images/diver-osint-ctf-2024-kakitsubata/chiban-mapple.jpg)

筆界未定地とは、様々な経緯で土地の境界が定まっていない土地のこと。写真では新しめの公道になっているが、元々は道路ではない何かだったのだろうか。

`Diver24{筆界未定地-6}`


## championships (medium) [kuzushiki] - 440 point 40 solves
>2010年10月、この場所には大会の広告が貼られていた。その大会の優勝者のニックネームを答えよ。
>Flag形式: Diver24{優勝者のニックネーム}
>
>In October 2010, an advertisement for a competition was placed at this location. Give the nickname of the winner of that tournament.
>Flag format: Diver24{winner's nickname}.
>![](/images/diver-osint-ctf-2024-kakitsubata/championships.jpg)

場所は一目瞭然で**光華商場**だとわかる。光華商場は台湾・台北にある電気街（日本で言うアキバ）だ。大会の情報はどうやって探せばよいだろうか。

問題文でニックネームを聞かれていることが気になった。本名でないということはゲームの大会か何かではないだろうか？Googleで「before:2011」「大会」「優勝」などのキーワードを組み合わせて検索するも、それらしき大会は見当たらず。広告の情報を探そうとYoutubeなどの動画サイトも漁ったが空振りに終わった。

万策尽きたかに思われたが、ここで**Google mapのタイムマシン**機能を思い出す。タイムマシンを使えば10年前の広告も見れるのでは？と思い付近の情報を調べてみると、2010年10月当時の写真が残っていた。以下はチームメイトのrokuさんが共有してくれたスクショである。

![](/images/diver-osint-ctf-2024-kakitsubata/championships2010.jpg)

画像中央にGIGABYTEの広告が見える。大会について調べるとGO OC 2010が台湾で開催されていることがわかった。

https://www.gigabyte.com/Press/News/929

優勝者を確認したいところだが記事内のURLにアクセスできない。大会ページはすでに使われなくなっているようだ。

http://gooc2010.gigabyte.com/

Wayback Machineで調べたところ、当時のページを閲覧できた。

https://web.archive.org/web/20110316103019/http://gooc2010.gigabyte.com/

`Diver24{Matose}`

筆者は2010年当時台湾に住んでいたが、この大会のことは全く知らなかった。見に行っていればこの問題に即答できたのに...

## power (medium) [blackwasan] - 475 point 26 solves
>写真右側に写っている送電線の運用容量値（単位: MW） はいくつだろうか。整数で答えよ。
>なお、この写真は2024年に撮影されたものである。
>Flag形式: Diver24{100}
>
>What is the operational transmission capacity value (unit: MW) of the transmission line on the right-hand side of the picture? Answer >in whole numbers.
>Note that this photo was taken in 2024.
>Flag Format: Diver24{100}
>
>(Hint) "operational transmission capacity value" is "運用容量値" in Japanese
>(5 attempts)
>![](/images/diver-osint-ctf-2024-kakitsubata/power.jpg =300x)

写真の右側に映る送電鉄塔。ここの運用容量値（整数値）を答える問題。運用容量値とは、一般送配電事業者が定めている各送電線を安定的に運用するための潮流の上限容量。ざっくり言うと壊れない範囲で流してもいい電力の量みたいなもの。
5回の解答数制限があるので、数字のブルートフォースは排除されている。

電気の話は置いておいて、中央の電車は683系サンダーバードである。ということは、JR西日本の京都線から湖西線あたりの景色か。

線路沿いの送電線を漁りたいので、有用なツールがある。環境省のEADAS（https://www2.env.go.jp/eiadb/ebidbs/ ）というデータベースには、正確な系統マップを表示できる機能がある。本来は再エネ事業者向けのデータベースだが、使い勝手が良い。これを使って大阪から滋賀あたりまで、サンダーバードの経路と平行する送電線はないか探す。

>![](/images/diver-osint-ctf-2024-kakitsubata/power-eadas.jpg)
*EADASによる系統マップ*

そうすると、JR高槻駅付近とJR桂川駅付近にそのような場所があることが分かり、後者のJR桂川駅付近のストリートビューが問題の画像と一致。そういえば、写真を見たときに妙な既視感があったのだが、視点の後ろ側には京都でも有数のデカさを誇るイオンモール京都桂川があり、筆者が関西在住時よく行っていたからだった・・・。

https://maps.app.goo.gl/FHMUZj5EaCoaYqqh8

関西電力送配電HPの系統空き容量マッピング資料のページに、送電線の地図（若干簡略化されている）と空き容量情報があるので、該当する送電線を探す。空き容量情報の方に、送電線の運用容量値を含む情報が一括で掲載されている。

https://www.kansai-td.co.jp/consignment/disclosure/distribution-equipment/mapping.html

"京都市 [747.71KB]"のリンクから京都市のマッピングの系統図を閲覧できる。PDFは転載禁止なので画像は各自で確認してほしい。

「北172」という送電線（77kV）が問題のそれに該当する。「西院線」と呼ばれているらしい。

PDFの表から運用容量値（MW）を探して回答。

`Diver24{84}`

## island (medium) [kn1cht] - 479 point 24 solves
>「四方ぎり島」という名前の島における、最高地点の標高を整数（メートル）で答えよ。
>Answer the highest elevation of the island named "四方ぎり島" in integer and metric.
>Flag例 / Flag example: Diver24{3776}
>(5 attempts)

"四方ぎり島"でGoogle検索すると、指定された島が南極にあることがわかる。読み方は「よもぎりじま（Yomogiri Zima）」 。
南極地名委員会報告（https://nipr.repo.nii.ac.jp/record/8622/files/KJ00000139872.pdf ）というPDFファイルを開くと、座標が **69°43'02"S 38°58'23"E** と記されている。

日本は南極観測を行っており、地形の測量や地図作成も行っている。その成果は、例えば国土地理院の「南極の地理空間情報」にまとまっている。

https://www.gsi.go.jp/antarctic/index.html

座標と地図を照らし合わせながらしばらく探す（日本の観測隊の拠点である昭和基地からは少し離れているので、縮尺の小さい地図を見る必要がある。
すると、”1/25,000画像化データ新”と呼ばれているものが見つかり、これまた座標をしっかり見ながら探していくと、”1:25,000 南極地形図 222 エインストーインゲン”が見つかる。

https://www.gsi.go.jp/antarctic/viewer_03_25000_new.html

四方ぎり島の各地点の標高が書いてあるので、一番大きいものがFlag。

`Diver24{18}`

なお、他のプレイヤーの方によるとWeb版の地理院地図（GSI Map）をうまく使っても表示できるらしい。

https://maps.gsi.go.jp/#14/-69.732398/38.982410/&ls=southpole_2500%7Csouthpole_25000%7Csouthpole_50000_2%7Csouthpole_satellite_250000%7Csouthpole_2500_ort_2%2C0.61%7Csouthpole_250000&disp=110111&lcd=southpole_250000&vs=c1g1j0h0k0l1u0t0z0r0s0m0f1&d=m

## construction (hard) [blackwasan] - 496 point 12 solves
>あるアナリストによると、37.669, 120.691 付近に新たな施設が建設されたことが判明した。この施設の名称を答えよ（現地の公用語表記）。
>例えば、韓国にある「青瓦台」が答えの場合、現地語表記を用いて Diver24{청와대} がFlagとなる。
>
>One analyst found that a new facility was built in the vicinity of 37.669, 120.691. Give the name of this facility (in the local >official language).
>For example, if the answer is "Cheongwadae" in South Korea, the Flag is Diver24{청와대} using the local language notation.


**meow_noisyによる探索作業：**
この座標を確認すると、中国山東省の蓬莱市の耕作地帯にピンが差さる。少なくともGoogleMapsの航空写真上では何も写っていない。

![](/images/diver-osint-ctf-2024-kakitsubata/construction-image.jpg =400x)

そもそもアナリストとは誰なのだろうか、軍事アナリスト？
中国の検索サイトBaidu（百度）のBaidu mapでも特段記載はない。

https://map.baidu.com/@13434352.981476534,4506206.633249341,17z/latlng%3D37.667502587230004%252C120.68153304267139

欧州宇宙機関（ESA）が提供する衛星画像ブラウザ「Sentinel Hub EO Browser」で確認すると、 滑走路のようなものを確認。

https://apps.sentinel-hub.com/eo-browser/

![](/images/diver-osint-ctf-2024-kakitsubata/construction-sentinel.jpg)

ここで、kn1chtが中国の航空基地と民間空港のリストマップを見つけてきた。

All Chinese Airports & Airbases
https://earth.google.com/web/@30.89056075,85.19814171,4756.63406472a,7731118.08079372d,30.00000233y,0h,0t,0r/data=MigKJgokCiAxSXZrZnJFVVNRQ3BoN0x4TjB2RDdXOWhCUnVZd01jayAC

しかし、当該の場所にピンはなかった。


**ここからblackwasan：**
滑走路が写っているということは、新設の空港か航空基地だろうということで、Baiduにて現地語で思いつくままに検索しまくっていたところ、「蓬莱市 建造 机场（日本語：蓬莱市 建設 空港）」で下記の画像がヒット。 形もSentinelに写っていたのとそっくり。

![](/images/diver-osint-ctf-2024-kakitsubata/construction-baidu.jpg)

右側のタイトルに「蓬莱通用机场（日本語で蓬莱総合空港の意） 」とあり、これがFlagだった。

`Diver24{蓬莱通用机场}`

## public_service (hard) [blackwasan] - 497 point 10 solves
>ある男が「20年前の11月だったかな、この交差点について市に文句を付けてやったことがあるんだよ。最近の行政は酷いもんだがね、あの頃は良かったよ。」と言っていた。
>彼は何を連絡したのだろうか。資料に記載されている通りの理由を答えよ。
>Flag形式: Diver24{Something happend}
>
>One man said, "I once complained to the city about this intersection, 20 years ago, I think it was November. The administration is terrible these days, but those were good days."
>What did he contact them about? Answer why, as stated in the documents.
>Flag Format: Diver24{Something happend}
>![](/images/diver-osint-ctf-2024-kakitsubata/publicservice.jpg)

※前半はチームメイトによる手助けを大いに含みます、ありがとう！

場所はニューヨークのとある交差点。46 Avenueと111Streetが交わる交差点のようだ。

Google Maps
https://maps.app.goo.gl/sTj3Ych6iwqzonEx5

ストリートビュー
https://maps.app.goo.gl/9o4L74HT9RDnDx7FA

「“111 th Street" newyork」などでググっていると以下のファイルが見つかる。111th Streetの改善計画が書かれたものだが、2015年のもので少し新しい。一応目を通すも、「2004年に不満を言っていた」ような記述は見当たらない。

ニューヨーク政府 (nyc.gov)の公式サイトに、「〇〇 Complaint」みたいなページがいくつかあり、更にサイトを巡回していると、「Complaint Statistical Report」なるものが発行されていた。クレームを受理した日とクレームのカテゴリIDが記載されているのでこれを参考にすれば解けそう？ と思ったが、このレポートも2015年から発行されているので違うようだ。

- https://portal.311.nyc.gov/kacategory/?id=311-81

「CIVILIAN COMPLAINT REVIEW BOARD（CCRB）」というのもあり、専用のHPを持っている。

- https://www.nyc.gov/site/ccrb/index.page


こちらは古いものも残っていそうで、アーカイブサイトも発見。

- https://www.50-a.org/complaint/200401917
- https://www.50-a.org/command/110pct


一通り調べてみるも、特に有用な情報は見つからなかった・・・というか警官へのクレームだった 。
なにかがおかしい、と思いCCRBの役割を調べたところ、警察の不正を調査する団体だった。 どうやらCCRBを調べてもフラグにはたどり着けなさそう。

再び振り出しに戻り、「new york complaint database 2004」などで検索していると、ニューヨーク市への苦情履歴データを表示できるサービスがあった。

https://data.cityofnewyork.us/Social-Services/311-Service-Requests-for-2004/sqcr-6mww/data_preview

2004年に該当するものは約110万件あり、そのレコードを100件ずつ見ていくのはとてもつらい。該当ページの右上に「Action」ボタンがあり、そこから全データをCSV（約532MB）としてダウンロードできる。 このデータには問い合わせの記録日（CreatedDate）や対象の場所（Street name）などとともに苦情内容（Descriptor）が記録されているが、さすがに目grepは不可能なので、grepしてみる。

TODO: image

「CrossStreet1」「CrossStreet2」または「Intersection Street1」「Intersection Street2」にそれぞれ”46 AVENUE” ”111 STREET”が記録されているものを炙り出したい。

```bash
$ cat 311_Service_Requests_for_2004_20240609.csv | grep "46 AVENUE,111 STREET" > grep.csv
```

レコードが3つ出てきたが、記録日が11月ではないものだった。一応Flagを試してみるが、Incorrect。
そこで、少し甘めに”46 AVENUE”と”111 STREET”がとりあえずレコードのどこかに含まれていればよいことにする。

```bash
$ cat 311_Service_Requests_for_2004_20240609.csv | grep "46 AVENUE" | grep "111 STREET" > grep2.csv
```

そうすると、Unique ID:”1219841”のレコードに111Street 46Avenueについて寄せられた苦情（2004/11/15付）が見つかる。

`Diver24{Street Light Out}`


# history
## promoter (easy) [meow_noisy] - 184 point 90 solves

>画像に写っている池沼を開拓した発起人の父親の命日を答えよ。
>Flag形式: Diver24{yyyy/MM/dd}
>
>The pond in the image was cultivated by a certain person who initiated the development in 1628 (Kan'ei 5, 寛永5年). Give the date of >death of this person's father.
>Flag Format: Diver24{yyyy/MM/dd}
>Note: "Kan'ei" (寛永) is one of Japanese era name. (wikipedia)
>![](/images/diver-osint-ctf-2024-kakitsubata/promoter.jpg)

exif確認→手がかりはない。

Google レンズ
![](/images/diver-osint-ctf-2024-kakitsubata/promoter-meijimura.jpg =320x)


明治村(愛知県)が提案される。検証して、似たような光景をアップロードしているブログや、中央にある球状の物体(テントとのこと)から、明治村であると確定。
この池は入鹿池という池らしい。

ダメだったアプローチ:
入鹿池のwikipediaに"甚九郎"という名前の人物が見つかったので、その父親である佐久間信盛の命日を入れたがincorrect。

正解のアプローチ:
"入鹿池 開拓"で検索すると次のソースが見つかる

https://suido-ishizue.jp/nihon/12/06.html

それによると、
>提唱したのは江崎善左衛門[ぜんざえもん]など地元の郷士（戦国浪人）六人衆。

6人いるとのことだが、まずは、江崎善左衛門について調査する。
https://ja.wikipedia.org/wiki/入鹿池#入鹿六人衆
> 父江崎善左衛門宗度（享禄3年（1530年） - 寛永4年（1627年））

年までは書かれている。

"江崎善左衛門 宗度 1627" で検索すると、次の資料がヒット。月と日がわかる。
https://www.jstage.jst.go.jp/article/jjsidre1965/49/7/49_7_630/_pdf

>寛永4年 (1627)11月13日 に病没 した。

`Diver24{1627/11/13}`

## paddy (medium) [meow_noisy] - 494 point 14 solves

>この地域では地形の人工的な大規模改変が2回行われたという。郡司としてこれにかかわった人物の名は？
>日本語表記で答えよ。
>Flag形式: Diver24{人名}
>
>The area has undergone two large-scale man-made alterations to the terrain. What is the name of the person involved in this as a >local governor (郡司)?
>Answer in Japanese (Kanji).
>Flag format: Diver24{person's name}
>
>[For non-Japanese speakers]
>・Japanese text can be written either vertically or horizontally.
>・Historical Japanese is written either horizontally from right to left or vertically. Some old characters (旧字体, Kyūjitai) are >also used.
>
>(Hint) Monument speaks.
>![](/images/diver-osint-ctf-2024-kakitsubata/gsimap.png =300x)


_roku_さんが地域を特定してくれた。石巻市周辺である。最寄り駅は「佳景山」駅。

![](/images/diver-osint-ctf-2024-kakitsubata/paddy-area.png =300x)

blackwasanさんが、ヒントにある「Monument speaks.」から、地図記号のモニュメントを見つけて読むのがよさそうという推測を行い、記念碑、または、自然災害伝承碑あたりを探し、「佳景山」駅近くに記念碑があることを発見した。

![](/images/diver-osint-ctf-2024-kakitsubata/paddy-monument.png =250x)

しかし、記念碑の画像が見つからなかった。

Meow_noisyが調査に参加。写真の区間が何と言う地域なのかを知りたいと思った。
しかし、google 航空写真を確認しても、明確な区間はなさそうである。

![](/images/diver-osint-ctf-2024-kakitsubata/paddy-image.jpg =350x)

別の地図サービスで書かれていないか調べるために、[OpenSwitchMaps Web](https://tankaru.github.io/OpenSwitchMapsWeb/index.html)で地図を切り替えながら、地名を調べていた。

https://tankaru.github.io/OpenSwitchMapsWeb/index.html

「今昔マップ」で、この領域に昔、名前がついていたことがわかった。
https://ktgis.net/kjmapw/kjmapw.html?lat=38.493964&lng=141.224635&zoom=15&dataset=tohoku_pacific_coast&age=0&screen=2&scr1tile=k_cj4&scr2tile=k_cj4&scr3tile=k_cj4&scr4tile=k_cj4&mapOpacity=10&overGSItile=no&altitudeOpacity=2

![](/images/diver-osint-ctf-2024-kakitsubata/paddy-kjmapw.jpg)

"沼縁廣"で検索したが、ヒットしない。英語の問題文の方にあった右から左に読むという補足説明を思い出し、"廣淵沼" で検索。

宮城縣廣淵沼沿革史 なるソースがあった。

![](/images/diver-osint-ctf-2024-kakitsubata/paddy-numa.jpg =550x)

https://books.google.co.jp/books?id=kAIIGsZEo8AC&printsec=frontcover&hl=ja#v=onepage&q&f=false

書籍内を"郡司"で検索すると、郡司の名前と、1回この場所を工事して2回目でテコを入れたようなことが書かれている。

郡司の名前でサブミットし、correct

`Diver24{山崎平太左衛門}`

## protest (hard) [kuzushiki] - 499 point 8 solves
>23歳の環境保全運動家の殺害場所となった道路の現在の路線番号は何か。
>フラグ形式：Diver24{路線番号}
>注意：殺害に関わる直接的な画像が表示される場合があります。
>
>What is the current road ID of the road where the 23-year-old ecologist was killed?
>Flag Format:Diver24{road ID}
>Warning: Graphic content related to the killing would be appeared in this challenge.

この問題は添付画像などがなく、情報がとても少ない。とりあえず海外の出来事だと推測して「23 years old」「protest」などのキーワードで調べると、以下の記事が見つかった。

https://www.thequint.com/what-we-know/iran-anti-government-protests-mahsa-amini-mohsen-shekari-first-execution-death-penalty

イランで23歳の男性が抗議活動で死刑となっている。しかし、以下の部分が問題文と噛み合わない。

- 環境保全活動ではなく民主化デモ
- 道路の現在の路線番号を聞かれていることからかなり昔の出来事だと予想されるが、この事件は2022年と新しい

似たワードで他の候補を探していると、以下の記事がヒットした。

https://www.santelmomuseoa.eus/atlas/detalle.php?ni=EP-0044&lang=en

記事の概要は以下のとおり。

>1979年6月3日、反核デモ中にトゥデラで市民警備隊に殺害された23歳のサン・セバスティアンの若きエコロジスト、グラディス・デル・エスタル・フェレーニョを偲んで制作された作品。原子力反対国際行動デー」の期間中、南部バスクの反核委員会は、トゥデラ（ナバーラ州）で、国家エネルギー計画とバルデナス射撃場建設に反対し、レモイズを停止させるための行動を組織した。その2カ月余り前の1979年3月28日、ハリスバーグ近郊のスリーマイル島原子力発電所2号機が、米国の原子力史上最も深刻な事故に見舞われた。（Deeplで翻訳）

これなら問題文と合致する。次に事件のあった場所を調べたい。wikipediaによると**彼女の名前がついた歩道橋**（Gladys del Estal Bridge）があるとのことだったので場所を確認した。

https://www.google.co.jp/maps/place/43%C2%B018'44.2%22N+1%C2%B058'39.6%22W/@43.3130477,-1.9790324,18.25z/data=!4m4!3m3!8m2!3d43.31228!4d-1.97767?hl=ja&entry=ttu

この問題にフラグの提出回数制限はないので付近の路線番号を提出するも、すべて不正解。スペインなのは確実なのでスペインの路線番号を総当りすることも少し考えたが、流石にそれは無粋なのでちゃんと場所を調べていく。以下のサイトが見つかった。

https://ejatlas.org/conflict/the-death-of-gladys-del-estal-tudela-spain

事件があったのは**Tudela**だと判明。付近の路線番号を提出すると通った。

![](/images/diver-osint-ctf-2024-kakitsubata/protest.jpg =400x)

`Diver24{NA-8703}`

# transportation
## youtuber (medium) [blackwasan] - 211 point 86 Solves
>2023年、あるYouTuberが日本で無賃乗車や無銭飲食を行い、その様子を投稿したことで問題となった。
>彼は日本である列車に無賃乗車をしていたところ、九州地方のある駅で一度捕まったが、そのまま逃走して別の列車に再び無賃乗車をした。
>彼が捕まった駅と、乗り継いだ列車の列車番号を教えてほしい。なお、列車番号は時刻表に掲載されている形式で回答せよ。
>Flag形式: Diver24{駅名_列車番号}
>例えば、折尾駅で捕まり、456Mという列車に乗り換えた場合、Diver24{折尾_456M} となる。
>In 2023, a YouTuber became a problem when he posted a video of himself and his mates riding for free and eating for free in Japan.
>He was once caught riding a train in Japan without paying at a station in the Kyushu region, but he escaped and ride another train >without paying again.
>I would like to know the station where he was caught and the number of the train to which he changed. Please answer with the train >number (列車番号) in the format shown on the timetable (時刻表).
>Flag Format: Diver24{station name_train number}
>For example, if he was caught at Orio Station and ride 456M train, the flag should be Diver24{Orio_456M}.
>(Hint) 列車番号を正規表現で表すと次の通りである / Regex of train numbers : \d{1,4}[A-Z]

**meow_noisy氏らによる探索：**
この事件はメディアで目にした方も多いのではないだろうか。まずはこのYouTuberについて探る。そうすると、いくつか記事が見つかる。

「無料で日本を旅」外国人YouTuber炎上動画、ガイドライン違反で削除　新幹線無賃乗車、ホテル食い逃げで批判殺到
https://www.j-cast.com/2023/10/25471658.html

外国人ユーチューバーFidiasＪＲ九州新幹線無賃乗車動画「無料で日本中を旅しました」のフィディアス・パナイオトゥのチャンネル名は？
https://tadatabilife.hatenablog.com/entry/2023/10/27/061617

件の動画は、Fidiasと名乗る人物の動画であることが分かる。
本人のものは削除されてしまっているが、YouTubeに再アップロードされた動画が見つかる。

@[youtube](YzUDKBolwXY)


在来線の駅で駅員に捕まるシーン(3:24～)が映っており、その後駅員の制止を振り切って新幹線にて逃走を図る(4:26～)。

TODO: image

**ここからblackwasanのフォロー：**
このシーンをよく見ると逃走に使用した新幹線は、さくら572号新大阪行であることが分かる。 当該車両の列車番号は572Aなので、あとは駅名を絞るだけ。

駅員に捕まっているシーンで、一瞬だけホームの案内表示が映る(3:49) 。

TODO: image

この時、Fidias氏はJR九州の在来線特急「リレーかもめ」から下車しようとしているので、この特急と九州新幹線が乗り継ぎ可能な駅である博多・新鳥栖のどちらか。映像から、明らかに博多駅の規模ではないので答えは新鳥栖駅である。 （よく考えると、さくらの案内表示に「次は博多」と書いてあるので、その手前の停車駅からも絞り込めます）

`Diver24{新鳥栖_572A}`

## accident (medium) [blackwasan] - 471 point 28 Solves
>画像に映るバスと似たような車の観光バスが2023年に起こした事故の時刻と場所を探してください。
>Flag形式は Diver24{時刻_場所} です。
>
>時刻: 2000-01-23 01:23:45のような形式で、3秒の誤差が許容されます（現地時間）。
>場所: 40.689N74.044Eのような形式で、小数第四位を切り捨てて表してください。
>なお、時刻はバスが事故を起こしてから完全に停止した時刻を指します。
>Flag例:
>Diver24{2000-01-23 01:23:45_40.689N74.044E}
>注意: この問題には交通事故に関連する情報が含まれています。
>
>Find the time and location of the accident in 2023 caused by a tourist bus similar to the bus model shown in the image.
>Flag format is Diver24{Time_Location} .
>
>Time: it is in the form 2000-01-23 01:23:45 with a margin of error of 3 seconds allowed (local time).
>Location: it is in the form 40.689N74.044E, rounded down to the fourth decimal place.
>Note that the time refers to the time when the bus comes to a complete stop after the accident.
>Flag example:
>Diver24{2000-01-23 01:23:45_40.689N74.044E}
>Warning: This challenge contains information relating to road accidents.
>![](/images/diver-osint-ctf-2024-kakitsubata/image.jpg =450x)

**_roku_氏による推測：**
看板に「愛爾麗集團」 とあり、台湾っぽいが過信は禁物。信号機は台湾の信号機のようだ。
類似のバスについて調査していると、英倫交通公司（http://inlandbus.com.tw/ ）のバスに写真のような塗装のバスがあるので、台湾でよさそうである。

https://www.alamy.com/stock-photo-tour-buses-park-in-front-of-taiwan-democracy-memorial-park-in-taipei-136369876.html

**blackwasan：**
10億人を優に超える中華圏。年間のバス事故件数もそれはそれは桁違いであろう所、バス会社が特定されていて助かった！
調べていると、同系統のバスが関連していると思われる事故（2023年10月）の記事が見つかった。

https://www.taipeitimes.com/News/taiwan/archives/2023/10/22/2003808051

この事故では4人が死亡、22人が重軽傷を負ったらしい。写真には大破した乗用車と緑色のバス（特徴的なルーフ形状も一致）が写っているので、この件で間違いなさそうだ。

更に調査を進めると現地メディアによる事故のニュース（動画）がいくつかYouTube上にアップロードされていた。下記の例において、当時の事故を捉えた道路監視カメラのような映像が映し出されている。

雲林國道重大車禍! 遊覽車小客車碰撞4人無呼吸心跳  國3南下斗六段遊覽車.小客車相撞! 釀5傷4命危│記者 張峻棟 廖宜德│【LIVE大現場】20231021│三立新聞台
@[youtube](z2ziMqkg51s)

![](/images/diver-osint-ctf-2024-kakitsubata/accident-cam1.jpg)

映像では、事故後停止した状態の動画しかないので、 「おそらく」事故発生の瞬間を写した道路監視カメラのアーカイブのようなものがどこかに転がっているのだろうと確信。

上記ニュース映像を頼りに、道路監視カメラ設置場所を示すと思われる「國3 南 斗六段（現地語）」でググると下記のFacebook記事がヒット（生の事故映像なので閲覧注意） 。
https://www.facebook.com/watch/?v=675908411335360

![](/images/diver-osint-ctf-2024-kakitsubata/accident-cam2.jpg)

映像から、完全停止した時間は2023-10-21 09:43:02と推測できる。

事故現場は台湾の雲林県斗六市を走る高速道路3号線・263Kポスト付近の「斗六路段」という場所（23.702N, 120.603E）。
ストリートビュー（2024年撮影）でも生々しいブレーキ痕が残っており、外壁や中央分離帯も破損したままの状態であることが確認できる。
https://maps.app.goo.gl/7daZonFtT8PmCtZV9

![](/images/diver-osint-ctf-2024-kakitsubata/accident-cam3.jpg)

`Diver24{2023-10-21 09:43:02_23.702N120.603E}`

## italy (medium) [`_roku_`] - 481 point 23 solves
>2023年10月27日、新潟大学に通う学生が「珍しいことにイタリアのヘリコプターがキャンパスの上空を通った」と話していた。
>しかし、周囲かは「ヘリコプターは長距離を飛べない。イタリアから新潟に来られるわけがない」と一蹴されてしまった。
>彼の主張を裏付けるため、以下のものを調べてほしい
>ヘリコプターの保有者（社名）
>キャンパス上空を通過した時刻（JST）と、そのときの時の気圧高度
>Flag形式: Diver24{社名_hh:mm:ss(JST)_高度}
>たとえば、"Diver"社のヘリが9:10:12(JST)に高度1000ftで通過した場合、Flagは Diver24{Diver_09:10:12_1000} となる。
>On 27 October 2023, a student from Niigata University said "Unusually, an Italian helicopter passed over our campus".
>However, those around him brushed it off, saying that helicopters cannot fly long distances. There is no way they could come to Niigata, Japan from Italy’.
>To corroborate his claims, we would like you to look into the following information
>Owner of the helicopter (company name)
>Time of passage over the campus (in JST) and the barometric altitude at the time
>Flag format: Diver24{company name_hh:mm:ss(JST)_altitude}
>For example, on 9:10:12 JST ,the helicopter which is owned by the company named "Diver" passed over there in 1000ft, the flag will be Diver24{Diver_09:10:12_1000}

問題文から考えてもフライトレーダー系の問題。昨年（2023年10月）の情報を確認する必要があるため、どのツールかな、と考えていたら、チームで用意した作業用ノートに meow さんが残した「こころの声」を確認した。

![](/images/diver-osint-ctf-2024-kakitsubata/italy-guess.png)

素直に https://globe.adsbexchange.com/ の使い方を学ぶ方向で、流れとしては Playback の仕方さえわかれば何とでもなりそうだとういことで考えていると、チームメイトからの助言もあり地図の右端にあるアイコンの一番下が Playback だと判明。

Playback の準備をする。学生が上空を見上げて機体を識別できる時間帯は明るい時間帯に限られるだろうが、絞り込みすぎて二度手間になるのは避けたいので JST の 0 時から Playback を開始する。

- 場所は新潟大学のキャンパス（五十嵐キャンパス）
- 日時は JST の 2023/10/27 を加味して 10/26 15:00 UTC
- 244倍速（max）で再生

![](/images/diver-osint-ctf-2024-kakitsubata/italy-1.jpg)

あとは新潟大学の上空を凝視。
10/27 の 4:45 から 4:46 にヘリコプターの機影が横切ったので、Playback の再生速度を落として詳細を確認。イタリアの機体という情報に合致する。

![](/images/diver-osint-ctf-2024-kakitsubata/italy-2.jpg)

時刻は 13:45:40 JST で、気圧高度（Barometric altitude）は 1600ft であることがわかるので、後は保有者（社名）がわかればよい。機種に関する情報から “I-LIDI AW-169” で検索して保有者（社名）情報を調べる。

https://flyteam.jp/registration/I-LIDI

保有が「アリダウニア（Alidaunia）」である情報が入手できるため、これまでに得られた情報をつないで Flag。

`Diver24{Alidaunia_13:45:40_1600}`


## container (medium) [meow_noisy] - 496 point 12 solves
TODO: TBA

# misc
## number (easy) [kuzushiki] - 100point 176 Solves
>この車両の持ち主に連絡を取りたい。電話番号を調べてもらえないだろうか。
>Flag形式: Diver24{0123456789}
>
>注意: 実際に電話を掛けてはならない。
>
>I would like to contact the administrator of this vehicle. Could you please find out their telephone number?
>Flag Format: Diver24{0123456789}
>
>CAUTION: DO NOT MAKE AN ACTUAL PHONECALL
>
>(Hint) 携帯電話ではなく、固定電話である。It is not a mobile phone. It is a landline phone.
>
> （外-4906というナンバープレートが付いた車の写真が与えられる）

プライベートな情報を含むため写真の掲載は控えるが、車両の写真が与えられる。ナンバープレートから持ち主を特定しろ、ということだろう。

ナンバーが特徴的であり、「外」という文字が含まれている。どこかの地域を指すのかと思い調べていたが、以下のブログから**外交官ナンバー**だと判明。外国の大使や領事といった外交官が使用する車両だとわかる。
https://car-moby.jp/article/car-life/road-traffic-law-accident/international-number-plate/

ではどの国の大使なのだろうか？外交官ナンバーについて調べていると以下の記事が見つかった。ナンバーの上2桁から国が特定できるそうだ。
https://warmheart0159.hatenablog.com/entry/2017/06/06/162406

とにかく早く回答したい、と考えていた私はあろうことかこの記事を誤読してしまい、末尾2桁に着目してしまった。大使館の電話番号を提出するも、国が違うためもちろん不正解となる。焦って関係者の電話番号をいくつか試すも上手く行かず。

外交官ナンバーについてさらに深く調べていると、大統領専用車両にもこのナンバーが使われていることが判明した。一応試してみるか、と思い調べるも**大統領が直通の電話番号を公開しているはずもなく**調査は空振りに終わった。
https://www.mbs.jp/news/column/inside-story/article/2022/06/089335.shtml

このあたりで冷静になって外交官ナンバーの記事を読み直す。国がわかるのは下2桁じゃなくて上2桁であることに気づき、その国の大使館の電話番号を提出すると通った。

`Diver24{0334550361}`

## label (medium) [blackwasan] - 352 point 62 Solves
>この荷物の宛先として考えられる施設の郵便番号を教えてほしい。
>例えば在日アメリカ大使館が答えの場合、Flagは Diver24{107-8420} となる。
>
>Please tell me the postal code of the facility as a likely destination for this package.
>If it is the US embassy in Japan, the flag will be Diver24{107-8420}.
>![](/images/diver-osint-ctf-2024-kakitsubata/label.jpg =350x)


一番右下のQRを読んでみたところ、左のTRACKINGの数字に対応しているだけだった。
次に、横長バージョンのQRっぽいやつを読みたい。これはデンソーが最近開発したrMQRという規格。
https://www.denso-wave.com/ja/adcd/fundamental/2dcode/qrc/rmqr.html

読み取りアプリ（公式）があるので、ダウンロードして読み出す。
https://apps.apple.com/jp/app/%E3%82%AF%E3%83%AB%E3%82%AF%E3%83%AB-qr%E3%82%B3%E3%83%BC%E3%83%89%E3%83%AA%E3%83%BC%E3%83%80%E3%83%BC/id911719423

そうすると下記の文字列が出る。
SHIPMENT ADDRESS:
Baba-cho 14-1, Tsuruoka City, Yamagata, Japan

これは慶應義塾大学鶴岡タウンキャンパスの住所らしい。ということで、997-0035がFlag。
https://www.ttck.keio.ac.jp/en/contact/index.html

`Diver24{997-0035}`

## wumpus (medium) [kn1cht] - 356 point 61 Solves
>とあるDiscordサーバにFlagが投稿されたぞ！
>本問に限って、ターゲットに接触を試みても大丈夫だ。
>
>Flag has been posted on a Discord server!
>Only for this challenge, it is okay to attempt to contact the target.
>![](/images/diver-osint-ctf-2024-kakitsubata/PXL.jpg =300x)

flagという名前のDiscordサーバーの画面を撮影した写真が与えられる。
URLを手打ちしてアクセスしても、自分が入っていないサーバーには当然アクセスできない。

- https://discord.com/channels/1244302408402735114/1244302408402735117

![](/images/diver-osint-ctf-2024-kakitsubata/wumpus-deny.png =450x)

サーバーになんとかして入るか、入っている人に頼み込んで情報をもらう必要がありそうだ。
ここでDiscordのURLの仕組みを調べてみると、1番目の数字はサーバーのID、2番目の数字はチャンネルのIDである。
そこで、**”1244302408402735114”でGoogle検索**すると、publicなDiscordサーバーをリスト化しているサービスが見つかる。

https://discordservers.com/server/1244302408402735114

右上にJoinボタンがあるので、押して認証するとflagに招待してくれる。#generalチャンネルに投稿されたQRコードがFlagの文字列を示している。

![](/images/diver-osint-ctf-2024-kakitsubata/wumpus-flagserver.png =450x)

QRコードを読むにはどんなツールを使ってもよいが、筆者はDENSOが制作したクルクル（QRQR）を好んで使っている。

https://www.qrqrq.com/

`Diver24{Discord_1s_m0s7_u5efu1_t001}`

## timestamp (easy) [kuzushiki] - 404 point 50 solves
>"53" と塗装されている航空機の写真が撮影された日時を答えよ。
> `https://twitter.com/jointstaffpa/status/1767515646286549226`
>Flag形式: Diver24{YYYY-MM-DDThh:mm}
>例えば2024年4月1日13時45分に撮影された場合、Diver24{2024-04-01T13:45} となる。
>
>Answer the date and time the photograph of the aircraft painted “53” was taken.
> `https://twitter.com/jointstaffpa/status/1767515646286549226`
>Flag format: Diver24{YYYY-MM-DDThh:mm}
>For example, if the photo was taken at 13:45 on 1 April 2024, the flag should be Diver24{2024-04-01T13:45}.
>
>(Hint) タイムゾーンを変換する必要はない。/ You don't need to convert time zones.
>(5 attempts)

以下のツイートで言及されている航空機の撮影日時を答える問題。

@[tweet](https://x.com/jointstaffpa/status/1767515646286549226)

空の上で撮られているため、天気など時刻の手がかりになりそうな情報はない。画像のメタデータも確認したが有用な情報は得られなかった。

この画像が別の資料にも掲載されているのでは？と思い「H-6爆撃機」「3月12」などのキーワードでググると**PDF形式の資料**が見つかる。
https://www.mod.go.jp/js/pdf/2024/p20240312_01.pdf

このPDFから画像を抽出できれば解けそうだと考えチームメイトに相談したところ、kn1chtさんがシュッと抽出を行い、**元の画像データでは「航空自衛隊撮影」という文字の裏にタイムスタンプが隠れている**ことを教えてくれた。

（kn1cht補足）PDFファイルから画像を取り出す手段はいくつかある（Adobe Acrobatでコピーや編集をしたチームもあった様子）。解いている途中では、画像のメタデータに情報があるケースも考えられたため、データをそのまま取り出せる「PDF画像抽出ツール」を使用した。
ツールから取り出された画像データの右下に時刻が（おそらくカメラの写し込み機能によって）入っている。

https://forest.watch.impress.co.jp/library/software/pdfimgtools/

![](/images/diver-osint-ctf-2024-kakitsubata/timestamp-extracted.jpg =550x)

`Diver24{2024-03-12T15:33}`

## howmany (hard) [kuzushiki] - 497 point 10 solves
>画像の通り、2つのマンホールがある。2024/05/30 23:46 (JST) 時点で、この2つのマンホールの間で何台の赤い自転車を返却できたと考えられるだろうか。また、返却地点のIDは何だろうか。
>Flag形式: Diver24{台数_ID}
>例えば、赤い自転車を10台返却でき、その場所のIDがa10012bであった場合、FlagはDiver24{10_a10012b}となる。
>
>As shown in the image, there are two manholes - how many red bicycles could have been returned between these two manholes at 23:46 (JST) on 2024/05/30? Also, what is the identifier of this returning point?
>Flag format: Diver24{number of bicyecle can be returned_ID}
>For example, if 10 red bicycles could be returned and the ID of that place is a10012b, the Flag shoule be Diver24{10_a10012b}.
>
>(Hint) location.pngは、上の方が北です。location.png is a north-up map.
>(5 attempts)

以下3つの画像が与えられる。

>![](/images/diver-osint-ctf-2024-kakitsubata/howmany1.jpg =270x)![](/images/diver-osint-ctf-2024-kakitsubata/howmany2.jpg =270x)
>![](/images/diver-osint-ctf-2024-kakitsubata/location.png =270x)

### マンホールの位置特定
聞かれているのは自転車の数だが、まずはマンホールの位置が分からないとどうにもならないと考えた。1枚目の画像は番号がくっきり映っているので、この番号で場所を特定できるのだろうと推測。実際に調べていると「特定できる」と書かれている情報が散見され、以下のサイトで特定できそうなことがわかった。
https://ekikaramanhole.whitebeach.org/ext/0A0A/?c=y&s=01&n=3H&e=0H&ms=osm

しかし、マンホールの番号（62-01-3F-08）を入れても「文字キャップ(蓋)の指定に誤りがあります」とのエラーになる。他のサイトを使ったほうが良さそうだと思い調査するも、有用なサイトは見つからず。色々調べているうちに番号には「I」や「S」などの文字が使われていることに気づく。もしかして「1」ではなく「I」か？と思い「62-0I-3F-08」で検索すると歌舞伎町のマンホールだと判明。普段16進数を扱うことが多いので、16進数だという思い込みがあった。

![](/images/diver-osint-ctf-2024-kakitsubata/howmany-map.jpg =350x)


この調子で2つ目のマンホールも特定するか...と思ったが2つ目の画像は番号が読めない。気を取り直して3つ目の画像に目を移すと、1枚目のマンホールはT字路にあることがわかった。今までのヒントを総合することでマンホールの場所は推定できた。
- 上の画像の青枠の範囲
- T字路
- シェアサイクルステーションの近く

![](/images/diver-osint-ctf-2024-kakitsubata/howmany-road.jpg =280x)

### シェアサイクルの情報収集
あとはシェアサイクルの情報が知りたい。台数を聞いてくるということはAPIでもあるのだろうと推測。以下のサイトを見つけた。
https://ckan.odpt.org/dataset/c_bikeshare_gbfs-d-bikeshare/resource/06ddbb21-be3d-4163-ac92-d90127e9bf90

以下のエンドポイントからステーションのID（00010184）を特定できた。
- https://api-public.odpt.org/api/v4/gbfs/docomo-cycle-tokyo/station_information.json

```json
      {
        "lat": 35.693602,
        "lon": 139.703229,
        "name": "D6-01.新宿区役所本庁舎",
        "capacity": 48,
        "region_id": "5",
        "station_id": "00010184"
      },
```

返却台数も以下のエンドポイントからわかる。ただ、最新のデータしか得られないため、フラグとなる2024/05/30の情報がわからない。
- https://api-public.odpt.org/api/v4/gbfs/docomo-cycle-tokyo/station_status.json

パラメータを指定できるのか？と思い調べるも、そのような情報は見当たらず。過去のデータがどこかに残っているのでは、と思いarchive.todayをチェックしたところ、2024/05/30 23:46 (実際はUTC表記で14:46) のデータが見つかった。

- https://archive.md/mGgLI

```json
{
    "is_renting": true,
    "station_id": "00010184",
    "is_installed": true,
    "is_returning": true,
    "last_reported": 1717080364,
    "num_bikes_available": 4,
    "num_docks_available": 41
}
```

### フラグの提出
Num_bikes_availableが4なので、4台返却したのだろうと思い嬉々としてフラグを提出するも不正解。問題文をよく読むと「何台の赤い自転車を返却できたと考えられるだろうか」とある。返却可能だった台数を聞かれていることに気づき、capacityの48台 - 4台 の44台で提出したがまたも不正解となる。この時点で回答チャンスは残り3回。

2つ目のJSONをよく読むと**num_docks_available**というデータがある。以下のドキュメントに「返却車両を受け入れることのできる機能的なドックの数」とあるのでこれで間違いない。

https://github.com/MobilityData/gbfs/blob/v2.2/gbfs.md#station_informationjson

Capacityが48台、シェア可能な自転車が4台なのに返却可能台数が41台...？辻褄が合わないのが引っかかったが、41台が正解だった。

`Diver24{41_00010184}`

2回外したところでそもそもマンホールの位置があっているのか不安になり、マンホールを新宿まで確認しにいくべきかチームメイトと相談していた。3回目で当てられて本当に良かった。

# military
## osprey1 (easy) [meow_noisy] - 100 point 203 Solves

>2023年11月29日、アメリカ軍のオスプレイ（V-22）が日本の屋久島沖で墜落した。この機体の番号と、墜落時のコールサインは何か。
>Flag形式: Diver24{XX-XXXX_CALLSIGN}
>たとえば機体登録番号が01-2345、コールサインがCALL01の場合、Flagは Diver24{01-2345_CALL01} となる。
>
>この事故に関する問題は本CTFにおいて3問あります。正解することで1問ずつアンロックされます。
>
>On 29 November 2023, a US military Osprey (V-22) crashed off Yakushima Island, Japan. What is the number of this aircraft and what was its call sign at the time of the crash?
>Flag format: Diver24{XX-XXXX_CALLSIGN}
>For example, if the aircraft registration number is 01-2345 and the callsign is CALL01, the flag should be Diver24{01-2345_CALL01}.
>
>There are three challenges on this accident in this CTF. Each challenges is unlocked by answering it correctly.


この事件は屋久島沖米軍オスプレイ墜落事故のことをさしている模様。
Blackwasanさんが「**(作問者と思われる)ryo-aさんがtwitterでつぶやいてるんじゃない?**」と言っていたので、 その方針で調査することにした。いわゆる作問者メタ推理というやつ。

twitterで下記の検索式で投稿を確認。
`"from:geo_vitya since:2023-11-28  until:2023-11-30"`

やり方が部分的に刺さった。

@[tweet](https://x.com/geo_vitya/status/1730110822939291726)

>おおよそ12-0065と確信　夜に再確認します

"屋久島沖 CV-22 コールサイン" でgoogle検索すると、GUNDAM22 だとわかる。

フラグ
`Diver24{12-0065_GUNDAM22}`

作問者メタ推理をされることはryo-aさんも想定していたと思う。

## osprey2 (easy) [kn1cht / meow_noisy] - 100 point 114 Solves
>2024年2月15日、ある米軍基地でこの事故に関する追悼式典が実施された。16:46:37ごろ、その式典はどこで実施されていたか。
>OpenStreetMapのWay番号で答えよ。
>Flag例: Diver24{123456789}
>
>On 15 February 2024, a memorial service concerning this accident was held at a US military base. At approximately 16:46:37, where was the ceremony taking place?
>Answer using the OpenStreetMap Way number.
>Flag example: Diver24{123456789}
>
>(Hint) 基地全体ではなく、基地内の特定の区画・地点を示してください。 / Not an entire air base. Plase designate the specific area/point in the airbase

前の問題に引き続き、屋久島沖米軍オスプレイ墜落事故の追悼式典に関する問題。
"osprey memorial service" で検索すると、式典の報道が見つかる。

‘Profound bond’: Hundreds gather at Tokyo air base to remember fallen Osprey aircrew | Stars and Stripes
https://www.stripes.com/branches/air_force/2024-02-15/osprey-crash-japan-yokota-service-13010379.html

式典の場所について、the athletic field outside Yokota’s Samurai Fitness Centerとあるので、Google MapsなどでSamurai Fitness Centerを調べると、確かに北側にYokota Training Fieldという運動場がある。

https://maps.app.goo.gl/xHsoKhRxcyLs7r287

回答のWayを得るためにOpenStreetMapで地物を調べると、Way #810021666 (ground)と#810021665 (running track)がその場所にある。
後者を答えても正答にならず悩んでいたmeowさんからこの段階でkn1chtが問題を引き継ぎ、**athletic fieldによりよく当てはまるものは#810021666の方**ではないかと考え、正答した。

https://www.openstreetmap.org/way/810021666

`Diver24{810021666}`

## osprey3 (medium) [kn1cht] - 479 point 24 Solves
>墜落した機体は2018年11月15日の夜、ある空港に駐機していたらしい。その地点の標高（フィート）を整数で答えよ。なお、各種データソースの時刻にズレはないと仮定してよい。また、空港に関する情報は最新のものを参照してよい（2018年時点のデータを用いる必要はない）。
>Flag例: Diver24{250}
>
>この"osprey3"がこのCTFにおけるオスプレイの事故に関する最後の問題です。
>
>The crashed aircraft was apparently parked at some airport on the night of 15 November 2018. Give the elevation (in feet and integer) of that point. You can consider there are no discrepancies in the times of the various data sources. Also you can refer the latest information about the airport (no need to use data as of 2018).
>Flag example: Diver24{250}
>
>This "osprey3" is the last challenges about the Osprey crash in this CTF.
>
>(Hint) 空港全体の標高ではなく、駐機していた地点の標高である。 / Not an altitude of entire airport but an altitude of parking spot.
>
>(Hint) 変換、四捨五入などを行う必要はありません。問題文に合致した明確な数値を見つけることができます。 / No need to convert, round and so on. You will see an obvious number which matches the statement.
>(5 attempts)

難しかったのか、CTF中にヒントがどんどん増えていった問題。機体が過去にどこにあったか調べるには、**航空機スポッター（airplane spotters）** と呼ばれる航空機の撮影や追跡を趣味にしている人たちの記録が役に立つ。そこで、osprey1で分かった機体番号12-0065を検索すると、JetPhotosという写真投稿サイトに2018年11月15日の写真が公開されていた。

https://www.jetphotos.com/photo/9144357
（この写真がかっこよすぎてCTF中叫んでいた）

場所がKraków John Paul II Balice Int'l - EPKK, Poland（ポーランドにあるクラクフ・バリツェ空港）だと分かる。Wikipediaによれば空港の標高は791 ftだが、ヒントを読むと**駐機している地点を特定する**必要がありそうだ。そこで写真と航空写真を見比べると、管制塔や後ろの倉庫との位置関係から、だいたいの撮影位置が分かる。

https://maps.app.goo.gl/iVrxjZkK5pmHvePMA

EPKK airport chartsなどで検索し、空港チャート（航空関係者向けに空港の情報を表した地図）をチェックすると、この駐機場は”MIL APRON 3”と呼ばれていることも判明する。

- https://www.virtualairlines.eu/charts/EPKK.pdf

チャートからはMIL APRON 3の標高を見つけられなかったので、EPKK  ”MIL APRON 3”と検索したところ、空港の工事に関する資料の中でこの場所の標高がフィート単位で書かれていた。

- https://www.ais.pansa.pl/aip/pliki/EP_Sup_A_2024_023_en.pdf

`Diver24{774}`

## satellite (hard) [kn1cht] - 489 point 18 Solves
>2023年11月、北朝鮮が偵察衛星を打ち上げた。2024-06-07T05:01:07Z時点で、この衛星から最も近い位置にあった軍用飛行場はどこか。
>また、その時点の衛星の高度（単位:キロメートル）はいくつか。
>Flag形式: Diver24{基地名_高度}
>
>解答に際しては、 2024-06-06T04:41:45.620Z に発行されたデータを用いよ。
>
>基地名はGoogle MapsやWikipediaで確認できる 現地語表記 で答えよ。また、高度は小数点以下を切り捨てた値を答えよ。
>例えば、トルコのインジルリク空軍基地が最も近い位置で、その際の高度が613.65kmであった場合、flagは Diver24{İncirlik Hava Üssü_613} となる。
>
>In November 2023, North Korea launched a reconnaissance satellite. At 2024-06-07T05:01:07Z, which military airfield was closest to this satellite?
>Also, what is the altitude (unit: kilometer) of this satellite at that time.
>Flag format: Diver24{airbase name_altitude}
>
>In answering the challenge, use the data which is issued at 2024-06-06T04:41:45.620Z.
>
>The airbase name should be answered with the local language name which can be seen on Google Maps or Wikipedia. For altitude, answer the value rounded down to the nearest whole number.
>For example, if the Inzilik Air Base in Turkey is the closest position and the altitude at that time is 613.65 km, the flag should be Diver24{İncirlik Hava Üssü_613}.
>
>(Hint) 民間の空港や飛行場ではない / Not a civil airport or airfield

2023年に北朝鮮が打ち上げた偵察衛星とは、万里鏡1号（MALLIGYONG-1）のこと。
https://ja.wikipedia.org/wiki/%E4%B8%87%E9%87%8C%E9%8F%A11%E5%8F%B7

人工衛星は常に軌道が監視されていて、名称や識別番号で検索すると大まかな現在位置はすぐに分かる。

https://www.n2yo.com/satellite/?s=58400

しかし、問題文を見ると、
- 2024-06-07T05:01:07Z時点と過去の位置を答える必要がある
- 2024-06-06T04:41:45.620Z に発行されたデータを用いる
- 高度もkm単位で答える必要がある

というやけに具体的な指定がある。
単に衛星追跡サイトを閲覧するだけではなく、衛星の軌道データから位置を計算する問題なのではないかと予想した。人工衛星の軌道は時々刻々と変化しており、軌道データが古いと正しい位置が計算できない。実際、Xで北朝鮮の人工衛星に関する情報を投稿している都立大の佐原教授によると、万里鏡1号はまさに数日前に軌道を修正しており、古い情報は役に立たないと考えられる。

@[tweet](https://x.com/TMU_SSL/status/1798890882189705693)

色々検索していると、衛星の軌道データ（Two-line element set; TLE）から位置を計算するプログラムについての情報が得られる。筆者はSkyfieldというPythonライブラリを導入し、ドキュメントを見ながらコードを実行した。

https://rhodesmill.org/skyfield/earth-satellites.html

```python
from skyfield.api import load, wgs84, EarthSatellite

tle = [
    '1 58400U 23179A   24159.97159215  .00006581  00000+0  30025-3 0  9991',
    '2 58400  97.4051  46.9797 0001572 158.6830 201.4471 15.20948569 30329'
]
ts = load.timescale()
satellite = EarthSatellite(*tle, 'MALLIGYONG-1', ts)
print(satellite)
geocentric = satellite.at(ts.utc(2024, 6, 7, 5, 1, 7))
lat, lon = wgs84.latlon_of(geocentric)
print('Latitude:', lat)
print('Longitude:', lon)
print('Altitude (km):', wgs84.height_of(geocentric).km)
```

```
結果：
MALLIGYONG-1 catalog #58400 epoch 2024-06-07 23:19:06 UTC
Latitude: 31deg 51' 55.4"
Longitude: -100deg 31' 53.6"
Altitude (km): 506.16102972003506
```

なお、出力を見るとTLEのepochが2024-06-07 23:19:06 UTCとなっていて、問題文の2024-06-06T04:41:45.620Zから更新されてしまっているようだ。これが気になって以前のTLEを見つけようと粘った（奇跡的にGoogleの検索結果に残っていた）が、結論から言えば数時間ずれたTLEを使ったとしても解答には影響しなかった。

あとは、31° 51' 55.4" -100° 31' 53.6"（米国テキサス州にあるSan Angeloという町の近く）付近にある空軍基地の名前を調べ、高度である506 kmと繋げれば正解できる。

`Diver24{Goodfellow Air Force Base_506}`

# investigation_request
本カテゴリのみ、ある（架空の）人物についての連続した問題が出題される。ただ、mapperという問題が予想外の難易度となってしまい、本番中に本カテゴリを進めたのは2チームだけだった。

## mapper (easy) [kn1cht] - 500 point 2 Solves
あなた方は極めて高い調査スキルを持っていると聞いた。我々の身分は明かせなくて申し訳ないのだが、一つ調査依頼を受けてくれないだろうか。
我々はある男を追っている。
情報が見つからず困っていたのだが、彼が撮ってアップロードした写真を見つけた。現地時間でいつ撮影されたのか特定してほしい。

Flag形式: Diver24{yyyy-MM-dd HH:mm}
例えば、2024年6月5日午後3時14分ならば Diver24{2024-06-05 15:14}となります。

この問題に正解すると、2問が新たに追加されます。

You have extremely high investigation skills. We are sorry that we cannot reveal our identities, but we would like to ask you for an investigation.
We are in pursuit of a man.
We were having trouble finding information, but we found a photograph he took and uploaded. We need you to identify when it was taken in local time.

Flag format: Diver24{yyyy-MM-dd HH:mm}
For example, if it is 3:14 pm on 5 June 2024, the flag should be Diver24{2024-06-05 15:14}.

If you answer this challenge correctly, two new challenges will be added.



Easyなのに終盤まで誰も解けなくて話題になった伝説の問題。
とある街角の写真が渡される。看板などの情報から、岐阜駅前の大通りであることは容易に割り出せるが、撮影時刻を知るには「彼がアップロードした」ときの情報を見つける必要があると思われる。

この写真のメタデータに”FBMD0a000a7101000044a8000099ef01008ffd0100bc0f02002c080400a0c3060003e806000c0407003b250700bb9a0b00”という文字列があり、これはFacebookがアップロードされた写真に付与しているメタデータだと分かる。それによって、我々も含め多くのチームはFacebook, Instagram, ThreadsなどのSNSから岐阜駅に関する投稿を必死で見まくる作業を続けていた。ただ、このメタデータは運営もそこまで重視していなかった様子で、予期せぬミスリードとして働いてしまった。

問題のタイトルに”mapper”（地図をつくる人）とあること、対象者が写真をアップロードしたと書かれている（SNSなら、投稿したという表現になりそう）ことから、SNSではなく地図関連のサービスで写真が見つかる可能性を思いついた。
それでもGoogle Maps, OpenStreetMap, Wikimedia Commonsなどメジャーなサイトでは該当する写真が出てこない。最終的に、”map image upload” “mapper image upload”などの検索結果を眺めていたところ、Mapillaryという場所の写真共有サービスを発見した。

https://www.mapillary.com/app

Webアプリを開き、岐阜駅近くにズームしていくと、この写真の地点の近くに全く同じ写真がmori_mune24というアカウントからアップロードされていることが分かる。

https://www.mapillary.com/app/user/mori_mune24?lat=35.412007&lng=136.75669800000003&z=17&pKey=438678415240541&x=0.5112438510127063&y=0.4499698406608722&zoom=0




撮影されたであろう時間が書いてあるので、分単位で入力するとFlag。

️Diver24{2023-02-06 09:46}

## venue (easy) [`_roku_`] - 500 point 2 Solves
>我々が追っている人物は2024年5月にイベントを開催していたらしいことが判明した。我々は予約履歴を照会したいと考えている。
>そのために会場と思われる場所を特定してもらえないだろうか。
>Flag形式: Diver24{会場のURL}
>
>It turns out that the person we are tracking seems to have organised an event in May 2024. We are thinking of querying their booking >history.
>Could you identify the venue for this event?
>Flag format: Diver24{URL of the venue}

Mapillary の “mori_mune24” のアカウント名から水平調査すると、Xのも同じアカウント名があることが確認できた。

https://x.com/mori_mune24

投稿されている内容を確認すると、イベントを予定しているという内容の投稿があり、そのイベントに関する Google docs へのリンクが共有されていることがわかる。

中身を確認すると 5 月にイベントを開催しようとしていることが確認できる。イベント名や「モリカワ」という名前から、どことなくお酒を飲みながらセキュリティのトークをする日本のイベントで開催される某CTFへのオマージュを感じる。
Google フォームから何か情報が手に入るかを確認してみたが、特に何も情報が得られない。ダウンロードして Word で開いて何か情報が得られないかも確認したが有益な情報は見られない。チームメイトと会話していると、ドキュメントにコメントがあるとの話になり、中身を確認したところ「リンクを消した」といったコメントが残っているが、リンク先がわからない。
ふと、展開ボタンがあることに気づき開いてみたところ、URL が記載されていた。

得られた URL は貸し会議室のもの。イベントはこの会議室で行われたと考えらえるため、こちらの URL を入力して Flag。

`Diver24{https://www.instabase.jp/space/8039340260}`


余談:
誤って文章をいじってしまったため、googleドキュメントにご本人登場が起きた。

## uploader (medium) [meow_noisy] - 500 point 2 Solves
我々が追っている人物は、どこかに猫の動画を投稿しているようだ。その動画のアップロード時間を特定できないだろうか。
Flag形式: Diver24{投稿時刻のunix time(UTC+0)}
例えば、2000年1月1日 12時34分56秒(UTC+0)に投稿されている場合、Diver24{946730096}となります。

The person we are pursuing appears to have posted a video of a cat somewhere. Can you determine the upload time of that video?
Flag format: Diver24{upload time in unix time(UTC+0)}
For example, if the video was uploaded at 12:34:56(UTC+0) on 1st Jan 2000, the flag should be Diver24{946730096}.

TODO: TBA

# 最後にひとこと

## kn1cht
難しい問題も多いCTFでしたが、全く詰まってしまうということは少なく、調査を進められている実感を得ながら解けた良い問題が多かったと感じました。特に、satelliteやcontainer、Akeomeは調査の過程自体が楽しく好きな問題です。

## `_roku_`
総当たりで危うく鳥貴族の文字がゲシュタルト崩壊しそうになりました。サブミットした問題数こそ少ないですが、それぞの問題に参加して考える時間があり、非常に楽しめました（運営の皆様ありがとう！！）。最後まで残った Container 問題は、チームメイト全員であれこれ会話して correct にたどり着けた思い出深い一問です。

## kuzushiki
ここ数年OSINT CTFに取り組んでいるのですが、問題を解くスピードが遅いと感じています。今回のCTFでは早く解くことを意識しましたが、そのせいでしょーもないミスを複数やらかしました。とはいえチームとしては優勝できたので、終わり良ければ全て良しですね。国内のOSINT強豪チームである40548Fのメンバーと一緒に戦えて心強かったです。

## blackwasan
普段チーム40548FとしてOSINTに参加しているryo-aが今回は運営側に回ったので、”共闘”ということでmeow_noisyさんにお誘いいただき、チームKAKITSUBATAとして参加させていただきました。
geo問中心に問いていましたが、「こうやって解いてほしいんだろうな」という運営側の意図を感じさせる問題が多い印象を受けました。幅広い難易度の問題があるCTFで、初心者から常連まで楽しめる大会だったのではないでしょうか。
いつもは大体解ける問題を解き終わってそのまま行き詰まることも多いのですが、私が仮眠したり食事を取っている間に数問解かれていることが多々あったので、今回のチームメイトの粘り強さをひしひしと感じました。深夜0時すぎにmapperの扉が開かれた時の盛り上がりと、チームとして最終解答となった問題のcontainerが通った時の爽快さは想い出深いものとなりました。
運営の皆様方、楽しい問題をありがとうございました。お疲れ様でした！
