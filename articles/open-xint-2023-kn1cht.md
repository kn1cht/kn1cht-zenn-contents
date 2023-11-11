---
title: "Open xINT CTF 2023 Write-up (kn1cht)"
emoji: "🍺"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - OSINT
  - CTF
  - avtokyo
published: false
---

2023年11月11日に開催された[Open xINT CTF 2023](https://openxintctf.wixsite.com/pinja/post/2023-open-xint-ctf)に参加しました。
昨年に引き続き2回目の参加です。今回はチーム戦ではなく個人戦にルール変更されました。

https://zenn.dev/kn1cht/articles/open-xint-2022-40548f


全18問のうち8問を解いて1050点、11位という結果でした。解けた問題に関しては結構スムーズに正答までたどり着けた点は良かった一方で、あと一歩のところまで進めたのにFlagを取れなかった問題があったのはもったいかなったです……。

![](/images/open-xint-2023-kn1cht/stats.png)

解けた問題、途中まで進めた問題についてWrite-upを残しておきます。
**ネタバレなのでご注意ください。後から解きたい方のために、説明上やむを得ない場合を除きFLAGの文字列そのものは記載しません。**
問題文および問題画像は競技システムから引用しました。

# GEO Category
## Amusement

>ここの施設の名前は？
>What is the name of this facility?
>![](/images/open-xint-2023-kn1cht/5_GEO_Amusement.jpg)

ウォーミングアップ的な問題です。
リフトと観覧車が写った写真が与えられました。珍しい組み合わせだなと思いつつGoogle Lensにかけると、旅行案内サイトなどで施設名が分かります。そのまま日本語で名称を答えれば正解です。

## mural
>この壁画が描かれた建物の位置座標を答えてください。
>フラグ形式： 7桁のplus code
>(画像出典: https://iranprimer.usip.org/blog/2022/feb/09/irans-revolution-43-politics)
>What are the coordinates of this building with a mural?
>Flag format: 7 digits plus code
>(image: https://iranprimer.usip.org/blog/2022/feb/09/irans-revolution-43-politics)
>![](/images/open-xint-2023-kn1cht/10_GEO_mural.jpg)

建物に描かれた壁画の写真が与えられました。

引用元の記事を見ると、 "A mural in Tehran depicting Ayatollah Ali Khamenei (left) and Ayatollah Ruhollah Khomeini (right)" とあり、イラン・イスラム共和国の首都**テヘラン**にあることがわかります。

見た目の規模からしてかなり有名な絵ではないかと推測されるので、Google Lensにかけると予想通り多くのページが見つかります。
ただ、この絵はイランの有名な指導者を描いていることもあり、イランの政治について論じた記事でイメージ写真として使われているケースが多くて場所を見つけるには手間がかかります。

そこで、今まで手に入れたキーワードを並べて"tehran mural khamenehi"などでキーワード検索したり、そこで出てきた写真を再びGoogle Lensで調べたり……といったことを繰り返していると、**場所が記載されたWebページ**が見つかりました。

https://www.flickr.com/photos/ninara/41361150950

写真の説明から撮影地が**Enghelab Square**ということ、写真自体から建物前に**何かの公共交通機関の乗り場**（後で調べたらバスのようでした）があることが分かります。

地図で見れば分かるようにEnghelab Squareは結構広いのですが、写真で見た乗り場らしきものを中心にGoogleストリートビューで探していけば目的の建物が分かります。

https://maps.app.goo.gl/RHWR8cBfsrrqvbbs8

隣の建物を誤って答えないよう、衛星写真も見て建物の形を確認しながら、Googleマップの住所表示からplus code（`P92A+BCD`のような7桁）をコピーすれば正答です。

# WHOIS Category
## neko
>neko.vn の登録日付はいつ？
>フラグ形式： yyyy-mm-dd
>hint: ほかのサイトで探してみて。
>What is the registration date for neko.vn?
>Flag format: yyyy-mm-dd
>hint: search with other websites

`whois`コマンドで`whois neko.vn`のように問い合わせると、何やら結果が出てきましたがその情報の中に正答はありません。

出力の中に、`vn`のTLD（トップレベルドメイン）を管理しているVietnam Internet Network Information CenterのURLが載っているので、そこからドメインを検索するようにします。

https://vnnic.vn/

開くと全体がベトナム語で戸惑いますが、右上にあるイギリスの国旗マークを押すと英語に変わります。ページ内にあるWHOIS SEARCHにドメインを入れて検索すると、CAPTCHA認証を挟んだ後にWHOIS情報が出てきます。Creation Dateを答えれば正解です。

![](/images/open-xint-2023-kn1cht/vnnic.png)

なお、ここのCAPTCHAが絶妙に難しくて数回弾かれました。この問題一番の難所かも！？

## COMPANY
>neko.vn のMSTは？
>What is the MST for neko.vn?

nekoの続きです。MSTというドメイン用語を調べても不調に終わりますが、問題名から類推すると**会社に関する何かを調べる**べきではと思われるので、neko.vnのページ最下部にある会社名 "TY CỔ PHẦN CÔNG NGHỆ NEKO" をGoogle検索します。

![](/images/open-xint-2023-kn1cht/company.png)
*ちょっと面白い字面*

すると、会社情報を載せたWebサイトが引っかかるので眺めていると、"Mã số thuế" なる項目に数字が掲載されています。これの頭文字がMSTではないかと考え、数字を答えたら正答でした。

https://masothue.com/0107742974-cong-ty-co-phan-cong-nghe-neko

後で調べたところ、MSTは「税番号」で、（最近何かと話題の）公式なインボイスをベトナムで発行してもらうために必要とのことです。

https://vietexpert.jp/news/others/6347

# MISC Category
## Aircraft
>この機体のモデル名は？
>フラグ形式： [A-Z]+-\d+[A-Z]+
>What is the model of this aircraft?
>Flag format: [A-Z]+-\d+[A-Z]+
>![](/images/open-xint-2023-kn1cht/Aircraft.png)

航空機を撮影した解像度低めの動画が与えられます。秒数が短いものの、**4発のターボプロップ機**に見えます。

~~そしてここからが本題ですが、自衛隊はこのタイプのF16を装備していない~~

4発ターボプロップというといくつかあるものの、身近なものにはP-3CやC-130シリーズなどがあります。
まずは自衛隊も装備しているらしいC-130Hを答えてみましたが、これは不正解でした（詳しい人はカラーリングなどの情報から弾けるそうな）。

https://www.mod.go.jp/asdf/equipment/yusouki/C-130H/index.html

C-130シリーズにはC-130Jというのもあり、これも画像検索結果で言及されていたので答えてみると正解になりました。米軍で運用されているC-130Jには尾翼に国旗が描かれていて、よくよく見ると動画の機体にもうっすら赤青の何かが見えるので、正解を確信できます。

https://trafficnews.jp/post/66544

なお、本問では解答フォーマットが正規表現で与えられました。CTFに出るような方にはこの程度の正規表現は余裕かもしれませんが、不安な場合は[regex101](https://regex101.com/)のような正規表現テストサイトを使用すると便利です。

![](/images/open-xint-2023-kn1cht/regex.png)

# AI Category
## REVERSE AI
>この生成AI画像は、ある文学作品の冒頭部分を画像イメージにしたものである。この文学作品のタイトルを答えよ。
>This AI-generated picture depicts an opening of a literature. What is the title of this book?
>![](/images/open-xint-2023-kn1cht/15_AI_REVERSE_AI.jpg)

2名の銃を持った人物と白い犬が冬っぽい林を歩いているイラストです。

筆者はこの情景を見た瞬間に「◯◯の◯◯◯◯◯っぽくね？」と浮かんでしまい、青空文庫で冒頭部を念のため読んでからFlagとして提出したら通ってしまいました……。

他の方はもう少し真面目に（？）画像生成AIの逆をどうするのかなどを考えていたらしいのでなんだか申し訳ないですね……。

なお、Stable Diffusionのような画像生成モデルの画像から逆にpromptを予測するinterrogatorと呼ばれるモデルは実際に開発されています。

https://runrunsketch.net/clip-interrogator/

# BUS Category
## BUS
>このバスの座標を求めよ。
>フラグ形式： Nxx.xxx_Exxx.xxx
>Answer the coordinates of this bus.
>Flag format: Nxx.xxx_Exxx.xxx
>![](/images/open-xint-2023-kn1cht/1_BUS_BUS.jpg)

xINT CTF伝統のバス問。バスが正面から写っているので、そこをトリミングしてGoogle Lensで調査すると「**日光市営バス**」の車両で、足尾地域と日光地域を結ぶ路線に投入されたということがわかります。

https://www.shimotsuke.co.jp/articles/-/413925

日光市営バスの路線図を検索すると、日光の市街地と足尾の市街地を結んでいる比較的シンプルなルートでした。

ここで写真に戻ると、人家などは写っておらず大きな擁壁があることから、**市街地から離れた道路上**だと推測できます。
そこで、市街地と市街地の間にある区間を中心にストリートビューで見ていきます。なお、途中には長さ2,765 mの日足トンネルがあり、そこは除外できるので調査が楽でした。

同じようで少しだけ違う景色に遭遇してがっかりしながら道路を進んでいくと、写真とぴったり一致する地点にたどり着きます。

![](/images/open-xint-2023-kn1cht/bus.jpg)
*[Google Maps](https://maps.app.goo.gl/HiLeyjr7vEk3zwhq6)より*

あとはバスの座標を答えます。小数点以下3桁まで座標を答える形式では毎年少しの位置ずれで苦戦する人も多いのですが、本問の写真をよく見るとバスの後ろに道路の制限速度表示が見えるので、その少し前あたりを基準に座標を答えれば正解です。

![](/images/open-xint-2023-kn1cht/bus-zoom.jpg)

# SOCIAL
## cosplayer (unsolved)
>下記のURLに投稿された、恐竜に扮したCCさくらのコスプレイヤーの方は昔、
>人間のCCさくらのコスプレイヤーの方と2ショットを撮っています。
>人間のCCさくらのコスプレをしていた方のX(Twitter)ユーザ名を答えてください。
>https://x.com/MikeCovers/status/979804843543056384?s=20
>フラグ形式： pinja_xyz
>The dinosaur-version CC Sakura cosplayer took a photo together with a human-version CC Sakura cosplayer in the past.
>What is the X (Twitter) username of the human-version CC Sakura cosplayer?
>https://x.com/MikeCovers/status/979804843543056384?s=20
>Flag format: pinja_xyz

（個人名の記載を控えます）

恐竜の気ぐるみとCCさくらの衣装を着たインパクトのあるツイートが与えられます。ただ、投稿者はコスプレイヤー本人ではないようなので、まずは恐竜のコスプレイヤーを探す必要があります。

Anime Bostonというイベント名が記載されているので、"anime boston cc sakura"などで検索していくと、結果の一つに「この人は知り合いの友人で**、彼のFacebookに写真がある**」といったコメントが見つかります。

https://www.reddit.com/r/CLAMP/comments/88mg1n/comment/dwor11y/

Facebook（URL省略）を見ると、確かに複数のイベントでこのコスプレをしている人物のFacebookページです。
「写真」の欄を見ても問題文にあるツーショットはありませんが、「タグ付けされている写真」にそれらしき投稿があります。

![](/images/open-xint-2023-kn1cht/cosplay.jpg)

この投稿をしたカメラマンがもう一人のコスプレイヤーさんもタグ付けしていて、Facebookで名前を知ることができます。

イベント中はここで実名を検索したりしてもたどり着かないな……と思いながら終了してしまいました。名前ではなくFacebookのアイコンを画像検索すると、同じアイコンがXでも使われているのでそのユーザ名が正解でした。試せばよかった……。

# CRYPTO Category
## Bitcoin
>2021年11月に結婚したIlyaが所有していたとみられるBitcoinのアドレスは？
>What is the Bitcoin address of Ilya who married in 2021 November?

（一部、個人名の記載を控えます）

暗号通貨に関する有名人ではないかと思われるので、`Ilya married 2021 bitcoin`などと検索すると、その名前の人が盗難ビットコインを資金洗浄しようとした疑いで逮捕されたとの記事が見つかります。

https://www.justice.gov/opa/pr/two-arrested-alleged-conspiracy-launder-45-billion-stolen-cryptocurrency

ここで本人と妻のフルネームが分かるので、`（妻のフルネーム） married 2021`で改めて検索すると、確かにこの2人が2021年に結婚したことが確認できます。

ではアドレスはなんでしょうか？　暗号通貨では所有する資産はアドレスに紐づいており、全ての情報が公開されています。特に、主要な犯罪に関連するアドレスはそれ自体が有名になることも多いです。
そこで`（Ilyaのフルネーム） bitcoin "address"`で検索すると、暗号通貨専門のニュースサイトに「Ilya夫妻に関連するアドレスが米国政府によって押収されている」という記述があります。

https://u.today/5th-biggest-mysterious-bitcoin-wallet-owner-is-somebody-totally-unexpected-report

ニュース記事には、「彼らが10,000 BTCという大量のビットコインを送金したときにWhale Alertによって注目された」ということも書いてあります。Whale Alertは、「クジラ」と呼ばれるビットコイン大量保有者による高額の送金を逐一監視しているサービスのことです。

https://u.today/part-of-almost-130000-btc-stolen-in-2016-bitfinex-hack-just-transferred-to-anonymous-wallet

ニュースのリンクから辿っていくと、問題の取引の記録が判明するので、10,000 BTCを受け取ったアドレス（bc1...）をコピペすれば正答です。

https://whale-alert.io/transaction/bitcoin/77ad70fadfbbad5191c47c951469095ca845006f25fe9814f30f2853af367459

# ONSITE Category
## [SIGINT]SIGNAL (unsolved)
>我々は不審なデバイスを見つけた。デバイスを発見して停止コードを書き込むことで、そのデバイスの不正な動作を一定期間止めることができる。デバイスの動作を止めよ。
>We have found a suspicious device. Find the device and write a stop code to the device: it will stop the device's unauthorized operation for a period of time. Stop the device.

今回SIGINTの問題が出ると聞き、以前オンサイトで開催されたxINT CTFの情報を調べると、**過去にはBluetoothに関連する問題が出ていた**ことを知りました。

そこで、スマホとPCにBLEスキャナーアプリをインストールしてから会場に赴き、イベント開始からときどきスキャンをかけるようにしていました。

すると、`xINT_CTF_Flag`というこれ見よがしに怪しいデバイスが見つかりました。これにConnectして情報を引き出せばよさそうです。

![](/images/open-xint-2023-kn1cht/ctf_flag.png)

ただ、セキュリティカンファレンスAVTOKYOの会場内ということもあって、周辺には**数百ものBLEデバイス**があって劣悪な電波環境です。接続ボタンを押してもいっこうに接続できません。
そこで、発信源に近づけばいいのではと考えて会場内を長時間歩きまわりますが、デバイスの位置は特定できず……。pinjaブースにあるのは分かりやすすぎるし反対側にあるのか、それとも実は人が持っていてときどき移動しているのかなど色々考えてしまいます。

@[tweet](https://twitter.com/pinja_xyz/status/1723251309355340213)

すると、運営であるpinjaの方から「**pinjaブースにあるよ**」とのヒントが出ました。深読みしすぎた……。

pinjaブースの近くに移動して接続を試すと、なんとか接続できました！　このBLEペリフェラル（ここからBLEの用語が出てきますが、ググってみてください）には、1つのserviceと1つのcharacteristicがあるようです。
そして、characteristicはread/write可能なもので、`0x6265657220706C65617365`なるデータが送られてきていました。

これは、ASCIIとして解釈すると`beer please`となります。そこで、`beer`（`0x62656572`）を何度もwriteしてみるのですが、接続が悪くて操作したらすぐに切れてしまいます。
`beer`という文字列自体もFlagではなく、タイムアップとなりました。

![](/images/open-xint-2023-kn1cht/beer.png)

後で話を聞くと1位のryo_aさんはpinjaブースに張り付くくらいの位置でbeerを送ってFlagを取得したそうで、近づき度合いが足りなかったのかもしれません……。

# 感想
オンサイトのCTFはいつぞやのSECCON以来で、少し緊張しながらもAVTOKYOのワイワイした雰囲気を楽しみながら参加できました。問題の楽しさや難易度バランスの良さはさすがOpen xINTという感じで、（正解した範囲では）気持ちよく解ける問題が多かったように感じます。

SIGINTの準備はしていたにも関わらずオンサイト専用問題が1つも解けなかったのは心残りです（HUMINTも、会場が薄暗くて対象人物がよく分からないと思って後回しにしてしまいました）。次回があればチャレンジしたいと思います。
