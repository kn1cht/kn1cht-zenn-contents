---
title: "SWIMMER OSINT CTF Writeup (kn1cht)"
emoji: "🏊‍♀️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - OSINT
  - CTF
published: true
---

2026年1月17日に開催された[SWIMMER OSINT CTF](https://swimmer.diverctf.org/)に参加しました。DIVER OSINT CTF 2025を開催したチームによる初心者向けOSINT CTFです。

DIVER OSINT CTF 2025のWrite-upはこちら：
https://zenn.dev/kn1cht/articles/diver-osint-ctf-2025-kakitsubata

本大会は6名を上限としたチーム戦ですが、 ~~チーム編成を忘れていたため~~ 一人で参加しました。
結果、全問を解いて32位（全完多数のため、全問題に正答した時刻順）となりました。（数え間違えていなければ）ソロ参加の中では7位でした。

- **以下のWriteupには、本CTFのネタバレが含まれます。後から解きたい方のために、説明上やむを得ない場合を除きFLAGの文字列そのものは記載しません。**
- 問題文および問題画像は競技システムから引用しました。
- 本CTFにはストーリーが存在し、未来にいる依頼者が偽情報を流布する組織の調査を現代にいる我々に依頼してきたというものでした。「未来では～」のような表現が問題文にあるのはそのためです。

# briefing category
8つの課題が順番に開放され、回答していくことによって基本的なルールやOSINT CTFの解き方、フラグの提出方法などを学べます。基本的に文章に書いてある内容を読み、理解すれば正答できます。
また、最後の課題まで終えると、ルールおよび今回の調査対象人物・事項に関する説明資料をダウンロードできます。

# tgt_rain category
> ”rain” と名乗る人物は偽情報を制作・流布する実働部隊として活動を行っていることが確認されました。2026年時点では、SNS上で「旅人である」と称している以外は詳細な手がかりを掴めていません。

## rain_01_social (easy)
>rain は2026年時点でXのアカウントを所持していたようです。
>我々は、この人物の投稿のスクリーンショットを入手しました。
>
>スクリーンショットからアカウントを特定し、このアカウントのID（スクリーンネーム）を解答してください。
>
>例えば、@gov_online が対象のアカウントの場合、Flag は SWIMMER{@gov_online} となります。
>
>As of 2026, rain appears to have had an X account.
>We have obtained a screenshot of their post.
>
>Identify the account from the screenshot, and answer the account's ID (screen name).
>
>For example, if the target account were @gov_online, the flag would be SWIMMER{@gov_online}.
>
>![rain_01_social](/images/swimmer-osint-2026-kn1cht/rain_01_social.jpg)

画像にある「これわりと近所だったから見に行ったな」という文言をXで検索すると、画像と同じと判断できる投稿が見つかり、IDを特定できました。

https://x.com/bruto_rain/status/1995806649903448141

## rain_02_region (medium)
>rain は自身のブログに趣味の投稿を行っていたようです。 投稿に用いられている写真のほとんどがこの人物の撮影したものではないフェイクのようですが、1枚だけ、実際にこの人物が撮影したと考えられる写真が存在します。 その写真を特定し、撮影地を地図上で解答してください。
>
>It seems rain posted hobby content on their blog.
>Most photos used in the posts were not taken by them, but not all of them are fake.
>One photo appears to be genuinely taken. Please answer where it was shot on a map.

2問目にしてなかなか歯ごたえがあり、カテゴリ内でも一番解けたチームが少ない課題でした。

ブログのURLはrain_01で発見したアカウントのbiographyに記載されています（`https://brutorain.wordpress.com/`）。中身を見ると、鉄道写真の投稿が続いたあと、突然陰謀論的なタイトルと謎の歴史に絡んでいそうな画像つきの記事が存在しました。

鉄道写真は20枚を超えており、一見本物のように見えるものばかりです。Xの投稿からも有益な情報が取れるわけではなく（後の課題で判明するように、Xにもフェイクが混じっていました）、糸口が掴めませんでした。

今大会では運営（ストーリー上は、"未来"の依頼者）からヒントが3回全員に配布される仕組みでした。2回目までのヒントの内容はこのようになっています。

>rainは関東周辺に住んでいると自称していますが、これは事実ではないようです。
>「遠征」と称して投稿している写真に、彼が実際に撮影したものが含まれているかもしれません。
>
>rainが自作のものとしている写真には、AIで生成したものが含まれているようです。また、rainが自作のものとしている写真のうち、AIで生成されていない写真の大半はインターネット上から拾ってきたもののようです。
>なんとかしてこれらをより分けることができれば良いのですが……

というわけで、ブログにある鉄道写真を全部手元にダウンロードして、目視でAI生成画像を除外していく作業を始めました。また、ヒントの前半から関東の記事は無視し、 **遠征と書かれている記事** （中国地方遠征・東海遠征？・東北遠征・関西遠征・九州遠征）の画像12枚に絞って調査を進めました。

![fake_or_real](/images/swimmer-osint-2026-kn1cht/fake_or_real.jpg)

ここからは楽に進める方法はなく（鉄道に詳しい方なら一見して見破れるかもしれませんが）、地道に各画像を拡大表示して変なところを探します。そもそもAI生成画像のクオリティがやけに高いんですよね……。「これ本物かなあ」と思いながら拡大していくと細部の文字だけが崩れている、という程度のものがいくつかありました。

私が各画像をAIと判断した基準を以下に示します。

- 車両の行き先表示や駅名標を拡大すると文字が崩れている、特に文字が小さい英語表記によくある（hankyu-1.png, hiroshima-1.png, hokkaido.png, kyushu2.png, okayama.png, sannnomiya-1.png, tohoku-1.png）
- いかにも実在の車両っぽいが、細部を見比べるとデザインが少しだけ違う（kansai.png, kyushu.png, setouchi.png）

その結果残ったのがkeihan-2.pngとtokai-2.pngでした。この時点で正解と確信できてはいないものの、前者は拡大しても駅の広告や掲示の文字が本物らしく、実際の写真の可能性が高いと判断したためこの場所を答えることにしました。

![keihan-2](/images/swimmer-osint-2026-kn1cht/keihan-2.jpg)

この問題で登場する画像全般に言えますが、「この画像はOSINT CTF競技のために用意された～」という透かしが随所に入っているので、Google Lensに読ませるとAI要約がOSINTの解説ばかりになるんですよね……。場所特定においてもこれが障壁となってしまったものの、ホームの広告にあるクリニックの名前を検索することで、ここが **大阪府枚方市の星ヶ丘駅** であることが分かりました。

Google Mapsの3D表示などを見ながら位置を微調整（車両の少し後方上空にある架線柱がMapsでも見えるので、目印として有用）して正答しました。

## rain_03_source1 (medium)
>rain は、目を引くタイトルの記事を書くことで閲覧数が多くなると考えたようです。
>この人物はこれをきっかけとして偽情報の作成・流布にのめり込んでしまったと考えられます。
>
>この人物が一番最初に投稿した偽記事では、ある画像に全く関係ないキャプションが付けられています。 本来の出典を探し、この画像がどこの市区町村にまつわる資料に掲載されたものか>解答してください。
>都道府県名を含む必要はありません。
>
>例えば、平塚市 の資料に掲載されていた場合、Flagは SWIMMER{平塚市} となります。
>
>rain believed that writing clickbait article titles would boost views.
>They then became engrossed in creating and spreading disinformation.
>In their very first fake article, an unrelated caption was attached to an image. Find the original source and answer which municipality's materials this >image appeared in.
>You do not need to include the prefecture name.
>
>For example, if the picture were on the document about 平塚市 (Hiratsuka City), the flag would be SWIMMER{平塚市}.

https://brutorain.wordpress.com/2025/12/19/%e6%b0%97%e4%bb%98%e3%81%84%e3%81%a6%e3%81%97%e3%81%be%e3%81%a3%e3%81%9f%ef%bc%81%ef%bc%81/

陰謀論的な記事の1つめに関する課題です。かなり昔に撮影したと思われる工事中の現場の写真があります。

![source1](/images/swimmer-osint-2026-kn1cht/source1.png)

本問も結構時間がかかりました。素直に画像検索するだけでは、見当違いの場所をGeminiが提示するだけで正答には至りません。

画像内には「**工事中ノ濾過池（其一）**」という説明が付けられていました。"工事中ノ濾過池"でGoogle検索すると、國立臺灣大學（国立台湾大学）のデジタルライブラリで同じ題が付けられた写真がいくつかヒットします。雰囲気としては似ているようにも思えます。

https://dl.lib.ntu.edu.tw/s/photo/item/90690#

これは日本統治時代の台湾で行われた水道工事などの記録を集めたもので、これ自体はとても面白かったものの、一致する写真があるわけではありません。やはり国内の歴史資料を当たるべきかと思い[国立国会図書館サーチ](https://iss.ndl.go.jp/)や[JAPAN SEARCH](https://jpsearch.go.jp/)でも検索しますが、特に有力な手がかりは得られませんでした。

このあたりで諦めて他の問題を進め、競技開始から6時間あたりで再度「工事中ノ濾過池（其一）」で検索したところ、AI概要が興味深い結果を返してきました。

![source1_correct](/images/swimmer-osint-2026-kn1cht/source1_correct_search.png)

>特に、豊橋市水道誌の翻刻資料において、建設工事中の様子を捉えた写真のキャプションとして見られます。

https://app.honkoku.org/transcription/A665885145923E1270F323287C03076C/28

出典として示されたリンクに探し求めていた画像そのものがあったので、自治体の名前をflagとして提出したところ正解でした。
AI概要も侮れないなあという感想ですね。

## rain_04_source2 (easy)
>rain が2番目に作成した偽記事には、「rainが未公開の情報を見つけた」ということが書かれているようです。しかし、これは虚偽だと思われます。
>この人物の嘘を暴くには、正確な出典を探す必要があります。
>
>この画像の出典となる古い書籍は電子化されており、詳細が参照できるはずです。
>その資料のデジタルオブジェクト識別子（DOI）を解答してください。
>例えば、答えが 12.34567/890123 の場合、Flagは SWIMMER{12.34567/890123} となります。
>
>In rain's second fake article, it claims they found unpublished information, but this is likely false.
>To debunk it, you need to find the original source.
>The image comes from an old book that has been digitized, so you should be able to view the details.
>Please answer the digital object identifier (DOI) of that source.
>For example, if the answer were 12.34567/890123, the flag would be SWIMMER{12.34567/890123}.

https://brutorain.wordpress.com/2025/12/21/%e8%aa%b0%e3%82%82%e7%9f%a5%e3%82%89%e3%81%aa%e3%81%84%e7%9c%9f%e5%ae%9f%e3%82%92%e7%99%ba%e8%a6%8b/

陰謀論的な記事2つめに関する課題です。

壮大な建物を描いた図があるので画像検索すると、これは日本の国会議事堂の建築設計競技に渡辺福三が提出した設計案でした。

https://commons.wikimedia.org/wiki/File:National_Diet_Building_Competition_Submission_Watanabe_Fukuzo.jpg

Wikimedia Commonsに示された出典URL（国立国会図書館デジタルコレクション）をたどり、『議院建築意匠設計競技図集』の書誌情報からDOIを見つければ完了です。

## rain_05_date (medium)
>rain は2025年12月25日 (JST)、ある場所に来たことをX上で投稿しています。
>しかし、この画像は2025年の別の日に撮影された写真のようで、事実とは異なるようです。 この写真が本当に撮影された日を YYYY/MM/DD の形式で解答してください。
>ただし、撮影日は日本時間（JST）を基準とします。 例えば、答えが2025年6月8日の場合、Flagは SWIMMER{2025/06/08} となります。
>
>rain posted on X that they visited a certain place on December 25, 2025 (JST).
>However, the image seems to have been taken on a different day in 2025. Please answer the actual date the photo was taken in YYYY/MM/DD format.
>Use Japan Standard Time (JST) for the date.
>For example, if the correct date were June 8, 2025 in JST, the flag would be SWIMMER{2025/06/08}.

https://x.com/bruto_rain/status/2004085702469075019

大阪城ホールの前の写真が投稿されています。画像を拡大してよくみると、"10000 *****"というイベントの看板が立っています。

「大阪城ホール 10000人 イベント」などで検索すると、「サントリー1万人の第九（SUNTORY 10000 Freude）」が1位でヒットします。公演日がflagです。

https://www.suntory.co.jp/culture-sports/daiku/

## rain_06_ai (hard)
>rain のブログには、東京駅の画像があります。しかし、これは実際に撮影したものではなく、AIで作成された画像のようです。
>この画像が何というツールで作成されたのか特定し、解答してください。
>例えば、答えが Adobe Firefly の場合、Flagは SWIMMER{Adobe Firefly} となります。
>
>There is an image of Tokyo Station on rain's blog, but it appears to be AI-generated rather than an actual photo.
>Answer which tool was used to create it.
>For example, if the answer is Adobe Firefly, the flag would be SWIMMER{Adobe Firefly}.

https://brutorain.wordpress.com/2026/01/03/%e6%97%a5%e5%b8%b8%e3%82%92%e8%a8%98%e9%8c%b2%e3%81%99%e3%82%8b/

記事の画像 `https://brutorain.wordpress.com/wp-content/uploads/2026/01/tokyo_0003.jpeg` をダウンロードします。ブラウザから右クリックで保存するとWEBPという別形式で保存されてしまうため、wgetコマンドなどで元の画像を直接取得しました。

exiftoolコマンドでメタデータを確認するとツール名が確認できます。
AI画像生成ツールは、AI生成であることを示したり、ツールの情報を伝達したりする目的でメタデータに様々な情報を埋め込むことがよくあります。

```
$ exiftool tokyo_0003.jpeg
ExifTool Version Number         : 12.76
File Name                       : tokyo_0003.jpeg
// ...
User Comment                    : {"aigc_info":{"aigc_label_type":0,
                                   "source_info":"dreamina"},"data":
                                   {"os":"web","product":"dreamina",
                                   "exportType":"generation","pictureId":
                                   "0"},"trace_info":{"originItemId":
                                   "7590949739743448328"}}
Image Width                     : 5404
Image Height                    : 3040
// ...
```

# tgt_debeyohiru category

> “debeyohiru”と名乗っている人物がいます。ソフトウェアエンジニアリングのバックグラウンドがあり、ツールの開発などに従事していると推定されます。

## debeyohiru_01_social (medium)
>調査対象の人物はソフトウェアエンジニアで、2026年以降には debeyohiru というIDでの活動が確認されています。
>この人物がこのIDでの活動を開始したのは2026年1月のようです。この人物がこれ以前に使用していたIDを特定できないでしょうか？
>
>2026年1月時点で、noteというサービスに古いIDのアカウントが残存しているようです。
>このアカウントのIDを解答してください。
>
>例えば、digital_jpn_gc が対象のアカウントの場合、 Flag は SWIMMER{digital_jpn_gc} となります。
>
>The person under investigation is a software engineer, and from 2026 onward has been active under the ID debeyohiru .
>They appear to have started using this ID in January 2026. Can you identify the ID they used before that?
>
>As of January 2026, an account with their old ID still exists on the service note.
>Please answer that account ID.
>
>For example, if the account was digital_jpn_gc, the flag would be SWIMMER{digital_jpn_gc}.

debeyohiruでGoogle検索すると、Blueskyのアカウントが見つかります。

https://bsky.app/profile/debeyohiru.bsky.social/post/3mavlx6v3ak2c

12月にnote記事のスクリーンショットを投稿しており、URLも分かるのでIDを特定できます。

## debeyohiru_02_profile (easy)
>debeyohiru は2026年1月時点で求職中で、プロフィールページをウェブ上に公開していたようです。
>このページを探り出し、そのURLを解答してください。
>
>例えば、 https://example.com/foobar が対象のページの場合、 Flagは SWIMMER{https://example.com/foobar} となります。
>
>As of January 2026, debeyohiru appears to have been job hunting and had a profile page published online.
>Find this page and answer its URL.
>
>For example, if the relevant page were https://example.com/foobar, the flag would be SWIMMER{https://example.com/foobar}

https://whatsmyname.me/ でfuraigo5というIDのアカウントを探索するとGitHubアカウントを発見でき、そのプロフィールに探したいURLが記載されています。

https://github.com/furaigo5

## debeyohiru_03_email (easy)
>debeyohiru が2026年現在、普段使っているメールアドレスが知りたいです。
>この人物が現在使用中とおぼしきメールアドレスを探り出し、解答してください。
>
>例えば、メールアドレスがfoobar@example.comの場合、Flagは SWIMMER{foobar@example.com} となります。
>警告: このアドレスに対してメールを送信してはなりません。解答に際して、メールを送る必要は一切ありません。
>
>We want to know the email address that debeyohiru currently uses in 2026.
>Investigate and answer the email address they are likely using now.
>
>For example, if the address were foobar@example.com, the flag would be SWIMMER{foobar@example.com}.
>Warning: Do not send any emails to this address. You do not need to send any mail to answer.

前問で発見したページの一番下にメールアドレスが書かれています。

なお、このGmailアドレスをGHuntやEpieos（いずれもGoogleアカウントの情報を調査するOSINTツール）で調査すると公開されているカレンダーに予定が1件だけあるのがわかります。これは最終問題で役に立ちます。

## debeyohiru_04_meal (medium)
>debeyohiru はある料理が好物で、よく食べているようです。 直近では2026年1月10日の夕食にその料理を食べたことが確認されています。 この人物がこの日の夕食に食べたメニューを特定し、店舗のメニューに記載された名前で解答してください。
>
>例えば、マクドナルドでビッグマックを食べたことが特定できた場合、Flagは SWIMMER{ビッグマック} になります。 メニュー名の表記は店舗のメニューの 日本語 表記に準拠してください。
>
>debeyohiru has a favorite dish and seems to eat it frequently. Most recently, it was confirmed that they ate this dish for dinner on January 10, 2026.
>
>Identify the menu item they ate for dinner on this day and answer with the name exactly as listed on the restaurant's menu.
>
>For example, if it is determined that they ate a Big Mac at McDonald's, the Flag would be SWIMMER{ビッグマック}. Please ensure the menu item name follows the Japanese notation used on the restaurant's menu.

https://bsky.app/profile/debeyohiru.bsky.social/post/3mazue5yqdc2n

https://bsky.app/profile/debeyohiru.bsky.social/post/3mc2utbra622w

debeyohiruのBlueskyを見ていくと、12月28日においしそうなオムライスの写真を投稿しています。また、1月10日には「今宵も来たぞ。今日は単品でいいや」と返信しています。

写真のメニューはポムの樹で提供される「特製デミソースのダブルチーズオムライス」だと比較的容易に分かるため、ほとんどの人がこれを答えて撃沈したのではないでしょうか……。
（途中で公開されるヒントにも、 **特製デミソースのダブルチーズオムライスではない** と明記される始末でした。）

冷静に考えると、同じ店で短期間に同じメニューばかり頼むことはなかなかないでしょう。1月10日に食べたものがすぐ分かる情報源があるはずです。とはいえ、Blueskyから得られる情報はもうないようです。

当該人物は渋谷を拠点としていることがプロフィールページなどから分かるため、渋谷にある「ポムの樹 渋谷スペイン坂店」のGoogleマップレビューを新しい順に閲覧すると、「ふらいご」さんによる写真付きレビューを発見できました。

https://maps.app.goo.gl/1BRyCoyZQk1tLKss9

ポムの樹の冬季限定メニューを[公式のプレスリリースPDF](https://www.pomunoki.com/img/release/release-281.pdf)から確認し、見た目が似ている料理の名前をコピーして提出したところ正解でした。

地図サービスや旅行レビューサイトの書き込みに調査対象人物がノリノリで書き込んでいるという流れは、海外のストーリー式OSINT CTFでもたびたびあるパターンです。

## debeyohiru_05_hidden1 (medium)
>debeyohiru の本名が知りたいです。
>この人物の実名と考えられるものを解答してください。 ローマ字（アルファベット）表記で入力してください。漢字の表記を考慮する必要はありません。
>
>例えば、Sanae Takaichiが実名の場合、Flagは SWIMMER{Sanae Takaichi} となります。
>
>We want to know debeyohiru's real name.
>Answer the name you believe is their legal name.
>
>For example, if their real name were Sanae Takaichi, the flag would be SWIMMER{Sanae Takaichi}.
>Please answer in Latin alphabet; you do not need to consider Kanji or Hiragana.

プロフィールページのソースコードや、アーカイブサイトに残っていたちょっと前のページ（これは次の問題で使うものでした）のソースコードをじっくり見ても本名につながる情報は出ていませんでした。ただ、ページで読み込まれている"js/script.js"のコメント欄にauthorという表記があり、これがflagでした。

## debeyohiru_06_hidden2 (medium)
>debeyohiru が 2025年12月 時点で使用していたと考えられるスマートフォンの機種が知りたいです。
>なお、複数の端末を使用していたと考えられる場合は、アンダーバー（_）で繋いで全てを解答してください。この場合、回答順序は問いません。
>また、メーカー名は不要です。
>
>例えば、Xperia 10 VIIとiPhone 17を使用していた場合、 Flagは SWIMMER{Xperia 10 VII_iPhone 17} となります。
>
>We want to know the smartphone models debeyohiru is believed to have used as of December 2025.
>If you believe they used multiple devices, answer all of them connected with underscores (_). In this case, the order does not matter.
>You do not need to include the manufacturer name.
>
>For example, if they used Xperia 10 VII and iPhone 17, the flag would be SWIMMER{Xperia 10 VII_iPhone 17}.

最近は、個人や組織のWebサイトをOSINT CTFでみたら問答無用で各種Webアーカイブの検索にかけるようにしています。
すると、CTF当日より少し前、2026年1月2日に保存されたものがありました。

https://archive.is/ORR6S

ほとんど現在のページと変わらないように見えるものの、内容をコピーしてテキスト差分を見ると使っているスマートフォンの機種名が変化しています。
変化する前の機種名2つをアンダースコアでつなげばflagが得られます。

# tgt_lilica category
> “lilica” と名乗る人物が組織に関与していることが判明しています。組織において、デザインやリクルーティングなどを担っていると思われます。2026年頃は「黄昏ブロッサムリリカ」と名乗っていたことが分かっています。

## lilica_01_social (easy)
>lilica は2026年時点でクリエイターとして活動しており、「黄昏ブロッサムリリカ」と名乗っていたことが分かっています。 また、Xのアカウントを持っていたことが確認されています。
>
>そのXアカウントのID（スクリーンネーム）を解答してください。
>
>例えば、@gov_online が対象のアカウントの場合、Flag は SWIMMER{@gov_online} となります。
>
>In 2026, lilica was active as a creator and was known to use the name "黄昏ブロッサムリリカ".
>We have also confirmed that they had an X (Twitter) account.
>
>Please answer the ID (screen name) of that X account.
>For example, if the target account were @gov_online, the flag would be SWIMMER{@gov_online}.

「黄昏ブロッサムリリカ」などで検索するとXアカウントが見つかります。

https://x.com/twilight_lilica

## lilica_02_virtual_identity (easy)
>lilica はVRにも関心があるようで、未来でもVR関連の活動がわずかながら確認されています。
>lilica が2026年時点で使っていたVRChatのユーザーIDを特定し、解答してください。
>
>VRChatのユーザー情報はブラウザからも確認できます。
>例えば、対象アカウントのURLが https://vrchat.com/home/user/usr_xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx の場合、 Flagは SWIMMER{usr_xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx} となります。
>
>lilica is also interested in VR, and some activity has been observed in the future.
>Please answer the VRChat user ID which lilica was using as of 2026.
>
>VRChat user info can be viewed in a browser.
>For example, if the account URL were https://vrchat.com/home/user/usr_xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx, the flag would be SWIMMER{usr_xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}.

VRChatはユーザ同士の交流機能を充実させており、Webからユーザの検索や友達登録ができます。ただ、ログインしなければ使えない機能ばかりなので、VRChatアカウントを持たないプレイヤーは作成から行う必要があったことでしょう。筆者はもともとVRChatを利用していたため楽でした。

ログイン後、ホーム画面でふたたび「黄昏ブロッサムリリカ」を検索するとヒットします。

https://vrchat.com/home/user/usr_b103fac6-8341-4b89-a606-920092e75e43

## lilica_03_virtual_world (easy)
>lilica はVRChatでの活動をSNSに投稿していたようです。
>2025年11月9日（日本時間）に投稿された画像にはある「ワールド」が写っているようです。このワールドのIDを解答してください。
>
>VRChatのワールド情報はブラウザからも確認できます。
>例えば、対象のワールドのURLが https://vrchat.com/home/world/wrld_xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/info だった場合、
>Flagは SWIMMER{wrld_xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx} となります。
>
>It seems lilica posted about VRChat activity on social media.
>In an image posted on November 9, 2025 (JST), a certain "world" appears. Please answer the ID of this world.
>
>You can view VRChat world information in a browser.
>For example, if the world URL were https://vrchat.com/home/world/wrld_xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/info,
>the flag would be SWIMMER{wrld_xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}.

https://x.com/twilight_lilica/status/1987359447111639464

これは明らかにエンジンかずみ氏による著名な交流用ワールド「NAGiSA」なのでそのURLを答えます（リアル知識で殴るよくないプレイヤー）

https://vrchat.com/home/world/wrld_1b94e327-036b-4d09-81be-e898d71f02cb

もしVRChatの知識がない方でも、画像内に「ナギサ」「エンジンかずみ」といったキーワードを検索すれば発見できると思います。

## lilica_04_domain (easy)
>lilica は個人のWebサイトを運営していたようです。
>このWebサイトのドメイン名が取得された日付が知りたいです。YYYY/MM/DD の形式で解答してください。
>
>例えば、2026年1月2日 がドメイン取得日の場合、Flagは SWIMMER{2026/01/02} となります。
>
>lilica ran a personal website.
>When was its domain name registered? Answer in the YYYY/MM/DD format.
>For example, if the domain was registered on January 2, 2026, the flag would be SWIMMER{2026/01/02}.

https://x.com/twilight_lilica/status/1999551960341496014

得られたドメイン名でwhoisし、作成日を答えます。

```
$ whois twilight-lilica.com
   Domain Name: TWILIGHT-LILICA.COM
   Registry Domain ID: 3026076793_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.discount-domain.com
   Registrar URL: http://gmo.jp
   Updated Date: 2025-10-05T07:08:47Z
   Creation Date: 2025-10-05T07:08:47Z
   Registry Expiry Date: 2026-10-05T07:08:47Z
   // ...
```

## lilica_05_hosting (medium)
>lilica が運営している個人サイトは、あるホスティングサービス上に存在しています。
>このサイトは規約違反を行っているわけではないので何かできるわけではありませんが…… 念の為、連絡先を知っておきたいです。
>このホスティングサービスの規約違反通報メールアドレスを解答してください。
>例えば、メールアドレスが foobar@example.com の場合、Flagは SWIMMER{foobar@example.com} となります。
>
>警告 : このアドレスに対してメールを送信してはなりません。 解答に際して、メールを送る必要は一切ありません。
>
>lilica hosts a personal site on a hosting service.
>The site itself is not violating any terms, but for caution, we want to know the contact address of the hosting service.
>Please answer the abuse-reporting email address for this hosting service.
>For example, if the address were foobar@example.com, the flag would be SWIMMER{foobar@example.com}.
>
>Warning: DO NOT SEND EMAIL to this address. You do not need to send any emails to answer.

IPアドレスが糸口になりそうなのでとりあえずpingを打ちます。

```
$ ping twilight-lilica.com
PING twilight-lilica.com (45.77.129.141) 56(84) bytes of data.
64 bytes from 45.77.129.141.vultrusercontent.com (45.77.129.141): icmp_seq=1 ttl=56 time=4.95 ms
```

vultrusercontent.com というドメイン名が見えました。これはVultrというクラウドホスティングサービスのドメイン名のようなので、その違反報告用メールアドレスを検索で調べれば正解できました。

## lilica_06_name (hard)
>lilica の本名が知りたいです。
>この人物の実名と考えられるものを解答してください。
>ローマ字（アルファベット）表記で入力してください。漢字の表記を考慮する必要はありません。
>
>例えば、Sanae Takaichi が実名の場合、Flagは SWIMMER{Sanae Takaichi} となります。
>
>Please answer what you believe to be lilica's real name.
>Answer in the Latin alphabet (romaji); you do not need to consider Kanji / Hiragana.
>For example, if the real name were Sanae Takaichi , the flag would be SWIMMER{Sanae Takaichi}.

https://x.com/twilight_lilica/status/2000551415039791278

**XのアカウントでファイルアップローダーのURLを全世界に公開するという明らかにインターネットリテラシーに欠けた行為** がみられるので、ダウンロードして中身を確認します。

配布されていたのはFBXファイル（3Dモデルを扱えるファイルフォーマットの一種）でした。FBXファイルには作成したソフトウェアの情報やファイルのPC内でのパスがメタデータとして残る場合があります。

https://blender.atom-box.net/fbx-meta-path/

メタデータを取り出す方法はいくつかありますが、stringsコマンドでやってみると次のようになります。

```
$ strings simple_hair_pin.fbx | grep -i users
C:\Users\shiharu_nanaogi\Documents\modeling\vrc_test\hair_pin\simple_hair_pin.fbx
C:\Users\shiharu_nanaogi\Documents\modeling\vrc_test\hair_pin\simple_hair_pin.fbxD
```

実名と考えられるWindowsのユーザ名が見つかるため、整形して提出すれば正解です。

## lilica_07_work (medium)
>lilica は、Xとは別のSNSにもアカウントを所持しているようです。
>このアカウントの情報から職場の最寄り駅として推定されるものを、東京メトロの駅名 で解答してください。
>例えば、推定される職場の最寄り駅が 新宿三丁目駅 の場合、Flagは SWIMMER{新宿三丁目} となります。
>
>lilica seems to have an account on another social network.
>From that account, infer their workplace's nearest station and answer using the Tokyo Metro station name.
>For example, if the nearest station is 新宿三丁目駅 (Shinjuku-sanchome), the flag would be SWIMMER{新宿三丁目}.

実名（Shiharu Nanaogi）を使っているSNSアカウントを見つける問題と思われますが、Google検索やWhatsMyNameではなかなかヒットせず苦戦しました。ふと思いついてBingで検索したところ、なんとInstagramのアカウントが出てきました。

![bing](/images/swimmer-osint-2026-kn1cht/bing.jpg)

https://www.instagram.com/p/DSRYlK3CRfH/

https://www.instagram.com/p/DSTxI6-iRdF/

昼休みに会社を出て周辺を散歩しているとの内容があるため、投稿全体で共通する地域の駅を答えれば正解です。

# ops_swimmer category
> 多様なバックグラウンドを持つ人物が組織を構成していることが判明しています。実生活で彼ら・彼女らが接することはなさそうに思われるのですが……　このきっかけを特定することで、実態解明につなげられるかもしれません。

本カテゴリは1問だけで構成されており、3名の人物に関する一連の課題を全て解けたチームのみアクセスできました（research_2025カテゴリは必須ではないようでした）。

## ops_swimmer (hard)
>2025年のある日、rain, debeyohiru, lilica が初めて3人で集まったようです。 この日以降、この集団は活動を徐々に活発化させました。
>それはいつ・どこだったのでしょうか？
>以下の情報を教えてください。
>
>日付（YYYY/MM/DD 形式）
>集合時刻（HHmm 形式。日本時間で24時間表記）
>場所（チェーン店名と店舗名）
>例えば、2025年の10月1日の20時00分（日本時間）に、マクドナルド 新宿西口店で集合している場合、 Flagは SWIMMER{2025/10/01_2000_マクドナルド新宿西口店} となります。
>
>店舗名は 公式サイト の日本語表記に準拠します。ただし、スペース ( ) が含まれている場合、それを省いてください。
>
>On a day in 2025, rain, debeyohiru, and lilica seem to have met together for the first time.
>Their activity increased from that day onward. When and where did this happen?
>Provide the following information:
>
>Date (YYYY/MM/DD format)
>Meeting time (HHmm 24-hour format, JST)
>Place (chain name and store name)
>For example, if they met at 20:00 JST on October 1, 2025 at McDonald's Shinjuku Nishiguchi, the flag would be SWIMMER{2025/10/01_2000_マクドナルド新宿西口店}.
>
>The store name will be based on the Japanese name on its official website. Omit spaces ( ) if it is included.

rainやlilicaのXアカウントには **デニーズで食事したと思しき写真** があり、食べている料理も一致しています。集まったというのはこのことでしょう。

https://x.com/bruto_rain/status/2007384438939021503

https://x.com/twilight_lilica/status/2007451888996724926

また、 **debeyohiru_03_emailで発見したメールアドレス** をGHuntまたはEpieosで調査することで、公開されているGoogleカレンダーに「集会（説明: 18:30集合\n店の前）」という予定があり、2025年12月30日18時30分開始だったことが分かります。

あとはデニーズのどの店舗なのかが問題となります。rainの当日付近の画像投稿では、品川区の駅前で解散したことや、直前の時間帯に大井町駅で入場券を購入したことが判明します。

https://x.com/bruto_rain/status/2005982246499742052

https://x.com/bruto_rain/status/2006604801514057878

大井町駅のすぐ近くにはデニーズ大井町駅前店があり、これが正解でした。

# research_2025 category
> 我々の時代では、過去の正しい情報にアクセスできなくなりつつあります。我々が得ている情報の正確性を補正するために、2025～2026年ごろにおける実社会の状況についても調査を行ってください。

## cx (easy)
>2025年春、かつて香港に存在していた空港の100周年を記念して、特別なフライトが実施されたようです。
>このフライトの便名を解答してください。
>例えば、便名が JL2026 のとき、Flag は SWIMMER{JL2026} となります。
>
>In Spring 2025, it appears that a special flight was carried out to commemorate the 100th anniversary of the airport that once existed in Hong Kong.
>Answer the name of the flight number.
>If the flight was JL2026, the flag should be SWIMMER{JL2026}.

本カテゴリに関しては、キーワードを検索すると同時にGoogle Geminiにも問題文を渡してなにか答えさせるようにしていました。

Geminiの回答:

>2025年春、かつて香港の玄関口であった啓徳（カイタック）空港の開港100周年を記念して実施された特別フライトの便名は、CX8100です。
>このフライトは、2025年3月30日にキャセイパシフィック航空によって実施されました。かつての名物であった「香港カーブ」を彷彿とさせる低空飛行のデモンストレーションが行われ、多くの市民や航空ファンを沸かせました。

https://www.traicy.com/posts/20250329333965/

Geminiの答えが正解であることをニュースサイトで確認し、便名を答えて終了です。

## pilot (easy)
>cx の問題で示されたフライト中、添付画像の席に座っていた人物の名前を英語で解答してください。
>（なお、添付画像は座席を示すためのものであり、当該フライトの実際の写真ではありません）
>例えば、人物の名前が John Doe のとき、Flag は SWIMMER{John Doe} となります。
>
>In the flight indicated in the cx challenge, answer the name of the person who was sitting in the seat shown in the attached image in English.
>(Note that the attached image is for indicating the seat and is not an actual photo of the flight.)
>For example, if the person's name is John Doe, the flag should be SWIMMER{John Doe}.

添付写真では、コックピットの右側の席に矢印が示されています。これは副操縦士席なので、CX8100便の副操縦士を探します。

Geminiの回答:
>2025年3月30日に実施された、啓徳（カイタック）空港100周年記念フライト（便名：CX8100）において、コックピットの副操縦士席（右側席）に座っていたのは、キャセイパシフィック航空のフライト・トレーニング・マネージャー（Flying Training Manager）であるAdrian Scott氏です。

なんとこちらもGeminiが一発正解です。結構Gemini強いですね……。

裏を取る必要はあるのですが、「CX8100 pilot」で検索してヒットするLinkedInの投稿で紹介されている動画に御本人が出演しているので間違いないことがわかります。

https://www.linkedin.com/posts/catherine-zhu-0278258_behind-the-scenes-of-cx8100-a-cathay-flypast-activity-7316468822264160257-MWz3/

@[youtube](Fjs9AfnyuLg)

## flag_on_the_don (easy)
>2025年8月28日、群馬県で「太鼓の達人」を利用したイベントが開催されました。
>その会場となった建物はどこでしょうか。 OpenStreetMapのウェイ（way）番号で解答してください 。
>例えば、建物が 123456789 というway番号であれば、Flagは SWIMMER{123456789} となります。
>
>On August 28th 2025, an event using "Taiko no Tatsujin" (太鼓の達人) was held in Gunma Prefecture.
>What is the building where the venue was held? Please answer with the way number of OpenStreetMap .
>For example, if the building's way number is 123456789, the Flag should be SWIMMER{123456789}.

こちらはGemini一発チャレンジでは正答が得られませんでした。「2025年8月28日 群馬県 太鼓の達人」などと検索すると、当日に群馬県渋川市で高齢者向けeスポーツ大会が開催されたというニュースと、渋川市の資料PDFが見つかります。

https://www.jiji.com/jc/article?k=2025082800855

- [令和7年7月第4回市長定例記者会見（渋川市）](https://www.city.shibukawa.lg.jp/manage/contents/upload/68870b94eeb0d.pdf)

会場が渋川市民会館であることが分かるので、OpenStreetMapで調べてway番号を答えればOKです。

## obsolete (easy)
>2025年11月、「台湾有事」を巡って日中関係が悪化しました。
>その中で、在日本中国大使館のSNSアカウントは国連憲章の「敵国条項」について言及する投稿を行いました。
>これに対し、日本の外務省は「その条項は 1995年の国連決議 によって死文化（obsolete）している」と反論したことが報じられています。
>
>さて、この決議において、投票を 棄権 した国がいくつかあります。それはどこの国か、解答してください。
>決議に関する公式記録に記載されている国名をアンダーバー (_) で区切ったもの（順不同）をFlagとします。
>
>たとえば、UNITED STATES（アメリカ合衆国）とRUSSIAN FEDERATION（ロシア連邦）と記載されていた場合、Flagは SWIMMER{UNITED STATES_RUSSIAN FEDERATION} となります。
>
>In November 2025, relations between Japan and China worsened over a potential contingency in Taiwan (referred as "台湾有事").
>During this argument, the Chinese Embassy in Japan posted on social media about the "Enemy Clauses (敵国条項)" of the UN Charter.
>Japan's Ministry of Foreign Affairs reportedly countered that the clause had become obsolete due to a 1995 UN resolution.
>
>In that resolution, several countries abstained from the vote. Answer which countries they were.
>Use the country names as written in the official record of the resolution, separated by underscores _ (any order), for the flag.
>
>For example, if the record listed UNITED STATES and RUSSIAN FEDERATION, the flag would be SWIMMER{UNITED STATES_RUSSIAN FEDERATION}.

Geminiの回答:
>1995年 国連総会決議50/52 の投票結果
>この決議は、国連憲章から「敵国条項（第53条、第77条、第107条）」を削除する手続きを開始することを念頭に、同条項が**「死文化（obsolete）している」**と明記したものです。
>投票の結果、棄権した国は以下の3カ国です。
>
> - リビア
> - 北朝鮮（朝鮮民主主義人民共和国）
> - キューバ

ソースを示せと追撃したところ、次のような回答でした。

>1. 国連の公式記録（一次ソース）
>この決議は、1995年12月11日の第50回国連総会本会議において採択されました。
> - 決議番号： A/RES/50/52
> - 議題： 「国連憲章特別委員会および組織の役割強化に関する報告書」
> - 公式投票記録： United Nations Digital Library - Voting Record A/RES/50/52
> - このリンク先の "Abstain"（棄権） の欄には、以下の3カ国が明確に記載されています。
>     - CUBA（キューバ）
>     - DEMOCRATIC PEOPLE'S REPUBLIC OF KOREA（北朝鮮）
>     - LIBYAN ARAB JAMAHIRIYA（リビア）

Geminiがソースとして示したURL自体は全く関係ない資料のものでしたが、決議A/RES/50/52自体は実在するもので、国連のデジタルライブラリの記録を閲覧できます。

https://docs.un.org/en/A/RES/50/52

しかしこれには投票結果が載っていないので、引き続き「United Nations Digital Library Voting Record A/RES/50/52」などでググりました。その結果、UN General Assembly Resolutions Tables: 50th Session というページを発見でき、A/RES/50/52 の meeting record は A/50/PV.87 であることが分かります。

https://research.un.org/en/docs/ga/quick/regular/50

あとは国連のデジタルライブラリで A/50/PV.87 の文書を検索し、ダウンロードできるPDFでresolution 50/52に対して棄権した国名をコピーして繋げばOKです。

https://digitallibrary.un.org/record/203933?ln=en&v=pdf

なお、Geminiが回答した国名は表記も含めて正解でした。さすが。

## truck (easy)
>あるトラックが 8773X3XQ+JWQ を2025年6月21日 13:39:54（現地時間）ごろに通過しました。このトラックの車体に書かれていたFQDNを解答してください。
>たとえば、www.google.com の場合、Flagは SWIMMER{www.google.com} となります。
>
>A truck passed 8773X3XQ+JWQ around June 21, 2025 at 13:39:54 (local time). Please identify the FQDN written on the body of this truck.
>For example, if that was www.google.com, the flag would be SWIMMER{www.google.com}.

8773X3XQ+JWQというのはGoogle Mapsで使われている位置を示すコード、Plus Codeです。Google検索すると、アメリカ合衆国ノースカロライナ州ダーラム（Durham, NC）にある橋を示していました。他の国の道路での出来事を自力で調査できる気がしないのでGeminiにお願いします。

Geminiの回答:
>2025年6月21日 13:39:54（現地時間）ごろ、ご指定の場所（ノースカロライナ州ダーラム、通称「11foot8」または「Can Opener Bridge」として知られる鉄道高架下）を通過し、事故を起こしたトラックの車体に書かれていたFQDNは以下の通りです。
>
> （筆者注：間違いなので削除）
>
>事故の詳細と背景
>この場所（Plus Code: 8773X3XQ+JWQ）は、高さ制限が非常に低いことで世界的に有名な橋で、設置されたカメラによって多くの衝突事故が記録されています。
> - 発生日時: 2025年6月21日 13:39（現地時間）
> - 場所: ノースカロライナ州ダーラム、グレグソン通り（Gregson St）
> - 状況: 車高の高い引越し用トラック（筆者注：間違いなので削除）が、高さ制限の警告灯や赤信号を無視して進入し、高架を保護する鋼鉄製の梁に衝突しました。この事故は通算で187回目の衝突として記録されています。

FQDN（Fully Qualified Domain Name; www.example.comのような完全なドメイン名）まで一発で正解することはできなかったものの、説明されている場所が有名な事故多発地点であることが判明しました。

Geminiは資料としてYouTube動画を添付してくれており、これがまさに問題文で言われているトラックの事故の様子でした。

@[youtube](MJ4tpEhQ86g)

車体にドメイン名が大きく書かれているので写せば正解です。

なお、この11foot8という場所はよく見るとGoogle Mapsに地点登録されており、公式サイトまでありました。なんでだよ

https://11foot8.com/

## debris (medium)
>2025年12月8日、日本のあるテレビ番組で、高速道路のパトロール隊への密着取材の様子が放映されました。
>パトロール隊の108号車は、路上に落下していたトン袋の回収を命じられました。トン袋が最初に落下していた地点はどこでしょうか？
>マップ上で示して解答してください。
>
>content warning : 交通事故の映像が含まれる映像を視聴する必要があります（人が巻き込まれるシーンはありません）。
>
>On December 8, 2025, a Japanese TV program aired close coverage of a highway patrol unit.
>Patrol car 108 was ordered to recover a bag (トン袋, ton bukuro) that had fallen onto the roadway. Where was the bag initially dropped?
>Please answer by indicating the location on the map.
>
>content warning: You will need to watch footage that includes a traffic accident (no scenes of people being hit).

Geminiの回答:
>2025年12月8日に放送された（筆者注：間違いなので削除）テレビ系の人気特番**『（筆者注：間違いなので削除）』**（または同日のニュース・情報番組内の密着コーナー）等で、高速道路パトロール隊の活動が紹介されました。
>
>ご質問の「108号車」が回収に向かったトン袋（大型土のう）の落下地点は、以下の通りです。
>
>トン袋の落下地点
>路線名： 首都高速道路（首都高）
>
>具体的な場所： 5号池袋線（下り）の（筆者注：間違いなので削除）付近

けっこうなハルシネーションをしていますが、雰囲気は分かったのでキーワード検索していくと番組の内容をまとめた公式Web記事が見つかりました。

https://mezamashi.media/articles/-/232641

たくさん掲載されている映像の切り取り画像や、担当者の「上りの護国寺合流先、音羽」というセリフを手掛かりに首都高速5号池袋線をGoogle Mapsで見ていきます。

すると同じ景色の地点が見つかるので、回答画面の地図にピンを刺したところ正解しました。

@[codepen](https://codepen.io/kn1cht/pen/PwzpKJN)

## lattice_tower (medium)
>2025年12月、日本で地震が発生し、ある通信施設の鉄塔が被害を受けました。
>主要ニュースで報じられた名称以外に、このビルには通信施設としての別の公式な名前があるようです。
>その正式名称を日本語で解答してください。
>
>ただし、解答にアルファベットや数字が含まれている場合はそのまま答えてください（ただし、 半角英数字 にしてください）。
>また、そのビルを保有している会社名は含まず、ビル名だけがFlagになります。
>
>例えば、「SWIMMER OSINT CTF株式会社　台場SWIMMER2026ビル」だった場合、Flagは SWIMMER{台場SWIMMER2026ビル} となります。
>
>In December 2025, an earthquake struck Japan and damaged the lattice tower of a communications facility.
>In addition to the name reported in the major news articles, the building appears to have another official name as a communications facility.
>
>Please answer that official name in Japanese.
>If the answer contains letters or numbers, keep them as they are, but use ASCII alphanumerics.
>
>Do not include the company that owns the building; only the building name should be in the flag.
>For example, if the building were called "SWIMMER OSINT CTF株式会社　台場SWIMMER2026ビル" the flag would be SWIMMER{台場SWIMMER2026ビル}.

Geminiの回答:
>2025年12月8日に発生した青森県東方沖を震源とする地震（最大震度6強）で被害を受けた、青森県八戸市柏崎にあるNTT東日本のビルですね。
>
>この建物は、主要ニュースや一般的には「NTT青森八戸ビル」と報じられていますが、通信施設としての公式な名称（正式名称）は以下の通りです。
> （以下略、間違っていた）

「NTT青森八戸ビル」で検索すると地震のニュースばかりで見つけにくいので、Google検索の期間指定機能で発生前である2025年11月までの情報に絞ります。

すると、詳しい人のブログがヒットして通信局名が分かりました。

https://telecom.blog.jp/archives/02_NTTE_Hachinohe.html

本問は筆者がFirst bloodを獲得しました。

![first blood](/images/swimmer-osint-2026-kn1cht/first_blood.png)

## paint (medium)
>添付画像に写っている航空機は、ある映画にちなんだ特別塗装機です。
>この特別塗装機は、2017年3月21日に就航開始しましたが、2025年に引退が発表され、去る2026年1月9日、この塗装での最後の運航を終了しました。
>
>さて、2017年にこの特別塗装が行われた建物はどこでしょうか。座標を地図上で示してください。
>
>The aircraft in the attached image features a special livery themed after a movie.
>This livery entered service on March 21, 2017, and its retirement was announced in 2025.
>The final flight with this livery took place on January 9, 2026 (JST).
>
>Where was this special livery applied back in 2017? Please answer the building's coordinates on a map.
>
>![paint](/images/swimmer-osint-2026-kn1cht/paint.jpg)

全日本空輸（ANA）が運航していたC-3POデザインの機体です（結構有名だと思います）。

https://www.aviationwire.jp/archives/334479

普通にGoogle検索しても引退した最近のニュースしか出てこないため、lattice_towerと同様に期間指定で2017年春ぐらいまでの情報をのワードで探しました。

https://travel.watch.impress.co.jp/docs/news/1050316.html

すると、この特別塗装が行われたのは伊丹空港に隣接するMRO Japanという会社であることが分かります。

MRO Japanは現在では那覇空港に移転していてGoogle Mapsでは位置が分からないのですが、衛星写真で当時の映像と同じであろう倉庫が見つかるので、そこにピンを刺せば正解です。

## satellite_imagery (medium)
>この衛星画像が取得（撮影）された日はいつでしょうか？ YYYY/MM/DD 形式で解答してください。
>例えば2026年1月17日の場合、Flagは SWIMMER{2026/01/17} となります。
>
>When was this satellite image captured? Please answer in the YYYY/MM/DD format.
>For example, if the date was January 17, 2026, the flag would be SWIMMER{2026/01/17}.
>
>![satellite_imagery](/images/swimmer-osint-2026-kn1cht/satellite_imagery.jpg)

Google Earthのスクリーンショットが渡されました。画像内にある「美々」「ケータリングビル」などのキーワードで探すと、これが新千歳空港であることが分かります。

Google Earth Proで当該地域を見ると、画像の撮影日が画面上に表示されるためflagを得られます。

## rage (hard)
>2025年7月、メキシコシティでは住宅価格の高騰を理由とするデモが起きました。
>以下の記事URLから確認できる報道写真で破壊されている店舗がオープンした日時を、 YYYY/MM/DD （現地時間）の形式で解答してください。
>例えば、答えが2025年7月5日の場合、Flagは SWIMMER{2025/07/05} となります。
>
>In July 2025, a protest broke out in Mexico City over soaring housing prices.
>Using the news photo in the article below, answer the opening date (local time) of the vandalized store in YYYY/MM/DD format.
>For example, if the answer were July 5, 2025, the flag would be SWIMMER{2025/07/05}.
>
>記事 / Article: https://www.jiji.com/jc/article?k=2025070500264&g=int&p=20250705at46S&rel=pv

ガラスを勢いよく蹴破っているツンツンヘアーの人物の上の看板をGoogle Lensで検索すると、店の公式Instagramを発見できました。RIPNDIPという衣料品店のようです。

https://www.instagram.com/p/C_vhevVPEA-/

この投稿（2024年9月11日）では、RIPNDIPのメキシコシティ店が「この土曜日」に移転してグランドオープンするという告知がなされているので、次の土曜日である9月14日を回答しました。これは最初誤答扱いだった（後述する移転前の最初の開店日が想定解でした）ものの、筆者がadminに問い合わせたところ正解として認められました。ご対応ありがとうございました。

**（移転前の最初の開店日を見つける方法）**

メキシコシティ店のインスタを遡っていくと、デモによる破壊の数日後の投稿で「店は一時閉店している」「2022年のオープン以来、RIPNDIPメキシコは100%メキシコ人が運営している」という記述がありました。

https://www.instagram.com/p/DLz-xoAsCWR/

あとは「RIPNDIP mexico 2022 "open"」などと検索して開店の告知を見つければOKです。

https://www.instagram.com/p/Cc5rkCzu-21/
