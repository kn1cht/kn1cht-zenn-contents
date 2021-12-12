---
title: "M5AtomからI2Sでステレオ音を出力する（UDA1334A）"
emoji: "💓"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - IoT
  - m5atom
  - m5stack
  - i2s
published: false
---

:::message
本記事は、[M5Stack Advent Calendar 2021](https://qiita.com/advent-calendar/2021/m5stack) 12日目の記事です。
:::

# TL;DR
- M5AtomはI<sup>2</sup>Sにより音を出力できるものの、公式のキットに搭載されている**NS4168はモノラルアンプ**なのでステレオ音源に対応できません
- ステレオ対応DACであるUDA1334Aのモジュールを使うと、M5Atomからステレオ出力できることを確認しました
- 確認用のコードを、GitHubに公開しています

<a href="https://github.com/kn1cht/m5atom-stereoi2s-uda1334a"><img src="https://github-link-card.s3.ap-northeast-1.amazonaws.com/kn1cht/m5atom-stereoi2s-uda1334a.png" width="460px"></a>

# M5Atomと音声出力
M5Atomは、M5シリーズの中でも**最小のサイズと、サイズにしては多めのGPIOピン**を特徴とする開発モジュールです。
[スイッチサイエンスさんでatomを検索すれば分かる](https://www.switch-science.com/catalog/list/?keyword=atom)とおり、挿すだけで使える拡張モジュールがたくさん発売されています。

M5Atom向けには、音を扱えるモジュールとしてATOM EchoとATOM SPKが出ています。

- [ATOM Echo - スマートスピーカー開発キット - スイッチサイエンス](https://www.switch-science.com/catalog/6347/)
- [ATOM NS4168搭載 スピーカーキット - スイッチサイエンス](https://www.switch-science.com/catalog/7092/)

## M5Atom標準のNS4168はモノラルのみ
これらモジュールはNS4168というアンプチップを搭載しています。I<sup>2</sup>S（Inter-IC Sound）インタフェースを通してM5Atomから音信号を送り込み、音楽を流したり、合成音声を喋らせたりできるというわけです。

I<sup>2</sup>Sは、3本の信号線によって**左チャンネル・右チャンネルのステレオ出力**ができる規格です。
ところが、NS4168はモノラル出力しかできません。データシートの回路図を見れば明らかなように、そもそも出力ピンがスピーカ1個分しかありません。
ステレオ信号が入力されても、左か右のどちらかしか再生できないのです。

![NS4168の回路図](https://storage.googleapis.com/zenn-user-upload/e772a35cd8b2-20211212.png)
*NS4168データシートより*

## M5Atomでもステレオ出力したい！
ATOM EchoやATOM SPKの想定用途は自作スマートスピーカーやオーディオプレイヤーのようです。スピーカーから喋らせたりちょっと音楽を鳴らしたりするだけならモノラルでもよいかもしれません。ただ、I<sup>2</sup>Sはせっかくステレオに対応できるのですから、やはり**ステレオで音楽を聴いてみたい**ものです。

もう1つの理由は限られた人にしか当てはまりませんが、**振動触覚アクチュエータ（振動子）を使う場合**にもステレオ出力ができると便利です。この秋、[触感デバイス開発モジュール“hapStak“](https://www.switch-science.com/catalog/7461/)が発売され、M5AtomからI2Sで振動を制御できるようになりました。

![hapStak](https://storage.googleapis.com/zenn-user-upload/c671606828c5-20211212.png)
*hapStak*

このモジュールもNS4168を使っているので、1個のM5Atomに対して1個の振動子しか使えません。もしM5Atomからステレオ出力ができれば、1個のAtomで2つの振動子に別々の振動を送る、スピーカから音を出しながら振動子も動かすなど、使い方が広がります。

# 検証環境
## ハードウェア
- [M5Atom Matrix](https://www.switch-science.com/catalog/6260/)
- [**UDA1334A搭載 I2S ステレオDACモジュール**](https://www.switch-science.com/catalog/3572/)
- ジャンパケーブル5本、ブレッドボード
- 有線のイヤホンかヘッドホン

![使った部品たち](https://storage.googleapis.com/zenn-user-upload/9e479bbbba1c-20211212.jpg)

## ソフトウェア
- **Visual Studio Code & PlatformIO**
    - Arduino IDEでも本記事と同様の操作は可能です。適宜読み替えてください
    - ただ、[M5StackはArduino IDEを使うと書き込みがとても遅く、平気で数分かかります](https://qiita.com/tatsuya1970/items/ceeb5a5b011d53910fbb)。PlatformIOを導入して慣れる手間を惜しまないほうが、生産性が上がって幸せになれると考えます

# UDA1334Aモジュールを使ってみる
「I2S ステレオ」でググると真っ先に出てくるのが、Adafruitの[**UDA1334A搭載 I2S ステレオDACモジュール**](https://www.switch-science.com/catalog/3572/)です。3.5mmステレオミニジャックと音声出力ピンを備え、I<sup>2</sup>S以外のフォーマットにも対応するそうです（今回は普通にI<sup>2</sup>Sで使います）。

- [Pinouts | Adafruit I2S Stereo Decoder - UDA1334A | Adafruit Learning System](https://learn.adafruit.com/adafruit-i2s-stereo-decoder-uda1334a/pinouts)

ピン配置についてはAdafruitのドキュメントが分かりやすいのでご覧ください。たくさんピンが付いてますが、I<sup>2</sup>Sで使う分には**電源に2本・I<sup>2</sup>Sに3本**の5本だけ繋げば動きます。

![UDA1334Aモジュールのピン配置](https://storage.googleapis.com/zenn-user-upload/47ccf77f0c82-20211212.jpg)
*左からVIN（電源） / GND（グラウンド） / WSEL（左右ch選択用） / DIN（音声信号用） / BCLK（ビットクロック）*

**I<sup>2</sup>Sのピン名称にはいくつかの表記がある**ので、役割を覚えておくのがいいです。例えば、WSELピンは左右の信号のどちらを送るか選択する用途ですが、英語でWord Selectとも、Left Right Clockとも呼ばれます。

　M5Atom側の表記 | モジュール側の表記
----------------|------------------
 LRCK | WSEL
 Data | DIN
 BCLK | BCLK

![](https://storage.googleapis.com/zenn-user-upload/bc1ba0cefb92-20211212.jpg)
*接続した様子（GPIOは22, 19, 33を使用）*

# 罠になりやすいポイント
M5AtomとUDA1334Aの組み合わせは、基本的には「繋いでサンプルを書き込めば動く」です。
ただ、ミスしやすいポイントを踏むと音が出なくて悩むことになります。

https://twitter.com/kn1cht/status/1469585957154603008

*↑この後２回ほどハマりポイントを踏んだ。慢心には気をつけよう！*

## ATOM EchoとATOM SPKで、I<sup>2</sup>Sに使うピン番号が異なる
ATOM EchoとATOM SPKは、共にNS4168を使ったモジュールです。しかし、**使っているGPIOピンが異なるため互換性がありません**。

https://twitter.com/mongonta555/status/1370765861426851844?conversation=none


   -  | BCLK | LRCK | Data
------|------|------|-------
 ECHO | G19  | G33  | G22
 SPK  | G22  | G21  | G25

今回拡張モジュールそのものは使わないものの、使うサンプルコードが想定するピン番号と異なるピン番号にケーブルを挿していると、無音になったり謎の雑音が再生されたりとうまく動きません。
**使うサンプルコードのピン番号指定を必ず確認**し、コードを書き換えるか、ケーブル位置を変えるかのどちらかで対応しましょう。

## ケーブルのちょっとした挿し具合でノイズが入ったりする
これは筆者が用いたジャンパケーブルの品質の問題かもしれませんが、ブレッドボードに挿した**ケーブルの角度が少し変わるだけで、音が消えてサーというノイズだけになる・無音になる**などの現象が起きました。

サンプルで音が出なくても、指でケーブルの根本を掴んだら正常な音になることもあります。同様の現象が起きた場合はお試しください。
また、お試しレベルではなくもしこのセットで常用されるのであれば、もう少ししっかりケーブルを固定するなどの措置を取るのがいいでしょう。

# ATOM SPKのサンプルを動かす
まずは、ATOM SPKのサンプル`PlayRawPCM`を書き込んでみましょう（使うGPIOピンは上の写真から22, 21, 25に変わります）。
読者の方は後述するESP8266Audioのサンプルをいきなり試してもOKですが、SPKのサンプルはコードの書き換えをほぼせずに「**書き込めば動く**」ものですので動作確認にお役立てください。

https://github.com/m5stack/M5Atom/tree/master/examples/ATOM_BASE/ATOM_SPK/PlayRawPCM

PlatformIOを使う場合は、`.pio/libdeps/{環境名}/M5Atom/examples/TOM_BASE/ATOM_SPK/PlayRawPCM`と辿ればファイルが入っているはずです。お試しなので、サンプルのファイル4つを`src/`フォルダにコピーして`PlayRawPCM.ino`を開き、ビルド・書き込みしてみます。

![PlayRawPCMの書き込み](https://storage.googleapis.com/zenn-user-upload/90af341e5c54-20211212.png)
*chocobo_loop_r.c（直球）*

ただしく接続できていれば、M5Atomのボタンを1回押すとサンプル音楽がエンドレスで流れるはずです。音が出た証拠として、PCのライン入力にオーディオケーブルを繋いだ様子を載せておきます。

![PlayRawPCMの動作](https://storage.googleapis.com/zenn-user-upload/c954cf4a58ae-20211212.jpg)

# ステレオ化する

## ESP8266Audioのサンプルを動かす
まず、ATOM SPKのサンプルを拡張してステレオにできないか検討しました。
しかしながら、[AtomSPL.cpp](https://github.com/m5stack/M5Atom/blob/master/examples/ATOM_BASE/ATOM_SPK/PlayRawPCM/AtomSPK.cpp)を見れば分かるように、このサンプルはESP-IDFのI<sup>2</sup>S機能を直接叩いています。これを改造していってもステレオ対応できるのかもしれませんが、もう少しライブラリに頼った開発をした方が楽だと感じました。

そこで、ESPシリーズのSoCのためのオーディオライブラリ**ESP8266Audio**を使ってみましょう。ESP8266Audioにはたくさんのサンプルコードがあり、どれが簡単なのか筆者はよく分かっていません。ひとまず、`PlayMP3FromSPIFFS`を使ってみることにしました。

https://github.com/earlephilhower/ESP8266Audio/blob/master/examples/PlayMP3FromSPIFFS/PlayMP3FromSPIFFS.ino

### コードの修正
先ほどのATOM SPKサンプルとは違い、コードに数か所修正が必要です。

1. Arduino.h -> M5Atom.h

```diff
@@ -1,4 +1,4 @@
-#include <Arduino.h>
+#include <M5Atom.h>
 #ifdef ESP32
   #include <WiFi.h>
   #include "SPIFFS.h"
```

2. AudioOutputI2SNoDAC -> AudioOutputI2S
    - `AudioOutputI2SNoDAC`はDACがない環境でもオーディオ再生ができるためのクラスのようです。今回はDACがあるので、NoDACを使ってしまうと**謎の雑音が再生されてうまくいきません**
    - 下で示しているdiff含め3箇所あるので忘れずに全部変えましょう

```diff
@@ -8,7 +8,7 @@
 #include "AudioFileSourceSPIFFS.h"
 #include "AudioFileSourceID3.h"
 #include "AudioGeneratorMP3.h"
-#include "AudioOutputI2SNoDAC.h"
+#include "AudioOutputI2S.h"

 // To run, set your ESP8266 build to 160MHz, and include a SPIFFS of 512KB or greater.
 // Use the "Tools->ESP8266/ESP32 Sketch Data Upload" menu to write the MP3 to SPIFFS
@@ -18,7 +18,7 @@
 
 AudioGeneratorMP3 *mp3;
 AudioFileSourceSPIFFS *file;
-AudioOutputI2SNoDAC *out;
+AudioOutputI2S *out;
 AudioFileSourceID3 *id3;
 
 
```

3. `out->SetPinout(19, 33, 22);`
    - M5Atomで使っているピン番号を指定します。引数は左からBCLK, LRCK, Data[です](https://github.com/earlephilhower/ESP8266Audio/blob/0d132c1391ae88931c5d3bdd7f33eb2261240393/src/AudioOutputI2S.h#L30)。
    - また、この1行上の`new AudioOutputI2SNoDAC();`のNoDACを忘れずに取りましょう。これらのクラスは暗黙の型変換ができ、もしミスしていてもコンパイルが通ってしまいます（2敗）。

```diff
@@ -56,7 +56,8 @@ void setup()
   file = new AudioFileSourceSPIFFS("/pno-cs.mp3");
   id3 = new AudioFileSourceID3(file);
   id3->RegisterMetadataCB(MDCallback, (void*)"ID3TAG");
-  out = new AudioOutputI2SNoDAC();
+  out = new AudioOutputI2S();
+  out->SetPinout(19, 33, 22);
   mp3 = new AudioGeneratorMP3();
   mp3->begin(id3, out);
 }
```

### SPIFFSにファイル書き込み
先ほどのATOM SPKの例と異なり、`PlayMP3FromSPIFFS`ではオーディオファイル（pno-cs.mp3）をSPIFFSというファイルシステムから参照します。PlatformIOでは、`data/`フォルダをプロジェクト内に作って書き込みたいファイルを設置し、メニューから"Upload Filesystem Image"を実行してしばらく待てばOKです。

https://labo.mycabin.net/electronics/arduino-esp32/1268/

![SPIFFS書き込みボタン](https://storage.googleapis.com/zenn-user-upload/445030296124-20211212.png)
*SPIFFS書き込みボタン*

https://twitter.com/kn1cht/status/1469631204026884097

動きました。結構音質もよくて驚きです。

## PlayMP3FromSPIFFSでステレオになっているか確認する
サンプルの音源は左右で同じ信号になっているので、ちゃんとステレオになっているか確認しましょう。
pno-cs.mp3を編集し、2秒おきに左右の音量バランスが極端に変わるようにして書き込みます。

![pno-cs-pan.mp3](https://storage.googleapis.com/zenn-user-upload/ec1816e208d1-20211212.png)
*pno-cs-pan.mp3*

ファイル名を変える場合は、サンプルコードのファイル名も忘れずに変えておく必要があります。

```diff
@@ -53,7 +53,7 @@ void setup()
   Serial.printf("Sample MP3 playback begins...\n");

   audioLogger = &Serial;
-  file = new AudioFileSourceSPIFFS("/pno-cs.mp3");
+  file = new AudioFileSourceSPIFFS("/pno-cs-pan.mp3");
   id3 = new AudioFileSourceID3(file);
   id3->RegisterMetadataCB(MDCallback, (void*)"ID3TAG");
   out = new AudioOutputI2S();
```

![ステレオ出力の動作](https://storage.googleapis.com/zenn-user-upload/92c6a061f0fb-20211212.png)
*モジュールからの出力をPCで録音した結果*

上のmp3編集画面とあまりにも似た波形ですが、これは**M5AtomとUDA1334Aモジュールから出た音を録音したもの**です。きちんとステレオで再生できています！

https://twitter.com/kn1cht/status/1469645828763820034

動いている様子の動画です。

# IMUでインタラクティブに左右の音量バランスを変えてみる
ステレオ出力にするだけなら、サンプルコードに毛が生えただけでできてしまいました。もう少しM5Atomならではの実験として、Atom Matrixに**内蔵されている慣性センサ（IMU）MPU6886**を使って、本体の傾きで左右の音量を変えられるようにしてみます。

ESP8266Audioのリポジトリを検索した限りは、左右の音量を独立に操作する機能が見つかりません。その代わりに、`AudioOutputMixer`クラスを使えば**複数のファイルを同時に再生し、それぞれの音量も変えられる**ようです。

https://github.com/earlephilhower/ESP8266Audio/blob/master/examples/MixerSample/MixerSample.ino

そこで、左だけ聞こえる音源と右だけ聞こえる音源を用意し、改造した`MixerSample`コードでIMUから取得した傾きに応じて音量を変更するようにしてみました。
コードは長いのでGitHubでご覧ください。

https://github.com/kn1cht/m5atom-stereoi2s-uda1334a/blob/mixer-imu/src/MixerSampleWithM5AtomIMU.ino

https://twitter.com/kn1cht/status/1469682415530811394

いきなり結果ですが、筆者がMixerの使い方を理解していないためか、1秒おきにクリックノイズが混入してしまっています。それでも、**左右の傾きでインタラクティブに音量バランスが変わっている**のが分かります。

## [Tips] ESP8266Audioではloop()に大きなdelay()を入れてはいけない
[READMEに書いてある](https://github.com/earlephilhower/ESP8266Audio/blob/master/README.md#usage)ように、ESP8266Audioは`loop()`関数内で`delay()`を使ってしまうと正常に音が出ません。もし頻度を減らしたい処理があるなら、一定時間おきになるよう条件分岐を使うなどして対処しましょう。

```cpp
  if(millis() % 100 == 0) {
    M5.IMU.getAttitude(&pitch, &roll);
    Serial.println((pitch + 45.0) / 90.0);
    stub[0]->SetGain((pitch + 45.0) / 90.0);
    stub[1]->SetGain((- pitch + 45.0) / 90.0);
  }
```

## [Tips] ループ再生させる
ESP8266Audioのサンプルでは、オーディオファイルの最後まで再生したら停止するようになっています。ループ再生させるためにもう一度`begin()`を呼んでも、最初から再生にはなりません（筆者が試したときは、ループのタイミングでCPUパニックが起きリセットがかかる挙動でした）。

https://github.com/earlephilhower/ESP8266Audio/issues/8

安全にループ再生させたいなら、**その都度ファイルや再生用のオブジェクトを全部作り直す**のがいいようです。筆者は初期化部分を関数に分離し、ループのたびに呼ぶようにしました。

```cpp
void initializeAndStartMP3(const char* filename, int id) {
  Serial.printf("Starting %s\n", filename);
  file[id] = new AudioFileSourceSPIFFS(filename);
  id3[id] = new AudioFileSourceID3(file[id]);
  id3[id]->RegisterMetadataCB(MDCallback, (void*)"ID3TAG");
  stub[id] = mixer->NewInput();
  stub[id]->SetGain(0.5);
  mp3[id] = new AudioGeneratorMP3();
  mp3[id]->begin(id3[id], stub[id]);
}
```

この処理は少し時間がかかってしまい、その間音の再生は止まってしまいます。従って、途切れずにずっとループ再生させることはできていません。

# おわりに

UDA1334AステレオDACモジュールを使い、M5Atomからステレオオーディオ出力をしてみました。細かい罠はあるものの、ピン同士の接続やコード書き込みはそこまで難しい作業なく進められました。

ステレオ出力の使い道は、主には音楽関係になると思います。携帯音楽プレーヤーとか、有線イヤホンをBluetoothで無線化するレシーバーを自作してみるのはいかがでしょうか？
