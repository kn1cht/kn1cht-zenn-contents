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
5名（meow_noisy, blackwasan, _roku_, kuzushiki, kn1cht）でチーム”KAKITSUBATA[^1]”を結成し、参加しました。
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

# Introduction
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

そこで、Xで「鳥貴族 閉店」を検索した。なお、この画像の**EXIF情報に2024年2月4日という日付があ**るので、そのあたりの期間を狙う。

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
