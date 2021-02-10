---
title: "Apple Watchの心電図データをPythonで分析して遊ぶ"
emoji: "💓"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - python
  - applewatch
  - watchos
  - ECG
  - 生体情報
published: true
---

# 概要

日本国内でも、Apple Watchの心電図測定機能が解放されました。測定したデータをCSVで書き出し、Pythonの生体情報ライブラリであるBioSPPyによって分析してみました。
**心電図の波形を見たり、ピーク検出を使ってRRI・RRVといった心電指標を計算したり**できるものの、LF/HFなどの周波数領域資料の算出は難しいようです。

サンプルコードをGoogle Colabで公開しています。ご自身がアップロードしたCSVデータで心電図の解析をお試しいただけます！

- [**applewatch_ecg_python - Colaboratory**](https://colab.research.google.com/drive/1oHeiMhPXrACYoTsdpWe76nxA2jY_pPDW?usp=sharing)

# Apple Watchによる心電図（ECG）測定

## 背景
2020年1月27日、日本国内で販売されたApple Watchを対象に、[**心電図アプリによる心電図（electrocardiogram; ECG）測定**を可能にするアップデート](https://www.apple.com/jp/newsroom/2021/01/ecg-app-and-irregular-rhythm-notification-coming-to-apple-watch/)が行われました。iOS 14.4とwatchOS 7.3にアップデートすれば、すでに購入済みのApple Watch Series 4、5、6でも自分のECGを見られます。

https://support.apple.com/ja-jp/HT208955

そもそもApple WatchによるECG測定は、2018年発売のSeries 4から搭載され、米国などでは利用できました。しかし、[日本での展開に必要な医療機器認証をすぐには取得できず、機能が長期間封印されていました](https://xtech.nikkei.com/atcl/nxt/column/18/00096/00012/)。わざと海外でApple Watchを購入するなどの手段を取れた人を除けば、多くの人にとって、待ちに待ったECG測定解禁となります。

## ECGデータを解析して遊びたい
Apple Watchで測定したECGは、心臓に異常を感じている方にとっては間違いなく重要なデータでしょう。しかし、特に異常を感じておらず、心電図を読めるだけの知識もない多数のユーザからすると「**測定してなんとなく波形を眺めて終了**」になりかねません。せっかく日常的にECGを測定できるセンサを初めて手にしているのですから、取得したデータであれこれ遊びたいものです。

@[tweet](https://twitter.com/h_okumura/status/1354290945512988673)

ECG機能の国内リリース直後に、三重大学の奥村先生が**ECGデータをPythonに読み込んでプロット**した様子をツイートされていました。約512 Hzで30秒間の測定データを、数値データとして得られるようです。本記事では、この数値データを使って、Pythonライブラリ`BioSPPy`による簡単なECGデータ分析を行ってみます。

# ECGの測定とデータの取り出し

ECG測定が初めてなら、[Apple公式の案内](https://support.apple.com/ja-jp/HT208955)を読みながら「心電図App」を設定しましょう。アプリをWatchで開き、Digital Crownと呼ばれるボタンに触れると測定が始まります。測定自体は30秒で簡単に終わるものの、**きれいな波形にするためには工夫が必要**です。

![](https://storage.googleapis.com/zenn-user-upload/tn7xknd6fhjgtassi8hciyka6q3a =300x)

![](https://storage.googleapis.com/zenn-user-upload/rxmicswpoukw0bj57nnn4rcm7npt =300x)

上の画像は**腕をしっかり固定して測定**した場合、下の画像は測定中に腕を動かした場合です。腕を動かしてしまうと、波形が上下にブレたり、細かいノイズが乗ってしまったりすることが分かります。

Apple WatchでのECG測定では、**Watchの背面から体内を通ってDigital Crownに触れた指までを電気回路として扱う**ことで電圧を測定します。測定する電圧はとても小さいため、少しでも腕などが動くと波形が大きくブレてしまうのです。机の上に腕を置くなど、30秒間しっかりと上半身を固定できる状況で測定しましょう。

:::message
ある程度動きながらでもECGを測定したいのであれば、やはり胸に電極を貼り付けるタイプの心電センサが有利です。**AD8232**というセンサを使った数千円の製品が、電子部品販売サイト等で売られています（[AD8232搭載単極誘導心電モニター](https://www.switch-science.com/catalog/1969/)、[M5Stack用ECGモジュール](https://www.switch-science.com/catalog/6782/)）。

なお、指先や耳たぶ等で測定するタイプの製品は「**脈波（光電容積脈波）センサ**」といい、ECGセンサとは測定原理が異なります。測定できる波形もECGとは全く違うので、間違わないようにしましょう。

:::

測定を終えたら、iPhoneの「ヘルスケア」アプリに移ってデータを取り出す作業をします。[奥村先生のWebサイトでも解説](https://oku.edu.mie-u.ac.jp/~okumura/python/applewatch_ecg.html)されているとおり、「**すべてのヘルスケアデータを書き出す**」機能でZipファイルを生成します。
この機能はECGに限らずあらゆるヘルスケアデータを書き出すので、過去に蓄積したデータが多いほど時間がかかります。気長に待ちましょう。

![](https://storage.googleapis.com/zenn-user-upload/ju277vnthi1pxl1bd44kzwrci23y  =300x)

Zipファイルができたら、iOSの共有メニューが開きます。Macをお使いならAirDrop、そうでない場合はiCkoud Drive・Google Drive・Dropboxなどのクラウドストレージに保存するのが楽です。

:::message
**Zipファイルの書き出しは手動かつ時間がかかる**ため、日常的にECGデータを取り出して分析したい人には向きません。[開発者向けのHealthKitでECGデータを取得できる](https://stackoverflow.com/questions/63291989/how-to-get-ecg-voltage-measurements-hkelectrocardiogram-voltagemeasurement-ios-1)ようなので、定期的にECGを出力したい場合には独自にアプリを作ってしまうのがいいでしょう。

:::

ECGデータは、`electrocardiograms`フォルダ内にCSVファイルとして格納されています。これをお好みのフォルダに展開すれば、分析の準備は完了です。

![](https://storage.googleapis.com/zenn-user-upload/309t9g6dji95k5aex9542rk79754)

# プロットしてみる

※本節以降で使用したコードを[Google Colabで公開しています](https://colab.research.google.com/drive/1oHeiMhPXrACYoTsdpWe76nxA2jY_pPDW?usp=sharing)。

まずは、[奥村先生のコード]((https://oku.edu.mie-u.ac.jp/~okumura/python/applewatch_ecg.html))を参考にmatplotlibで値をプロットしてみます。CSVの先頭に記載されているように、**サンプリング周波数は512 Hz、単位はµV**です。

```py
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
plt.rcParams['font.family'] = 'IPAexGothic'

ecg_df = pd.read_csv('ecg_2021-02-06.csv', skiprows=13, header=None)

fig = plt.figure(figsize=(10,2))

plt.plot(ecg_df[0])
plt.xlabel('sample')
plt.xlim(0, 15360)
plt.xticks(np.arange(0, 15360, 512 * 4))
plt.ylabel('ECG [μV]')
plt.ylim(-500, 1000)
plt.grid()

plt.show()
```

しっかり30秒分取れていることが分かります。

![](https://storage.googleapis.com/zenn-user-upload/xg1xuzrkbjax14xalhqhw11d653y)


```py
# （省略）
plt.xlim(512 * 5, 512 * 10)
plt.xticks(np.arange(512 * 5, 512 * 10, 512))
# （省略）

plt.show()
```
少し拡大してみると、よく見る心電図っぽい波形が見えてきました。

![](https://storage.googleapis.com/zenn-user-upload/js9ch193wuuotmzb7sm0imsnp5zg)

ECGの波形には、P波～U波までの名称が付けられています。もっとも高く鋭いピークはR波と呼ばれ、**心臓が血液を送り出すタイミング**を示します。
R波から次のR波までの間隔を **RR間隔（R-R Interval; RRI）** といい、心拍数などの計算に用いられます。

![](https://storage.googleapis.com/zenn-user-upload/eftftektcury37lo50b56qc4ss7p =350x)

:::message
プロットされたECG波形が上下逆になっている場合は、Apple Watchの向きの設定を間違えていることが原因です。データの符号を逆転させれば、正常なECGデータになります。

:::

# BioSPPyを使ってみる

Pythonで生体情報を扱えるライブラリは複数ありますが、本記事では`BioSPPy`[^1]をご紹介します。信号処理やパターン認識などの技術が搭載されており、**光電容積脈波（PPG）・心電（ECG）・皮膚電気活動（EDA）・脳波（EEG）・筋電（EMG）・呼吸をサポート**しています。
ポルトガルの研究機関Instituto de Telecomunicaçõesに所属するPattern and Image Analysis (PIA) Groupによってメンテナンスされており、研究者にとっても安心して使える品質のライブラリです。

https://biosppy.readthedocs.io/en/stable/

<!-- textlint-disable -->
[^1]: Carreiras C, Alves AP, Lourenço A, Canento F, Silva H, Fred A, et al. "BioSPPy - Biosignal Processing in Python", 2015-, https://github.com/PIA-Group/BioSPPy/ [Online].
<!-- textlint-enable -->

## ECGデータの読み取り
ライブラリをpipからインストールします。

```bash
$ pip install biosppy
```

ecgモジュールを使ってECG信号を処理します。showオプションをTrueにすると、データの概要がまとめてプロットされます。`ecg.ecg()`では、**データの周波数フィルタリング・R波ピークの検出・心拍数の算出**が行われています。

```py
from biosppy.signals import ecg

ecg_data = ecg.ecg(signal=ecg_df[0], sampling_rate=512., show=True)
```

概要プロットは、左列が上から**生の波形・フィルタリングされた波形と検出したR波・心拍数の変動**、右列が**ECGの1波形**を示しています。

![](https://storage.googleapis.com/zenn-user-upload/73ihav3broean7wgrczyddbqsypl)

## 周波数フィルタ
BioSPPyのフィルタリングによる効果を確かめるために、生波形とフィルタリングされた波形を見比べてみましょう。

```py
(ts, filtered, rpeaks, templates_ts, templates, heart_rate_ts, heart_rate) = ecg_data

fig = plt.figure(figsize=(10, 4))
plt.subplots_adjust(wspace=0.4, hspace=0.7)

for (i, data, title) in [(1, ecg_df[0], '生データ'), (2, filtered, 'フィルタリング')]:
    ax = fig.add_subplot(2, 1, i)
    ax.plot(ts, data)
    ax.set_title(title, fontsize=10)
    ax.set_xlim(0, 30)
    ax.set_ylim(-500, 1000)
    ax.set_xlabel('time [s]')
    ax.set_ylabel('ECG [μV]')
    ax.grid()

plt.show()
```

画像はなるべく腕を固定して測定したECG波形です。生波形では0付近の波形がブレているのに対して、**フィルタリング後は波以外の箇所がしっかり0に揃っている**のが分かります。ソースコードを見ると、3 Hzから45 Hzまでのバンドパスフィルタを掛けているようです。

![](https://storage.googleapis.com/zenn-user-upload/dkayie7vuwopc70vurjzrgy2gda0)

周波数フィルタリングは、**体動によって波形がブレてしまったECGデータ**に対して威力を発揮します。画像はわざと腕を動かして測定したデータで、生波形は大きく動揺しています。フィルタ処理後も細かいノイズは残っているものの、R波を見やすい波形に変換できました。

![](https://storage.googleapis.com/zenn-user-upload/jvtc1l6ena0kx9yhze6zya6ywsgz)

## R波のピーク検出

BioSPPyのECGモジュールには、**5種類のR波検出アルゴリズム**が実装されています。それぞれの論文は、一部を除いて[公式ドキュメントに引用されています](https://biosppy.readthedocs.io/en/stable/biosppy.signals.html#biosppy-signals-ecg)。
標準の`ecg.ecg()`で呼び出されているのは`hamilton_segmenter()`です。

- `christov_segmenter()` : (Christov, 2004)
- `engzee_segmenter()` : (Engelse and Zeelenberg, 1979; Lourenco et al., 2012)
- `gamboa_segmenter()` : (Gamboa, 2008)
- `hamilton_segmenter()` : (Hamilton, 2002)
- `ssf_segmenter()` : (Zong et al., 2003)

あえてピーク検出を難しくするため、**わざと腕を動かした時のECGデータ**に対して、それぞれのアルゴリズムを適用してみます。

```python
# 体動入りECGデータ
(ts, filtered, _, _, _, _, _,) = ecg.ecg(signal=ecg_df_2[0], sampling_rate=512., show=False)

rpeaks_christov, = ecg.christov_segmenter(filtered, 512.)
rpeaks_engzee, = ecg.engzee_segmenter(filtered, 512.)
rpeaks_gamboa, = ecg.gamboa_segmenter(filtered, 512.)
rpeaks_hamilton, = ecg.hamilton_segmenter(filtered, 512.)
rpeaks_ssf, = ecg.ssf_segmenter(filtered, 512.)
rpeaks_ssf_2000, = ecg.ssf_segmenter(filtered, 512., threshold=2000.)
```

検出されたピークを散布図にプロットしてみました。**Christov、Engzee、Hamiltonのアルゴリズムはすべて同じピークを検出**できています。

一方で、GamboaのアルゴリズムとSSFアルゴリズムは、R波以外のピークやノイズまで拾ってしまったようです。SSFの方は、オプションのthreshold値を100倍にすることでかなり改善しました（それでも、29秒付近でピークを2重に検出してしまっています）。

![](https://storage.googleapis.com/zenn-user-upload/xrvcrzk96p73a837y3tz1r6xaovj)

BioSPPyを開発しているPIA Groupによるレビュー論文[^2]によれば、公開データベースのECGについて**ChristovのアルゴリズムとSSFアルゴリズムがもっとも高い性能を発揮した**ようです。閾値を決める必要がないので、Christovアルゴリズムを使うのが楽そうです。

[^2]: Canento, Filipe, et al. "Review and comparison of real time electrocardiogram segmentation algorithms for biometric applications." Proc. 6th Int. Conf. Health Inform. (HEALTHINF). 2013.

# RRIと心電指標の計算

BioSPPyで可能な分析は、R波の検出までです。ECGから得られる指標は他にもある[^3]ので、代表的なものを算出してみましょう。

## R-R間隔（RRI）

R波から次のR波までの間隔です。単位はミリ秒（ms）を使うことが多いようです。

検出した**R波ピーク同士の差分**を取ることで簡単に求まります。

```py
ts_peaks = ts[rpeaks_christov]
rri = np.diff(ts_peaks) * 1000
# [929.62697347,  912.04999288,  925.72097778,  ... ]
```

ただ差分を計算しただけでは、R**RIの値同士が等間隔になっていない**ので、周波数領域での処理ができません。そこで、スプライン補間などによって等間隔（例えば1秒間隔）のデータに再サンプリングする処理をよく行います[^3]。

<!-- textlint-disable -->
[^3]: 藤原 幸一, ヘルスモニタリングのための心拍変動解析, システム／制御／情報, 2017, 61 巻, 9 号, p. 381-386. https://doi.org/10.11509/isciesci.61.9_381
<!-- textlint-enable-->

```py
import scipy

spline_func = scipy.interpolate.interp1d(ts_peaks[:-1], rri, kind='cubic')
ts_1sec = np.arange(ts_peaks[0], ts_peaks[-2], 1)
rri_1sec = spline_func(ts_1sec).round(6)
```

スプライン補間した曲線からは、RRIが**数秒の周期で変化**していることが見て取れます。

![](https://storage.googleapis.com/zenn-user-upload/wq5s8ecr6zk3jnricheilvwquakj)

## 心拍変動（R-R interval variability; RRV）

RRIの分散で、**心拍がどの程度変動しているか**を示します。RRVの指標として、[SDNN・RMSSD・NN50などが提案されています](https://www.trytech.co.jp/checkmyheart/glossary.html)。

ここでは、RRIの標準偏差であるSDNNを計算してみます。

```py
np.std(rri_1sec)
# 26.51 ms
```

Apple Watchには、定期的に測定した心拍データから心拍変動を推定する機能があります（RRIから求めたものではないため、厳密にはRRVではない）。心電図データを取得したのとなるべく近い時刻の心拍変動は33 msと記録されており、今回RRIから求めた26.51 msと大きくは違わない値でした。

![](https://storage.googleapis.com/zenn-user-upload/sz4bw93t3lo3ttp4f1him32n210i =250x)

## 周波数領域指標

RRIをフーリエ変換によってパワースペクトル密度（power spectral density; PSD）に変換すると、**周波数領域での指標**を計算できます。[例えば交感神経活動と副交感神経活動のバランスを表すLF/HF](http://hclab.sakura.ne.jp/stress_nervous_oth_stress_idx.html)などがあります。

ただ、Apple Watchのデータでは測定時間が短すぎたのか、**LF/HFを正しく算出できませんでした**（低周波であるLFが全く出てきませんでした）。
心電測定のガイドラインでは、LF/HFの算出には最低限2分間が必要で、他の研究との比較のために**5分間のデータを使うのが望ましい**とされています[^4]。
Apple WatchのECG測定時間は30秒なので、この基準に遠く及びません。10回ほど連続で測定すれば時間は確保可能ですが、その間Apple Watchを何度も操作しながら、なるべく動かずに測定を続けるのは困難でしょう。

<!-- textlint-disable -->
[^4]: Task Force of the European Society Electrophysiology, "Heart Rate Variability: Standards of Measurement, Physiological Interpretation, and Clinical Use," Circulation, vol. 93, no. 5, pp. 1043–1065, Mar. 1996. https://www.ahajournals.org/doi/10.1161/01.CIR.93.5.1043
<!-- textlint-enable-->

# おわりに

Apple Watchで測定したECGデータをPythonに読み込み、心電波形の確認や心電指標の算出をしてみました。一度自分のECGデータをあれこれ分析してみると、iPhoneでなんとなく波形を眺めるだけよりもさらに測定できている実感が湧くのではないでしょうか。
