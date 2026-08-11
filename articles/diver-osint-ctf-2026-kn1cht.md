---
title: "[40548F/kn1cht] DIVER OSINT CTF 2026 Writeup"
emoji: "🤿"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - diverosintctf
  - OSINT
  - CTF
  - writeup
published: true
---

[DIVER OSINT CTF 2026](https://diverctf.org/)のWriteup。

# 開催概要・制度
* 2026年7月25日12:10 JSTから7月26日12:10 JST（24時間）
* Jeopardy方式
* 問題数：52問
* チーム制、6人まで
* 点数は各問題の正答数に応じて減少するDynamic方式

# Writeups
Team 40548Fとして、blackwasanさんとkn1chtの2名で参加した。10244 pointsを取得し、24位となった。

![チームの結果。10244 pointsを取得し、24位。](/images/diver-osint-ctf-2026-kn1cht/40548f.png)

このWriteupでは、筆者が主に取り組んだ課題（crypto, disinfo, geo, hardware, history, misc, recon, transportationカテゴリ）について解法を示す。競技中に解けなかった問題には原則言及しない。見出しに示した点数は、CTF終了後のもの。問題サーバーから文章や提供されたデータを引用している。

:::message
Writeup文章の一部はOpen AI Codexを通じてGPT-5.6が生成した。
内容の明確さや正確性を筆者がチェックし、編集したうえで公開している。
:::

# welcome
ルールの確認や誓約に関する課題が6問。問題文を読み理解することがメインなので省略。

# crypto

## druk (407pt / 110 solves)

### 課題

> 2023年、ブータンのダガナ県に、政府によって建設されたと思われるビットコインの大型マイニング施設があることが報じられた。具体的な位置を地図上で示せ。
>
> In 2023, it was reported that a large Bitcoin mining facility, believed to have been constructed by the government, existed in Dagana District, Bhutan. Indicate its exact location on a map.

### 解法

このニュースはForbesが[Bhutan Built A Bitcoin Mine On The Site Of
Its Failed ‘Education City’](https://www.forbes.com/sites/iainmartin/2023/11/22/bhutan-secret-bitcoin-mine-locations/?sh=c0a06e34a4e2) として報じたもの。"in the rural region of Dagana"を写した衛星写真があるので、これをもとに探すことにした。Forbesの報道ではa middle schoolの近くであるとも言われていたので、広いDaganaの中で探す手がかりとして重要そうである。

ただ、しばらくGoogle Mapsを見て回っても発見できなかった。他の問題に使う時間の兼ね合いもあるのでOpenAI Codexに捜索を依頼したところ、検索や衛星写真のチェックを駆使して発見してくれた。

記事で言われていた中等学校はGesarling Central Schoolで、そのすぐ東南東に報道と外観が一致する建物群がある。

@[codepen](https://codepen.io/kn1cht/pen/019f9d19-62c9-76c2-97fd-41786266dce4)

![回答画面](/images/diver-osint-ctf-2026-kn1cht/druk.png)

:::details flag (Click to expand)
```
26.910102, 89.916861
```
:::

## stable (339pt / 144 solves)

### 課題

> **`0x5Ec2bb2D56FC8CC4d05110211EcaBDC8fAe3B128`**
>
> お店の名前を現地語で答えよ。
> たとえば、店名が現地語で향택 부산전포점のとき、Flagは `Diver26{향택 부산전포점}` となる。
>
> Answer the name of the shop in the local language.
> For example, if the name of the shop is 향택 부산전포점, the flag should be `Diver26{향택 부산전포점}`.

### 解法

暗号資産のアドレス形式は複数チェーンで使える。最初にチェックしたEthereumでは取引がなかったが、Polygonという暗号資産のブロックエクスプローラで確認すると多数の取引があった。

https://polygonscan.com/address/0x5Ec2bb2D56FC8CC4d05110211EcaBDC8fAe3B128#tokentxns

もっぱらJPYC（日本円に連動するステーブルコイン・電子決済手段）を受け取ったり換金したりしているので、最近普及が始まっているJPYC決済可能な店舗の1つと推定できる。

そもそも受け取っている額や取引数がかなり多い。有志が管理している「JPYC決済利用状況」を見ると、「千房 有楽町ビックカメラ支店」が圧倒的に一位なのでおそらくこれであろうと考えた。千房は有名なお好み焼き屋で、2026年4月からJPYCでの支払いを受け付けている。

https://www.chibo.com/news/%E3%80%8C%E5%8D%83%E6%88%BF%E3%80%8D%E3%81%A7%E3%82%B9%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB%E3%82%B3%E3%82%A4%E3%83%B3%E6%B1%BA%E6%B8%88%E3%82%B9%E3%82%BF%E3%83%BC%E3%83%88%EF%BC%81%E3%81%8A%E6%94%AF/

ただ、裏付けが欲しいので、この店舗での利用記録と照合するためSNSやGoogleで検索した。すると、Karahiro氏が4月14日に公表した記事で、1.550JPYCを支払済みの画面が写っていた。

https://note.com/karahiro/n/n928651285840

ブロックエクスプローラの履歴でも4月前半に1,550JPYCの取引があるため、確信はないがこの店舗である可能性が相対的に高い。店名をFlagとして提出したところ正解だった。

店舗のアドレスが2次元バーコードなどから分かる手段を発見できていれば、推測で解かずに済んだかもしれない。なお、本問は筆者がFirst bloodを取得した。

![kn1cht (40548F) solved stable (1st solve)](/images/diver-osint-ctf-2026-kn1cht/first_blood.png)

:::details flag (Click to expand)
```
Diver26{千房 有楽町ビックカメラ支店}
```
:::

# disinfo

## rch (500pt / 124 solves)

### 課題
要約：コールサインRCH4599で5月1日に日本から中華人民共和国方面に飛行した米軍の輸送機が何のために何を運んでいると推定されるか、一文で簡潔に答える。

![flight RCH4599](/images/diver-osint-ctf-2026-kn1cht/rch.jpg)

:::details かなり長いため折り畳み

> **この問題に限り、7月26日（日曜日） AM 8:00 (JST) で解答を締め切ります。それ以降の送信は採点されません。**
>
> OSINTリサーチャーであるあなた宛に、メッセージが送られてきたようだ。
>
> これは米軍機が北朝鮮か中国に荷物を運んでいる秘密のフライトの証拠だ！彼らは対立しているかのように見せかけ、実際には裏で繋がっているんだ！
>
> この主張は正しくない。実際には何のために何を運んでいると推定されるか。
>
> 採点基準:
>
> - 「何のため」（目的）および「何を運んでいるか」（貨物）を **1文で簡潔に** 明記せよ。貨物の名称は原則として正式名称を用いること。
>
> - もっとも主要な貨物を答えよ。例えば、貨物の輸送に際して、書類が同時に運ばれているとしても、「紙」「書類」などと答えたものは得点にならない。
>
> - 解答は英語または日本語で行うこと
>
> - この問題に関しては"Diver26{}"のフォーマットは不要である
>
> **なお、この問題の提出可能な回数は、1 回のみとなります。**
> もし誤送信をした場合、この問題については手動で取り消し対応を行います（1度のみ）。その場合は #ask-for-admin から問い合わせてください。
>
> ---
>
> **Only for this challenge, the deadline for submitting answers is 23:00 (UTC) on Saturday, July 25. Submissions received after this time will not be graded.**
>
> It appears a message has been sent to you, an OSINT researcher.
>
> This is proof of a secret flight where a U.S. military aircraft is transporting cargo to North Korea or China! They pretend to be adversaries, but in reality, they are colluding behind the scenes!
>
> This claim is incorrect. What is actually presumed to be the purpose of the transport, and what is being carried?
>
> Grading Criteria:
>
> - Clearly state the purpose ("what it is for") and the cargo ("what is being transported") **in one concise sentence** . As a general rule, use the cargo's official name.
>
> - State the primary cargo. For example, if documents are being transported alongside the cargo, answers such as "paper" or "documents" will not receive points.
>
> - Answers must be provided in English or Japanese.
>
> - For this challenge, the flag format "Diver26{}" is not necessary.
>
> **Please note that you can only submit this challenge once.** If you submit an answer by mistake, we can manually cancel the submission **once only for this challenge**. Please contact us through #ask-for-admin.

:::


### 解法

画像から分かるコールサインRCH4599をGoogle検索すると、航空ウォッチャーなどが、2026年5月のトランプ大統領訪中のための輸送であるとSNS投稿しているのが分かる。

@[tweet](https://x.com/MoloWarMonitor/status/2050501107927892304)

SNS投稿だけでは根拠として薄いので、中国側のニュースサイト等も見て確認したところ、確かに登録番号05-5140のC17が北京に着陸したことが写真付きで報道されていた。トランプ大統領の中国訪問の事前準備と推測されることも言及されていた。

https://www.i-meihua.com/Article/Detail/48288

主たる貨物としては、大統領が移動に使用する装甲リムジンや大統領車列向けの警備車両が考えられる。実際、5月7日の中国側報道で、米国政府のプレート付きの黒いSUVが北京の道路で目撃されたと伝えられている。

https://www.scmp.com/news/china/diplomacy/article/3352682/has-beast-arrived-beijing-trumps-motorcade-suvs-spotted-visit


設問は主たる貨物の正式名称を求めている。よって米国大統領専用車Presidential State Car）を運搬したという推定で回答した。

:::details answer (Click to expand)
2026年5月13日から15日にかけてのトランプ米大統領の中国国賓訪問の事前準備のために、米国大統領専用車（Presidential State Car）等の車両を運搬したものと推定される。
:::

# geo

## elevation (481pt / 51 solves)

### 課題

> Aidu風力発電所の建設が長年差し止められた原因の一つであった軍事レーダーのレドームについて、公的情報から入手可能な絶対標高（m）の最高値を示せ。データから得られた値を小数第二位まで回答すること。 例えば、東京タワーについて入手できたデータが351mだった場合、Flagは `Diver26{351.00}` である。
>
> The construction of the Aidu wind farm in Estonia was blocked for years, partly due to a military radar. What is the highest absolute elevation (in meters) of the radome of this radar obtainable from publicly available data? Submit the value obtained from the data to two decimal places. For example, if the available data for Tokyo Tower indicated 351 m, the flag should be `Diver26{351.00}`.

### 解法

本問については大部分をOpenAI Codex (GPT‑5.6 Terra)に任せて回答した。

Aidu風力発電所と軍用レーダーの関係を調べると、対象はエストニア・Kellavereのレーダーだった。現地報道によると、レドームの高さは12.5メートル。

https://www.delfi.ee/artikkel/50952617/kellaveres-avati-ohuruumi-seire-radar

エストニアでは政府機関Estonia's Land and Spatial Development BoardによってLiDARで測定された標高データ（LAZファイル）が公開されている。特定の図郭を選ぶ方式だが、Codexがタイル番号551644の特定・ダウンロード・Pythonでの解析まで実行してくれていた。

https://geoportaal.maaamet.ee/eng/spatial-data/elevation-data/download-elevation-data-p664.html

Codexが初めに出した回答は、2022年データにレドームそのものが反射点として含まれていないことから、その下の台座の最高点の高さとレドーム高12.5 mを足したものだった。これは小数点第2位があいまいであるため怪しいと思いつつ提出すると、やはり不正解だった。

そこで過去年度のLAZデータを含めて分析したところ、2009年、2012年、2013年のデータには、レドーム上面らしき点が直接含まれていた。

```python
"""Compare public LiDAR acquisitions over the Kellavere radar pedestal."""

from pathlib import Path
import laspy
import numpy as np

X_MIN, X_MAX = 644350.0, 644430.0
Y_MIN, Y_MAX = 6551630.0, 6551730.0

for path in sorted(Path(__file__).parent.glob("551644_*_*.laz")):
    points = laspy.read(path)
    x, y, z = np.asarray(points.x), np.asarray(points.y), np.asarray(points.z)
    classification = np.asarray(points.classification)
    inside = (X_MIN <= x) & (x <= X_MAX) & (Y_MIN <= y) & (y <= Y_MAX)
    local = np.flatnonzero(inside)
    highest = local[z[local].argmax()]
    print(
        f"{path.name}: {z[highest]:.2f} m "
        f"at ({x[highest]:.2f}, {y[highest]:.2f}), "
        f"class {int(classification[highest])}"
    )

# output
# 551644_2009_tava.laz: 177.88 m at (644395.78, 6551674.84), class 1
# 551644_2012_mets.laz: 177.51 m at (644396.93, 6551673.59), class 1
# 551644_2013_tava.laz: 178.02 m at (644397.13, 6551674.29), class 6
# 551644_2018_tava.laz: 162.40 m at (644386.13, 6551688.93), class 1
# 551644_2020_mets.laz: 162.39 m at (644386.35, 6551688.41), class 2
# 551644_2022_tava.laz: 162.56 m at (644386.83, 6551688.17), class 2

```

レドーム上面をとらえている中で最大値である178.02 mをFlagとして提出したところ、正解した。

目視チェックのためCodexに作成させた可視化画像でも、確かにレドームの最上部であろうことがわかる。

![2013年のレドーム点群](/images/diver-osint-ctf-2026-kn1cht/elevation.png)
*[Data from Republic of Estonia Land and Spatial Development Board](https://geoportaal.maaruum.ee/opendata-licence)*

:::details flag (Click to expand)
```
Diver26{178.02}
```
:::

## stargazing (417pt / 104 solves)

### 課題

> 動画 / Video: [https://www.youtube.com/watch?v=9ijHd0knEww](https://www.youtube.com/watch?v=9ijHd0knEww)
>
> 星街すいせいの楽曲「星をみる少女」のミュージック・ビデオでは、彼女が星空が見える場所へ旅する様子が描かれている。
> このミュージック・ビデオで、道路上の歩道橋が映っているシーンがある。この歩道橋の至近に、近年、カフェがオープンしている。その店名をラテン文字表記で答えよ。
> 例えば、カフェの店名が Davis Street Espresso であった場合、Flagは `Diver26{Davis Street Espresso}` となる。
> もし複数のカフェがある場合、歩道橋に最も近いものが答えとなる。
>
> The music video for Hoshimachi Suisei's song "*The Stargazing Girl*" (星をみる少女) depicts her journey to a place where you can see the starry sky.
> In this music video, there is a scene showing a pedestrian overpass over a road. Near this overpass, a café has recently opened. Answer with the name of the café in Latin characters. For example, if the café name is Davis Street Espresso, the flag should be `Diver26{Davis Street Espresso}`.
> If there are multiple cafes, the one closest to the overpass is the answer.

### 解法
本問は途中からチームメイトのblackwasanさんに引き継いでFlagにたどり着いていただいた。

課題にあるミュージックビデオは星街すいせいさんとJTBのコラボにより撮影されたもので、カザフスタンの荒野に星を見に行くというストーリー。

@[youtube](9ijHd0knEww)

https://www.jtbcorp.jp/jp/newsroom/2026/06/30_jtb_projectstargazer.html

歩道橋の場面は1:14付近からで、カザフスタンの都市の道路が短時間写る。右手にある看板が"Q?ra Qa?an"と読めるが、ぼやけていて確定しない。Google Mapsでカザフスタンの都市アルマトイの道路を見て回ったところ、街の雰囲気や歩道橋の規格が確かに似ており、アルマトイのどこかであろうと考えた。

ただ、衛星写真やストリートビューで発見できなかったためblackwasanさんにバトンタッチ。Overpass turbo でアルマトイ周辺の歩道橋を洗い出すなどした結果、有望な地点が発見された。

@[codepen](https://codepen.io/kn1cht/pen/019f9db1-36d5-7856-82fc-b157bbc553ea)

ここにはGoogleストリートビューの画像がなかったものの、Yandex MapsのPanorama写真であれば存在したため、筆者も確認して地点を確定させた。

@[codepen](https://codepen.io/kn1cht/pen/019f9db2-b421-7890-bf7b-2e576dec3780)

近傍のカフェを回答する必要があるが、Yandex Mapsでは"Qara Qazan"と"Lagmankhana-halal"の2店が至近距離に登録されており、どちらとも取れる。運営からもFlagが追加されたとアナウンスされていたこともあり、どちらも正解になると踏んで提出、正答した。

:::details flag (Click to expand)
```
Diver26{Lagmankhana-halal}
```
:::

## kaitai1 (450pt / 81 solves)

### 課題

> この電柱の、すぐ奥に見えている土地の **[地番](https://ja.wikipedia.org/wiki/%E5%9C%B0%E7%95%AA)** を **半角数字** で答えよ。
> なお、[住居表示](https://ja.wikipedia.org/wiki/%E4%BD%8F%E5%B1%85%E8%A1%A8%E7%A4%BA) の実施状況にかかわらず **地番** で解答せよ。
> 例えば、`東京都新宿区西新宿二丁目318-18` の場合、Flag は `Diver26{318-18}` となる。
>
> Answer the ["land lot number" (地番, chiban)](https://ja.wikipedia.org/wiki/%E5%9C%B0%E7%95%AA) in **numeric** form for the plot of land visible just behind this utility pole.
> Use the "land lot number," regardless of the status of the [residential addressing system (住居表示, jūkyo-hyōji)](https://ja.wikipedia.org/wiki/%E4%BD%8F%E5%B1%85%E8%A1%A8%E7%A4%BA).
> For example, if the address were `318-18 Nishi-Shinjuku 2-chome, Shinjuku-ku, Tokyo`, the flag should be `Diver26{318-18}`.

![kaitai1](/images/diver-osint-ctf-2026-kn1cht/kaitai.jpg =450x)

### 解法

地番問題。DIVER OSINT CTF 2024 "chiban"でも地番を回答する出題があり、場所を特定してから「MAPPLE法務局地図ビューア」等で地番を割り出すと踏んで調査開始。

写真には「ねこかいたいね」と落書きされた電柱が写っている。Google Lensによって日本のSNSで話題の場所だということが分かるので、Xの投稿をいくつか見ると、昼間で周辺の様子がわかるものがあった。

@[tweet](https://x.com/mitanimmk/status/1964671758197498128)

ここからは様々な位置特定方法があるようだが、筆者は背景の「～チンコ～ナミ」と読める看板を手がかりにした。看板の文字を全ては読めないが、Google Lensによる画像検索で、これが下北沢にかつて存在した「パチンコミナミ」の看板であることが分かる。

さらに画像検索を進めると、このビルは現在写真撮影用のスタジオとなっていて、屋上の看板も残されていた。

https://inthe-studio.jp/otherspace/

スタジオの住所「東京都世田谷区北沢2-9-21」をGoogle Mapsで確認し、SNSの写真と同様の角度で見えるあたりをGoogleストリートビューで捜索すると、写真の電柱を特定できた。

@[codepen](https://codepen.io/kn1cht/pen/019f9dbc-fed3-7989-99db-03939c06077d)

この電柱のすぐ奥の土地の地番を回答する必要があるが、「MAPPLE法務局地図ビューア」では地番が出なかった。そこで「地番 地図」で検索して出てきたchiban.ioで「東京都世田谷区北沢２丁目」を検索して区画を確認した。

https://chiban.io/sketch?v=2025&o=moj-13tokyo&f=13112-0109-74.xml

周辺道路の形状を見ながら、前の道路に対して細長く伸びる該当区画の地番が959-23であるのを確認して提出した。

:::details flag (Click to expand)
```
Diver26{959-23}
```
:::

## kaitai2 (390pt / 119 solves)

### 課題

> kaitai1 の土地は、ある建物が解体され、更地になっているようだ。この解体はある計画に起因すると推定される。
> その計画が完了した時、この場所に建設される物は何だろうか。
> その事物を示す Wikidata IDを答えよ。例えば、`Q57965` が答えの場合、Flagは `Diver26{Q57965}` となる。
>
> - 可能な限り具体的な事物を示せ。例えば、「東京スカイツリー」が答えであるとき、「建物」「塔」「電波塔」などは答えとして認められない。
>
> - 競技開始後に作成されたIDは解答にならない。
>
> The site of "kaitai1" appears to have been cleared following the demolition of a building. This demolition is presumed to be part of a specific project.
> What will be constructed on this site once the project is complete?
> Answer the Wikidata ID for that entity. For example, if the answer were `Q57965`, the flag should be `Diver26{Q57965}`.
>
> - Provide specific items whenever possible. For example, if the answer is `Tokyo Skytree`, terms such as `building`, `tower`, or `broadcast tower` will not be accepted as correct answers.
>
> - Wikidata IDs created after this competition has started will not be accepted as valid answers.

### 解法

kaitai1を解く過程で周囲のGoogleストリートビュー画像に再開発事業の看板が出ていたため、そこから当地周辺に新しい道路ができることが読み取れた。

- 東京都市計画道路幹線街路補助線街路第54号線
- 東京都市計画道路世田谷区画街路第10号線

世田谷区のウェブサイトで、今回の場所に通る予定の道路が「補助第54号線」の方であるのが分かるので、Wikidataで検索してFlagを得た。

https://www.city.setagaya.lg.jp/01205/4565.html

https://www.wikidata.org/wiki/Q11525315

:::details flag (Click to expand)
```
Diver26{Q11525315}
```
:::

# hardware

## yonezu1 (100pt / 316 solves)

### 課題

> 音楽家の米津玄師は、サメを模した乗り物を用いたパフォーマンスを行った。
> 公開情報から判断できる限りで、この乗り物の駆動プラットフォームはどの企業の製品を利用していると考えられるか。**英語表記** で答えよ。
> 例えばボーイング社が答えの場合、flag は `Diver26{Boeing}` となる。
>
> The musician Kenshi Yonezu (米津玄師) conducted a performance featuring a vehicle modeled after a shark.
> Based on publicly available information, which company's products are believed to be used in the drive platform of this vehicle? Answer with the company name in **English**. For example, if that company is Boeing, the flag should be `Diver26{Boeing}`.

### 解法

Codexに依頼した結果、18秒でWHILLだと言ってきた。速すぎる。

SNSに投稿されている米津氏のパフォーマンス動画を見ると、下に黒いオムニホールが見えていた。

@[tweet](https://x.com/MAJ_CEIPA/status/2065834168655954268)

Xでも複数の方がWHILLのオムニホイールだろうと話し合っていた。公式資料は発見できなかったが、状況からWHILLの可能性が高いと判断し提出、正答した。

:::details flag (Click to expand)
```
Diver26{WHILL}
```
:::

## yonezu2 (100pt / 245 solves)

### 課題

> 米津玄師が搭乗していたサメ形の乗り物を動作させている製品の制御仕様は公開されていると考えられる。
> この製品を回転させる際に使用されるコマンドとそのIDを確認してほしい。
> 例えば、 `IrisOut` という **コマンド** が `0xE2` という **コマンド ID** で定義されている場合、Flagは `Diver26{IrisOut_0xE2}` となる。
> コマンド名およびIDは仕様書に書かれている通りに解答せよ。
>
> The control specifications for the product that operates the shark shaped vehicle ridden by Kenshi Yonezu are considered to be public. Identify and answer the **command** and its **command ID** used to rotate this product. For example, if the command is `IrisOut` and it is registered as command ID `0xE2`, the flag should be `Diver26{IrisOut_0xE2}`. Answer the command name and ID exactly as specified in the documentation.

### 解法

Codexが、WHILLの公開プロトコル仕様をGitHubで発見した。

https://github.com/WHILL/whill_control_system_protocol_specification

PDFを見ると、SetVelocity (0x08)で前後左右の速度を指定できると書かれているので題意を満たす。

:::details flag (Click to expand)
```
Diver26{SetVelocity_0x08}
```
:::

# history

## tuition (483pt / 48 solves)

### 課題

> 添付画像で示されているロボットは、ある現代芸術家の作品である。この芸術家が、1961年2月20日に教育機関へ支払った金銭はいくらか。
> 通貨コード（ **ISO 4217コード** ）と共に答えよ。 たとえば、123.40米ドルの場合、Flagは `Diver26{123.40USD}` となる。通貨コードと数値の間にスペースは不要である。
>
> The robot shown in the attached image is the work of a contemporary artist. How much money did this artist pay to an educational institution on February 20, 1961?
> Answer with the amount and the currency code (**ISO 4217 code**).
> For example, if the answer is 123.40 US dollars, the flag should be `Diver26{123.40USD}`. No space is needed between the currency code and the value.

### 解法

添付画像のロボットが何なのか見当もつかないのでCodexに依頼したところ、ナム・ジュン・パイクの作品と特定してきた。すごい。

写真では一部が切り抜かれておりGoogle Lensで出にくいが、これはナム・ジュン・パイク (Nam June Paik)のRobot　K-567という作品。

https://www.mutualart.com/Artwork/K-567-Robot/86394035A4A8C36498DBEC50A2AE1150

Codexはさらに調査を進め、作者の学歴からケルン大学の学生手帳にたどり着いた。ただ、全ページの公開資料にたどり着けておらず、Flagは得られていなかった。

https://www.macba.cat/en/obra/a05633-studienbuch-ausgestellt-von-der-universitat-zu-koln-fur-nam-june-paik/

"Studienbuch ausgestellt von der Universität zu Köln für Nam June Paik"でGoogle検索すると、[この資料の全ページスキャンPDF](https://monoskop.org/images/d/d9/Paik_Nam_June_Studienbuch_ausgestellt_von_der_Universitaet_Koeln.pdf)が見つかるので、中身を読むことで大学への支払額を特定できた。

なお、当時の通貨はドイツマルクで、通貨コードはDEM。

:::details flag (Click to expand)
```
Diver26{97.50DEM}
```
:::

## accident (398pt / 115 solves)

### 課題

> 1955年、アメリカの軍人であるMiguel A. Pedrazaは運転中の事故によって死去した。 このとき彼が乗っていた車のナンバーを教えよ（ハイフン・スペースなし）。 たとえば、ナンバーが`ABC-123`である場合、flagは `Diver26{ABC123}`となる。
>
> In 1955, Miguel A. Pedraza, an American serviceman, died in a traffic accident. Answer the license plate number of the car he was driving at this time (without hyphens or spaces). If the plate number is `ABC-123`, the flag should be `Diver26{ABC123}`.

### 解法

本問はCodexがすべて自力で解決した。

最初に人物名をそのまま検索すると同名の記録が多かったため、没日と軍歴を墓地インデックスで確認した。Miguel Angel Pedrazaが1955年5月4日に亡くなった米陸軍大尉であることが、プエルトリコの墓地への埋葬者一覧に記載されていた。

https://www.interment.net/united-states/puerto-rico/veterans-burials/records.php?page=328&sort=za

次に、事故の詳細を調べるため当時の新聞が残されていないか調査した。その結果、事故から数日後の1955年5月6日付のエル・ムンド紙の縮刷版に事故の記載があった。

事故車両がChrysler 39-735と明記されているので、形式に合わせてFlagを得た。

https://gpa.eastview.com/crl/elmundo/?a=d&d=mndo19550506-01.1.1

:::details flag (Click to expand)
```
Diver26{39735}
```
:::

# misc

## sunny_saturday (436pt / 91 solves)

### 課題

> 映像 / Video: [https://www.youtube.com/watch?v=XNHPGva-RCQ](https://www.youtube.com/watch?v=XNHPGva-RCQ)
>
> この映像はある晴れた土曜日に撮影された。撮影した日時として考えられるものをYYYY-MM-DD_HH:mm形式（現地時間）で解答せよ。
> 例えば、現地時間の2022年7月25日18時47分に撮影された場合、flagは `Diver26{2022-07-25_18:47}` となる。
>
> なお、時刻については3分までの誤差を許容している。正答が18時47分である場合、18時45分や18時50分も正解Flagとして設定されている。
>
> This video was filmed on a clear Saturday. Answer with a possible date and time of filming in `YYYY-MM-DD_HH:mm` format (in local time).
> For example, if it was filmed at 18:47 local time on July 25, 2022, the flag should be `Diver26{2022-07-25_18:47}`.
>
> Note that a margin of error of up to 3 minutes is allowed for the time. If the correct answer is 18:47, then 18:45 or 18:50 are also accepted as correct flags.

### 解法

映像の場所は銀座・数寄屋橋交差点。360度撮られているので、行った記憶があれば探すまでもないはず。

映像の後半には時計が映っており、16時14分と読み取れた。また、Ginza Sony Parkの外壁が「100.80.60.展」のデザインとなっていて、この映像が展示の会期4月24日から5月31日付近と推定できた。

https://prtimes.jp/main/html/rd/p/000000484.000000353.html

Xの画像を見ていくと、5月15日の個人の投稿ではSony Parkの外壁は別の展示（ROUGE HERMÈS POPUP EVENT）のものに変わっていたため、日付は4月25日、5月2日、9日の3択に絞られた。

ここからの絞り込みは周辺でのイベント・広告・服装など多様な視点で可能だったようだ。筆者は次の3点から4月25日ではないかと推定した。

- 映像では晴れだが、筋状の雲が少量出ている。[日本気象協会Webサイト](https://tenki.jp/past/2026/04/25/satellite/japan-near/)で気象衛星画像を見比べたところ、5月2日16時は東京上空に雲が少なすぎるように見えた。
- 映像には太陽とそれによる影が写されている。[国立天文台歴計算室のシステム](https://eco.mtk.nao.ac.jp/cgi-bin/koyomi/cande/horizontal.cgi)で16:14の太陽方位を計算すると、4月25日は約268度、5月2日は約271度、5月9日は約273度となっている。影の方向から太陽は真西より少しだけ南側に見えるため、4月25日が最有力。5月9日は方位が違いすぎるため除外。
- 5月2日は日本の大型連休ゴールデンウイークに入った時期。連休の銀座は大混雑するはずだが、映像では極端に混雑している様子を感じない。

そのほかSNS投稿も慎重に確認し、最も矛盾が少ない4月25日を回答した。

:::details flag (Click to expand)
```
Diver26{2026-04-25_16:14}
```
:::

## paint (400pt / 114 solves)

### 課題

> [https://artexpress.co.ke/product/the-lady-at-the-hotel/](https://artexpress.co.ke/product/the-lady-at-the-hotel/)
>
> このサイトで "The lady at the hotel" と称されている絵画は、反転された同じ構図のものが別の題名で紹介されていることが確認されている。
> この絵画の **作者（英語表記）** と **反転された画像で紹介されている題名（英語表記）** をアンダーバー（`_`）で区切って答えよ。
> 例えば Leonardo da Vinci の Adoration of the Magi が答えであるとき、`Diver26{Leonardo da Vinci_Adoration of the Magi}` がflagとなる。
>
> It has been confirmed that the painting referred to as "The lady at the hotel" on this site is also presented under a different title with the same composition but in a reversed orientation. Answer the **artist's name (in English)** and the **title used for the reversed image (in English)**, separated by an underscore (`_`). For example, if the answer were *Adoration of the Magi* by Leonardo da Vinci, the flag should be `Diver26{Leonardo da Vinci_Adoration of the Magi}`.

### 解法

課題の文章にある通り、画像を左右反転してGoogle Lensにかけると、その絵がNairobi National Museumにて展示されていることが分かった。

https://www.instagram.com/p/DSj3N3JDM_2/?img_index=10

画像を拡大して作品名「The Maiden」、作者名「James Kamande」を特定し、本人のInstagram投稿も確認できたため正答と判断した。

https://www.instagram.com/p/DT2MzabjI0N/

:::details flag (Click to expand)
```
Diver26{James Kamande_The Maiden}
```
:::

## buyout (320pt / 152 solves)

### 課題

> 2024年4月、ザンビアにて大規模な詐欺拠点が摘発された。 高級住宅地に立地するこの拠点は、2026年現在ではホテルとして営業しているようだ。 このホテルの屋号を答えよ。 例えば、屋号が `Diver Premiere Hotel` であったとき、Flagは `Diver26{Diver Premiere Hotel}` となる。
>
> In April 2024, a large-scale fraud operation was raided in Zambia. Located in an upscale residential area, this former base appears to be operating as a hotel as of 2026. Answer the name of this hotel. For example, if the name of the hotel is `Diver Premiere Hotel`, the flag should be `Diver26{Diver Premiere Hotel}`.

### 解法

Codexの調べにより拠点がザンビアのLusakaにある道路Kasangula Road沿いと判明した。ただ、道路沿いにはいくつも宿泊施設があるため、特定できていなかった。

https://www.zambiamonitor.com/16-chinese-nationals-fined-k20000-each-for-working-without-permits-in-lusaka/

詐欺に使われていた会社名での検索や、ザンビアの裁判所の記録文書の探索などを行ったが解決せず、事件の報道やSNS投稿を見比べていたところ、詐欺グループ逮捕時の新たな写真を発見した。写真には緑の屋根の建物と、プールが見える。

https://www.lusakatimes.com/2024/04/11/cybercrime-bust/

これをGoogle Lensに読ませたところ、なんとAI概要によって建物の外観が同じ宿泊施設のURLが即座に判明した。

![プールを手掛かりにした照合](/images/diver-osint-ctf-2026-kn1cht/gemini_buyout.png)

:::details flag (Click to expand)
```
Diver26{Gianpiero's Bed and Breakfast}
```
:::

## nui (100pt / 242 solves)

### 課題

> 写真のぬいぐるみは、いつ、どのような機材で撮影されたものか答えよ。 2020年11月23日にCanon PowerShot SX70 HSで撮影されたものであれば、Flagは`Diver26{20201123_Canon PowerShot SX70 HS}`となる。
>
> Answer when the photo of the plush toy was taken and what equipment was used. If it was taken on November 23, 2020, with a Canon PowerShot SX70 HS, the flag should be `Diver26{20201123_Canon PowerShot SX70 HS}`.

![nui](/images/diver-osint-ctf-2026-kn1cht/nui.jpg =300x)

### 解法

添付画像のメタデータには撮影日時が残っていない。ただ、Google LensによってこれがNASAのArtemis IIに関する写真だと分かるため、NASAのブログから元画像を取得してメタデータを見ることで回答できた。

https://www.nasa.gov/blogs/missions/2026/03/27/artemis-ii-crew-arrives-at-launch-site-shares-moon-mascot/

```bash
$ wget https://www.nasa.gov/wp-content/uploads/2026/03/nasa-artemis-ii-zgi.jpg
$ exiftool nasa-artemis-ii-zgi.jpg
ExifTool Version Number         : 12.76
File Name                       : nasa-artemis-ii-zgi.jpg
Directory                       : .
File Size                       : 616 kB
File Modification Date/Time     : 2026:03:28 00:54:29+09:00
File Access Date/Time           : 2026:07:25 15:33:40+09:00
File Inode Change Date/Time     : 2026:07:25 15:33:39+09:00
// ...
Make                            : samsung
Camera Model Name               : Galaxy S23 Ultra
Exposure Time                   : 1/60
F Number                        : 2.2
Exposure Program                : Program AE
ISO                             : 125
Exif Version                    : 0220
Date/Time Original              : 2026:01:28 14:48:28
Create Date                     : 2026:01:28 14:48:28
// ...
```

:::details flag (Click to expand)
```
Diver26{20260128_samsung Galaxy S23 Ultra}
```
:::

# recon

## shopping1 (100pt / 313 solves)

### 課題

> ある EC サイトを利用したユーザーから、「このサイトで登録したクレジットカードの情報が漏洩しているようだ」という通報が複数件届いた。
> 調査したところ、このサイトのページの一部が改竄されているようだ。
> 配信元として悪用されているライブラリの**ソースコードを公開している GitHub アカウント名**を特定せよ。
> 例えば、アカウント名がexampleであった場合、Flagは `Diver26{example}` となる。
>
> 対象サイト: [https://shop.attic-findings.com/](https://shop.attic-findings.com/)
>
> We have received multiple reports from users of an e-commerce site, claiming that the credit card information they registered on the site appears to have been leaked.
> Upon investigation, it turned out that part of the site's pages had been tampered with.
> Identify the **name of the GitHub account publishing the source code** of the library being abused as the distribution source.
> For example, if the account name is example, the flag should be `Diver26{example}`
>
> Target site: [https://shop.attic-findings.com/](https://shop.attic-findings.com/)

### 解法

Webの調査ということでCodexに依頼。ページのソースコードでは、正規のCDNに似せた別のドメイン `js-deliver.com` からスクリプトが読み込まれており、これが改ざんと考えられる。

読み込まれていたのが `polyfill.min.js` であり、このライブラリには実在のセキュリティインシデントがあったことから、実物のpolyfillライブラリを公開しているアカウント "polyfillpolyfill" を回答したが、不正解だった。

https://gigazine.net/gsc_news/en/20240626-polyfill-supply-chain-attack/#gsc.tab=0

CTFの課題なので実在のセキュリティインシデントではなく架空の問題設定だろうと考え直し、 `js-deliver.com` を直接開いたところ、偽のCDNの説明ページからGitHubアカウント "smpri194-beep" に飛ぶことができた。

![js-deliver top](/images/diver-osint-ctf-2026-kn1cht/js-deliver.png)

:::details flag (Click to expand)
```
Diver26{smpri194-beep}
```
:::

## shopping2 (275pt / 170 solves)

### 課題

> shopping1 で特定した攻撃者は別のキャンペーンも並行運用しているらしい。並行キャンペーンサイトのFQDNを特定せよ。
> 例えば、FQDNがcampaign.example.comの場合、Flagは `Diver26{campaign.example.com}` となる。
>
> The attacker identified in shopping1 appears to be running another campaign in parallel. Identify the FQDN of the parallel campaign site.
> For example, if the FQDN is campaign.example.com, the flag should be `Diver26{campaign.example.com}`.

### 解法

"smpri194-beep" では他のアカウントが発見できないので、 "smpri194" でGoogle検索するとCloudflare Workersで公開されたサイト `plain-cherry-ca06.smpri194.workers.dev` を発見した。内容的には core.shim.js という別のライブラリのCDNを偽装するもの。

:::details flag (Click to expand)
```
Diver26{plain-cherry-ca06.smpri194.workers.dev}
```
:::

# transportation

## 250 (461pt / 72 solves)

### 課題

> SNS投稿 / Social Media Post: [https://www.facebook.com/reel/1181612624131449](https://www.facebook.com/reel/1181612624131449)
>
> 2026年7月4日18:00（現地時間）ごろ、このバスはどこにいたか。地図上で示せ。
>
> Where was this bus around 18:00 on July 4th, 2026 (local time)? Indicate its location on the map.

### 解法

投稿はアメリカ合衆国建国250周年記念（America 250）でBroward County Transitという企業のバスを特別ラッピングにしたというもの。動画や企業のプレスリリースから、車番20094が読み取れる。

https://webapps.broward.org/NewsRelease/View.aspx?intMessageID=16236

ここからよくわからなかったのでCodexに依頼したところ、BCT社はGeneral Transit Feed Specification (GTFS)形式で車両位置を公開していることが判明。GTFS-Realtimeの履歴が [gtfsrt.io](https://gtfsrt.io/) に日付別で公開されており、2026-07-04 分を対象に、`vehicle_id = 20094` で絞り込むことで位置を調査できた。

現地の夏時間（EDT, UTC−4）に合わせて時刻補正すると、18:00頃のバスの位置情報は以下のように求まった。

| Time | Latitude | Longitude | Route |
|---|---:|---:|---|
| 17:59:58 EDT | 26.145458 | -80.211624 | BCT36 “SUNRISE - A1A via LAUDERHILL TRANSIT CENTER” |
| 18:00:18 EDT | 26.145428 | -80.210121 | BCT36 “SUNRISE - A1A via LAUDERHILL TRANSIT CENTER” |

17:59:58がほぼ求められている時刻に近いので、1行目の座標付近にピンを立てて提出。

:::details flag (Click to expand)
```
26.145458, -80.211624
```
:::

## air2air2 (445pt / 85 solves)

### 課題

> 動画 / Video: [https://www.youtube.com/watch?v=Mm3CgN6mbVU](https://www.youtube.com/watch?v=Mm3CgN6mbVU)
>
> この動画は現地時間の2026年1月9日に撮影された。
> この動画に映っている航空機の機体記号（登録番号）と、コールサインは何か。アンダースコア (`_`) で繋いで解答せよ。
> たとえば、機体記号が F-WWJJ で、コールサインが AIB789 のとき、Flagは `Diver26{F-WWJJ_AIB789}` となる。
> **注意**:撮影者が搭乗している機体ではない。また、コールサインはICAOコードを使用すること。
>
> This video was filmed on January 9, 2026, in local time.
> Answer with the aircraft registration number and call sign shown in this video, connected with an underscore (`_`).
> For example, if the registration number is F-WWJJ and the call sign is AIB789, the flag should be `Diver26{F-WWJJ_AIB789}`.
> **Note**: This does not refer to the aircraft the person filming is on board. Also, use the ICAO code for the callsign.

### 解法

映像では撮影者の機体が着陸直前であり、右の窓から別の旅客機が見えていた。別の旅客機の外観と機体形状から、Air Canadaが運用するボーイング777-300ERと推定される。

地上の雰囲気から、撮影場所は成田国際空港と仮定し、ADS-B Exchangeにて2026年1月9日の成田到着便を調査することにした。映像の空は夕方ごろと思われるため、この日の日没である16時44分前後に到着したAir Canada便（ACA）を候補とした。

その結果、ACA3とACA9が候補となった。そのうちACA9をよく見るとAir JapanのAJX122便と並んで着陸しており、動画の着陸順序とも矛盾しない。機体記号とコールサインをつなげるとFlag。

https://globe.adsbexchange.com/?replay=2026-01-09-06:55&icao=86ceb0&lat=35.747&lon=140.392&zoom=12.9

![AJX122 and ACA9](/images/diver-osint-ctf-2026-kn1cht/aca9.png)

:::details flag (Click to expand)
```
Diver26{C-FIVW_ACA9}
```
:::

## telelens (337pt / 145 solves)

### 課題

> 映像 / Video: [https://youtu.be/3O09qFWkjBs](https://youtu.be/3O09qFWkjBs)
>
> この動画が撮影された日はいつか。現地時間のYYYY-MM-DDで答えよ。 例えば、現地時間で2026年7月1日に撮影された場合、Flagは `Diver26{2026-07-01}` となる。
>
> Answer the date this video was filmed in YYYY-MM-DD format based on local time. For example, if it was filmed on July 1, 2026 (local time), the flag should be `Diver26{2026-07-01}`.

### 解法

映像に、パプアニューギニア独立50周年を記念した特別塗装が施された、ニューギニア航空の機体（機体記号：P2-PGA）が写っている。また、周囲の建物から撮影場所は羽田空港。

![当日の写真](/images/diver-osint-ctf-2026-kn1cht/P2-PGA.jpg)
*CTF課題の動画から引用*

P2-PGAは2026年2月9日から11日の予定で羽田に飛来しており、航空ファンが多数写真を投稿している。

https://www.aviationwire.jp/archives/337508

映像はかなりの雨天である。各日の写真とSNS投稿を追うと、2月11日は雨の中羽田空港に滞在し、翌12日に離陸したとのことだった。FlyTeamの投稿写真でも天候が一致するため、2月11日と断定。

https://flyteam.jp/photo/4378999


:::details flag (Click to expand)
```
Diver26{2026-02-11}
```
:::

## conflicted (311pt / 156 solves)

### 課題

> 日本国内において、ソ連で設計された小型航空機が地上構造物との接触による事故を起こしたことが記録されている。
> この **構造物の位置** を地図上で示せ。
>
> **墜落場所ではなく、構造物の場所を答えよ。**
>
> There are recorded incidents in Japan involving small aircraft of Soviet design colliding with ground structures.
> Indicate **the location of this structure** on a map.
>
> **Not a crash site. Indicate the location of the structure.**

### 解法

Geminiの調べで、2003年のツインリンクもてぎ航空ショーでSu-26が事故を起こした件と推定。事故の瞬間の動画も残っている。

@[youtube](fqUBfcQs7Y8)

運輸安全委員会の事故調査報告書の後半（データ上15ページ）に地上の構造に重ねた飛行経路図があるため、衝突位置として示された地点を回答すればよい。

https://jtsb.mlit.go.jp/jtsb/aircraft/detail2.php?id=1776

:::details flag (Click to expand)
```
36.5316167131, 140.2297613260
```
:::

## dx (162pt / 208 solves)

### 課題

> 2026年2～3月、グリッド `31FEV` の中に存在する陸地でイベントが実施されたようだ。
> このイベントに関して利用された船舶が、最後に停泊していた港を地図で示せ。
> もし、いま現在も船舶がどこかの港に停泊していることが確認できる場合、その港を示せ。
>
> It appears that an event took place on land within grid `31FEV` between February and March 2026.
> Indicate on the map the last port where the vessel used for this event stayed. If the vessel can currently be confirmed to be staying at a port, indicate that port.

### 解法

Geminiによると次の通り（裏付けの調査はしたが、間違っていなさそう）。

> グリッド 31FEV（MGRS座標系）が指す陸地は、南大西洋に位置する世界で最も孤立した無人島 ブーベ島（Bouvet Island）です。
>
> 2026年2月〜3月にかけて、この島で大規模なアマチュア無線DXペディションイベント（3Y0K）が実施されました。
>
> 利用された船舶: Icetugs社が運用する砕氷船 Argus
>
> 現在停泊が確認できる港: Reykjavik Harbour（アイスランド・レイキャヴィーク港）
> 遠征終了後、本船は北半球へ回航され、現在はホームポートであるアイスランドのレイキャヴィーク港に停泊しています。

3Y0K Bouvet Island DXpeditionはアマチュア無線のイベントで、運営者が砕氷船Argus（IMO番号：7104752）でブーベ島に向かい世界の愛好家と通信するというもの。AISデータから現在レイキャヴィーク港に停泊しているので、その位置を回答した。

https://www.marinetraffic.com/en/ais/details/ships/shipid:156518/mmsi:219226000/imo:7104752/vessel:ARGUS

:::details flag (Click to expand)
```
64.1546431358, -21.9404911995
```
:::

# 感想
すべてを解き切ることができず残念ではあるが、LLMも活用しつつ難問も含めて正答できた。

今回特に実感したのがOpenAI CodexのようなAIエージェントの進化であった。これまでは画像やPDFファイル、衛星写真などを目視チェックすることの多いOSINT CTFは人力の領域を多く残すジャンルだと感じていた。しかし、今回CodexはYoutube映像をブラウザで閲覧してスクリーンショットする、Google Mapsで地図を少しずつ動かしながら地物のチェックをする、公式のPDFファイルを発見・取得してFlagまでたどり着くなど、人力作業と同等のことを（時間さえかければ）実行できていた。

一部のヒントが少ない難問ではFlagに至れないこともあるが、時間と費用さえかけられるなら、衛星写真を長時間閲覧し続ける等の根性論的な調査方法をAIエージェントも取ることができると考えられる。OSINT CTFやOSINTそのもののワークフローが、AIによって強化される傾向が強まっていると感じた。

