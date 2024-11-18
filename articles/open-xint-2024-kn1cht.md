---
title: "Open xINT CTF 2024 Write-up (kn1cht)"
emoji: "🍺"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - OSINT
  - CTF
  - avtokyo
published: true
---

2024年11月16日に開催された[Open xINT CTF 2024](https://openxintctf.wixsite.com/pinja/post/2024-open-xint-ctf)に参加しました。
チーム参加も含めると3回目の参加です。昨年同様に、個人戦かつオンサイト・オンラインのハイブリッド形式（ただし、オンサイト限定問題はスコアで優遇）でした。

筆者は諸事情で現地ではなく自宅から参加したため、現地限定のONSITEカテゴリ以外に絞って挑戦しました。全19問のうち11問を解いて2100点、5位でした。

![](/images/open-xint-2024-kn1cht/stats.png)

解けた問題、途中まで進めた問題についてWrite-upを残しておきます。

- **ネタバレなのでご注意ください。**
- 後から解きたい方のために、説明上やむを得ない場合を除き、FLAGの文字列そのものは記載しません。
- 全く手を付けていない問題（peincil、lighthouse、およびONSITEカテゴリ）には言及していません。
- 問題文および問題画像は競技システムから引用しました。

去年2023のWriteup:

https://zenn.dev/kn1cht/articles/open-xint-2023-kn1cht

## amore (100)
>この本のタイトルは？
>What's the title of this book?
>![](/images/open-xint-2024-kn1cht/4_book.jpg)

とある本を上から見た写真が与えられました。イラストが見えますが、これは背表紙ではなく各ページの端に印刷することで絵柄を表現したものです。おしゃれ。

小口印刷（edge printing）という手法のようです。

https://www.print-on.jp/doujin/comic/tokusyu/koguchi_insatu.htm

かつイラストにエッフェル塔やクロワッサンがあることから**フランス関連の書籍**と考えられます。デザインに特徴があるので話題になった可能性もあると思い、「エッフェル塔 クロワッサン 本」などでググったものの何も出ません。

もう一度写真をよく見ると、背景にある書棚にはアルファベットが書かれた本が並んでいます。つまり、日本語圏の本ではないのではないかということで、**海外の結果が出やすいよう英語設定にしたGoogleアカウント**でGoogle Lensを用いました。
これは日本語ユーザーが海外のOSINT CTF問題を解く際の定番手法です。以降、英語設定のGoogle検索を使った際は「英語Google検索」「英語Google Lens」と書くようにします。

https://meow-memow.hatenablog.com/entry/2022/08/06/111747

>![](/images/open-xint-2024-kn1cht/book-lens.png)

英語Google Lensの結果、本のタイトルがズバリ載っているページが見つかりました。ヨーロッパを舞台にした恋愛小説のようです。

https://pangobooks.com/books/d2c2b954-5659-4a33-bdc7-0ca732c4047e-GdlDO79abeO4yFwzPZee02xfCaj1

## all green (100)
>この店の電話番号は？
>形式：国番号およびハイフンなし
>What is the phone number of this store?
>Format: without country code and hypens
>![](/images/open-xint-2024-kn1cht/9_allgreen.jpg =250x)

白いお皿に乗っている食べ物をGoogle Lensにかけると、これはGinger and wasabiと呼ばれていることが分かります。
このお店は**海外の寿司レストラン**のようですね。

Google Lensであまり有望な結果が得られないため、画像を拡大して隅々までよく見てみました。机上に貼られたシールに"hiroba sushi"などと書かれています。

>![](/images/open-xint-2024-kn1cht/allgreen-zoom.png =400x)

店名で改めて検索し、店内のテーブルや天井が一致することを確かめて電話番号を入力したら正解でした。

## noodle (200)【未解答】
>この料理の名前は？
>What's the name of this dish?
>![](/images/open-xint-2024-kn1cht/1.jpg =400x)

おいしそうな麵料理の写真が与えられました。筆者は見た目的にパスタかなと思い込んでしまい（間違い）、正解できないまま競技が終了しました。
具体的には、画像検索結果の中にあった[レシピサイト](https://recipe.rakuten.co.jp/recipe/1480021056/)で、似た見た目のカチョ・エ・ペペ（cacio e pepe）というパスタがローマ近辺にあることを知って各種言語で入れたりしていました。

Lensの結果を落ち着いて見ると、「特撰ひやむぎ きわだち」というお店と食器が一致することが分かります。お店のSNSでメニュー名を探すとflagが得られます。

![](/images/open-xint-2024-kn1cht/udon.png =400x)
*競技終了5分前に万策尽きて適当に入力し始めた様子。意外と近かった？*

## whois (100)
>gov.ru の納税者番号は？
>What is gov.ru's tax payer ID?

$ whois gov.ru

`whois`コマンドの結果から`taxpayer-id`を探して入力すると正答。

## art (100)
>この作品の作者は？
>Format: 姓_名
>Who is the artist of this artwork?
>Format: Last name_First name
>![](/images/open-xint-2024-kn1cht/15_art.jpg =450x)

Google Lensによって、この像が日比谷公園かもめの広場にあることが容易にわかります。作者に言及しているWebページもいくつかあります。

https://mariko7.com/archives/3080

## exhibition (100)
>history-art問題の人物は、とある展覧会の常連で、作品は何度も受賞しています。 その展覧会で1922年、会友に推薦されて入会したものの、1924年に退会した人物がいます。 この人物の名前は？ Format: 姓_名
>A person associated with the history-art challenge was a regular at a certain exhibition and the artist's works won several awards. In this exhibition, there was an artist who was recommended for membership and joined as a member in 1922, but resigned in 1924. What is the name of this artist?
>Format: Last name_First name

「{前問の答え} 受賞 展覧会」などと検索すると、詳しい人物紹介ページで二科展にたびたび出展したことが分かります。

https://www.tobunken.go.jp/materials/bukko/28321.html

この展覧会は二科会という美術団体が開催しているものです。Wikipedia記事を開いて1922という文字列を探すと、なにやら引っ掛かります。

>横井弘三 - 入選：1915年。第1回樗牛賞受賞。1916年、二科賞受賞。1922年に二科会友に推挙されるも、翌年決別して会友を返上。（[「二科会」（2023年12月17日 (日) 21:29 UTCの版）『ウィキペディア日本語版』](https://ja.wikipedia.org/wiki/%E4%BA%8C%E7%A7%91%E4%BC%9A) ）

ほぼ間違いなさそうですが、1922年の翌年決別だとすると1923年になってしまって問題文と合いません。そこで他の解説も読んでみることにしました。

https://yuagariart.com/uag/nagano61/

>大正１２年、関東大震災が起き、横井は震災被害にあった子どもたちを慰めるため、おもちゃなどを題材にした油彩画を小学校に寄贈しはじめ、翌年の第１１回二科展に寄贈作のうち１１点をひとつの額におさめた「復興児童に贈る絵」を出品した。しかし、石井柏亭ら審査員に陳列拒否をされたため、会友を返上して二科会と決別、石井との確執はその後も続いた。（[素朴な画風で「日本のルソー」と呼ばれた横井弘三 - ＵＡＧ美術家研究所](https://yuagariart.com/uag/nagano61/)）

第11回二科展は関東大震災の翌年、大正13年（1924）年9月に開催されたそうです（https://search.artmuseums.go.jp/records.php?sakuhin=4251 ）。したがって、彼の名前がflagで間違いありません。

※ 前述のWikipediaの記述は不正確なのではないでしょうか……。

## birthplace (200)
>history-exhibition問題の人物が持つ別名には、ある画家が関連しています。 その画家の生家は最近特定されました。 生家のプラスコードは？
>Format: XXXX+XX
>The artist of the history-exhibition challenge has an alias that's linked to a painter. The birthplace of this painter was recently identified. What is the Plus Code of the birthplace?
>Format: XXXX+XX

前問の答えの人物は「日本のルソー」と呼ばれていました。1844年にフランス・Lavalで生まれた画家**アンリ・ルソー（Henri Rousseau）** のことですね。

「henri rousseau birthplace」などで検索（英語Google検索の方が出やすい）すると、Lavalの名所案内記事で問題の建物が紹介されていました。

https://nikkidiscovers.com/2018/11/02/10-top-things-to-do-in-laval/

>In Laval, you can visit the birthplace of the renowned painter Henri Rousseau, located conveniently close to the church, right next to it. This site, known as Beucheress Gate, features part of the medieval city wall and resembles a section of a castle. ([10 Top things to do in Laval – Nikkidiscovers](https://nikkidiscovers.com/2018/11/02/10-top-things-to-do-in-laval/))

城跡みたいな見た目なので本当にこれが家……？となりましたが、各所を調べるとアンリ・ルソーの一族が**古い町の門を買い取って暮らしていた**という話が伝わっているそうです。すごい

さて解答ですが、Google Mapsで素直に Beucheress Gate と検索しても出てこないため、フランス語での名称 Porte Beucheresse を入れれば地点が出てきます。plus code（地点を表す7桁のコード。例：`P92A+BCD`）をコピペして正答。

## top gear (200)
>「トップギア」シーズン11 エピソード4で、２人がバスに乗っているとき、映像に映りこんだバス停の名前は？
>In Top Gear Season 11, Episode 4, what was the name of the bus stop that appeared on screen when the two of them were on the bus?
>（筆者注：開始後しばらくは「バスに乗り込んだとき」と表現されていましたが、その後分かりやすく修正されました）

~~トップギアとトップガンを毎回混同するのは私だけですかね？~~

有名なエピソードなので、シーズン11エピソード4で何の企画が放送されたか、日本語でも英語でも調べればすぐ分かります。トップ・ギアの出演者たちが日本を訪れ、**日産GT-Rと公共交通機関で競走**した回ですね。リチャードとジェームズが電車の切り離しに遭遇して慌てふためくシーンが数カ月前にX（Twitter）で話題になっていたのが記憶に新しいです。

トップギアシリーズは各種配信サイトで配信されているため、もしそれらの会員であればすぐにでも視聴できます。ただ、ルールに書かれている通り、無償で得られる情報だけで解答できるはずです。筆者が最初に考えたのは、番組のファンがリチャード・ジェームズの移動ルートを細かく解説してくれているブログなどがないか？ということでした。

https://ja.wikipedia.org/wiki/%E3%83%88%E3%83%83%E3%83%97%E3%83%BB%E3%82%AE%E3%82%A2

調査の結果、2人がバスに乗ったのは京急久里浜駅から久里浜港に京急バスで移動したシーンであることまでは分かりました。
しかし、やはり映像を確認しなければflagは分かりません（「京急久里浜駅」は違いました）。

実はTop Gearの宣伝トレーラーなどに問題のシーンが映っていないのか？と考え、公式YouTubeチャンネル（https://www.youtube.com/@TopGear/search ）でseries 11と検索してみました。
すると、トレーラーどころか**本編映像**が出てくるではありませんか！　公開の経緯は不明ですが、公式チャンネルなので違法動画でないことだけは確かです。

@[youtube](DWjWX3dbkfQ)

ありがたく視聴している（ジェレミー・クラークソンが日本の高速道路を走ってるだけで面白い）と、1分29秒目あたりの車内映像で確かにバス停が映りました。
文字は判読できないものの、乗車した系統（久7 東京湾フェリーゆき）やGoogle ストリートビューの画像を調べていくと後方に写っているマンションを特定できます。マンションの前にあるバス停名称がflagです。

@[codepen](https://codepen.io/kn1cht/pen/WNVWpBJ)

終了後まわりに聞いてみたら、有料サイトで視聴したりAmazon Primeでレンタル（100円）したという方もいました。
アンケートを取ってみると、課金した方と無料で解いた方が両方いて、後者が若干多かったようです。

@[tweet](https://x.com/kn1cht/status/1857804414079340637)

## bus stop (300)【未解答】
>前方を走るバスが次に停車するバス停の名前は？
>What’s the name of the next stop for the bus in front?
>![](/images/open-xint-2024-kn1cht/7_busstop.jpg =450x)

バス問なのでまずはバスをよく見ます。奈良交通バスの車両でナンバーは6-38のようです。車種が一致するかどうかに注意しながら「奈良交通バス "6-38"」を検索すると、当該車両は葛城営業所に所属する日野レインボーであることが分かります。

http://blog.livedoor.jp/zassy_/archives/1078528805.html

「葛城営業所は奈良県南部の広いエリアを担当して」いるとのことなのでこれだけでは絞れません。もう一度写真をよく見ると、手前に「**天〇村消防団　〇二分団車庫**」と書かれたシャッターが見えました。奈良の地図を見れば、天川村という村があることも分かります。

あとは天川村にある消防団第二分団の車庫の位置が分かればよいのですが……。ネット上で位置が直接分かる情報は見つからず、バスが通るルート（2本ほどしかない）をGoogleストリートビューで巡回する作戦に出ましたが発見できず時間切れとなりました。


## bus (500)
>このバスの座標を求めよ。
>形式：Nxx.xxx Exxx.xxx
>What is the coordinates of the bus？
>Format: Nxx.xxx Exxx.xxx
>![](/images/open-xint-2024-kn1cht/8_bus.jpg =550x)

前問でもそうですが、うまい具合に行先表示が見えないようなカメラ設定で撮られていて絶妙です。

画像を拡大するとすぐにバス会社、ナンバーが見つかります。「弘南バス "13-83"」で検索し、この車両が弘南バス所属で車番は50404-6であることも分かりました。

https://4travel.jp/travelogue/11887699

https://www.wikihouse.com/Kantetsu1931/index.php?%C0%C4%BF%B9200%A4%AB13-01~14-00#google_vignette

弘南バスが走るエリアすべてをしらみつぶしするのは莫大な時間を要しますが、とりあえず前掲の4travelの記事で紹介されていた五所川原～青森線と仮定して調べることにしました。この路線は、青森市の西に位置する五所川原市中心部を出発し、山地を横断して青森市内へ至るものです。

写真を見る限り、木が生い茂っているので市街地の中ではなさそうです。そこで、バス路線をGoogle Mapsに表示させたうえで**五所川原市と青森市の間の道を中心にストリートビューで巡回**しました。

![](/images/open-xint-2024-kn1cht/bus-map.jpg)
*地図中白丸で囲ったあたり（出典：国土地理院）*

闇雲に探してもいいのですが、見逃す可能性を下げるためなるべく注目する地点を絞りたいところです。写真右にはカーブミラーが写っていて、その奥は建物なのでT字路なのが分かります。したがって、地図でバス路線を見ながら、T字路があるたびにストリートビューに降りて確認する作業を繰り返しました。

バスの後ろにある少しさびた屋根が見えないか注意しながら見ていくと、五所川原市側で同じ風景を見つけました。バスの位置はT字路の真ん中あたりです。

@[codepen](https://codepen.io/kn1cht/pen/OJKGmvw)

座標をフォーマットに合わせてから入力すれば正解です。

## groovin' (200)
>この店の名前は？
>What is the name of this shop?
>![](/images/open-xint-2024-kn1cht/12_groovin.jpg)

カフェでDJパフォーマンスを楽しむ人々の画像が与えられました。明らかに日本の店ではないため英語Google Lensで店内の風景を検索すると、まさにこの時の様子を投稿したInstagramの投稿が上位に出てきます。

https://www.instagram.com/p/C55sjTQNq-a/

店のアカウントもメンションされていて、その店名がflagです。

https://www.instagram.com/livestreamcoffee/

余談：このDJイベントを主催した団体がアーカイブ動画をアップロードしていて、競技後半の作業用BGMにしていました： https://www.youtube.com/watch?v=wRCfEV3thsU

## Tatopani (200)
>この場所のプラスコードは？
>https://x.com/kaidanmeguri/status/1852695312378253472
>Format: XXXX+XX
>What is the Plus Code for this location?
>https://x.com/kaidanmeguri/status/1852695312378253472
>Format: XXXX+XX

ネパールのTatopani（温泉）に行ったレポートがX（Twitter）に投稿されているので、場所を見つける問題です。Google Mapsでネパール付近の地図を見ながら探しました。素直にTatopaniと検索しても出にくいので、投稿の発言からキーワードを拾って "nepal villa tatopani" で検索するとよかったです。

複数候補が出てきてしまいますが、そんなに多くないので航空写真と道路形状が一致する地点を見つけてplus codeを確認できれば正解です。

https://maps.app.goo.gl/DiJHigC1xnQYqk1d8


## building (300)【未解答】
>https://www.5-tv.ru/player/5003603
>このビルはどこにある？
>Nxx.xxx Exx.xxx で示せ。 回答フラグ形式を修正しました。
>https://www.5-tv.ru/player/5003603
>Where is this building located? Please specify using coordinates in the format Nxx.xxx Exx.xxx We have modified the flag format.

ロシア側に占領されたウクライナ・ドネツク州の街Горловкаに対してウクライナ軍が行った攻撃を、ロシアのTV局Пятый Канал（チャンネル5）が報じた映像です。
残り時間が少なすぎたためちょっと調べただけでリタイア。大学時代にロシア語の授業を真面目に聴いておくべきだった……。

## mural (300)
>この壁面装飾を描いたアーティストに手紙を出したい。私書箱番号を教えてほしい。フラグは、数字のみ記入。
>I’d like to send a letter to the artist who drew the mural. What's the P.O. Box number? Enter only the numbers.
>![](/images/open-xint-2024-kn1cht/5_mural.jpg)

この場所は有名らしくGoogle Lensですぐ見つかります。米国ユタ州のKanabという町にある壁画のようです。
関連サイトを巡っていると、アーティストの名前は**Patti Lewis**だとした投稿が見つかりました。

https://www.flickr.com/photos/127584027@N08/51277725637

名前で検索すると、Pattiさんは夫のJeffさんと一緒に夫婦でアーティストユニットを結成しているそうです。ポートフォリオサイトには電話番号とメールアドレスが記載されているものの、本問ではP.O. Box番号が要求されているのでさらに調査が必要です。

https://www.lewisartservices.com/

英語Google検索で「"P.O. Box" "Patti Lewis" "Kanab"」などと調べました。Kanabを入れないと、同姓同名の政治家の情報ばかりが出てきます。また、完全一致を指示するダブルクォーテーションの位置も数パターン変えながら試しました。

すると、FastPeopleSearchやBeenVerifiedといったサイトに**特定人物の個人情報がまとめられ、その中にP.O. Boxの番号もある**ことが分かります。

このあと試行錯誤しましたが、最終的にはBeenVerifiedを使用しました。まずはGoogle検索の結果から近い名前の人の一覧を開きます。この場合はPattiを検索したのにPaulが出てきてしまいました。

https://www.beenverified.com/people/paul-lewis/ut/

上の検索窓に名前を入れてSearchを押したくなると思いますが、これをすると数分待たされたあと会員登録を要求されるだけなので時間の無駄です（2敗）。

そもそも、Paulの一覧には会員登録なしでたどりつけました。同様にPattiの一覧を見る方法を考えましょう。URLをよく見ると、`people/paul-lewis/ut/`となっています。これを`people/patti-lewis/ut/`に変更すると、結果が変化します。

https://www.beenverified.com/people/patti-lewis/ut/

最上位に表示されている人物は親族がJeffとなっていて、ユタ州に私書箱がありますので答えと考えて間違いないでしょう。Po Boxの番号をコピペすれば正答です。

一般の方の個人情報をこんな簡単にネット上で検索できていいんだろうか……と心配になる問題でした。

## 感想
素早く解けるべき問題をおおむね詰まらずに処理できた点は良かったと思います。ただ、どうせならbus問3つすべて解いて終わりたかったですね……。
