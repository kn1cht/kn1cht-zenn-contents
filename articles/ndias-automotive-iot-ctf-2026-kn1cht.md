---
title: "NDIAS Automotive/IoT CTF Writeup (OSINT/Keyfob/RF)"
emoji: "🚗"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - Automotive
  - IoT
  - OSINT
  - CTF
  - writeup
published: true
---

NDIAS Automotive/IoT CTF 2026のWriteupです。

# 開催概要・制度
* 2026年5月15日(金)18:00 JSTから5月17日(日)18:00 JST（48時間）
* Jeopardy方式
* 問題数: 37問
* チーム制、チームの人数制限なし
* 点数は各問題の正答数に応じて減少するDynamic方式。

# Writeups
筆者が正答したOSINT, Keyfob, RFカテゴリの問題のWriteupです。

見出しに示した点数は、CTF終了後のものです。
問題サーバーから文章や提供されたデータを引用しています。

:::message
Writeup文章の一部はOpen AI Codexを通じてGPT-5.5が生成しました。
内容の明確さや正確性を筆者がチェックし、編集したうえで公開します。
:::

# OSINT Category

## It’s not a car. (easy, 100pt, 194 solves)

### 課題

>English
>Identify the location shown in the top image at the link below.
>
>https://zoox.com/journal/resorts-world-las-vegas/
>
>Flag format: flag{latitude_longitude}
>Round down to the third decimal place and discard any digits after that.
>Example: flag{35.628_139.744}
>
>日本語
>リンク先のトップ画像の場所はどこか。
>
>https://zoox.com/journal/resorts-world-las-vegas/
>
>フラグフォーマット: flag{緯度_経度}
>小数点第三桁までで、四桁以降は切り捨てて答えること。
>例: flag{35.628_139.744}

### 解法
ロボタクシーZooxがラスベガスの観光地と契約したというプレスリリースが与えられました。

https://zoox.com/journal/resorts-world-las-vegas

トップには何かの建物の入り口の写真があるのでGoogle Lensにてreverse image searchしたところ、ラスベガス・ヒルトンの車寄せである可能性が浮上しました。

https://www.tripadvisor.com/Hotel_Feature-g45963-d23328331-zft19112-Las_Vegas_Hilton_at_Resorts_World.html

ホテルの写真等からデザインが似ていることを確認できます。ただ、ラスベガス・ヒルトンにはよく似た車寄せが3つもあり、写真からはどれなのか確信が持てません。Google StreetViewでも抜けている場所があったため、観光客が撮影した動画などを探して場所を確定させようとしました。

@[youtube](Ao2dlTEAyiY)

最終的に、Zoox車両がラスベガス・ヒルトンに入る流れの動画を発見し、南側の車寄せであることが確定したので座標を確認してFlagを得ました。

:::details flag (Click to expand)
```
flag{36.134_-115.167}
```
:::

なお、CTFルールでflagの大文字・小文字が区別されるという注意を軽く考えており、`FLAG{...}`がincorrectになってしばらく悩んでいました。

## AV Striking (medium, 100pt, 181 solves)
### 課題
> English
>Identify the location where the incident described in the document occurred.
>
>Document: https://static.nhtsa.gov/odi/inv/2026/INOA-PE26001-10005.pdf
>
>Flag format: flag{latitude_longitude}
>Round down to the third decimal place and discard any digits after that.
>Example: flag{35.628_139.744}
>
>日本語
>リンク先のインシデントが発生した場所はどこか。
>
>ドキュメント: https://static.nhtsa.gov/odi/inv/2026/INOA-PE26001-10005.pdf
>
>フラグフォーマット: flag{緯度_経度}
>小数点第三桁までで、四桁以降は切り捨てて答えること。
>例: flag{35.628_139.744}

### 解法
National Highway Traffic Safety Administration (NHTSA)の報告資料が与えられました。

内容は、2026年1月23日にWaymoの自動運転車がサンタモニカの学校の近くで子供にぶつかった（軽傷で済んだ）というものでした。"waymo santa monica child" などでGoogle検索すると、当時の報道がいくつか発見できます。

https://www.telemundo52.com/video/fotosyvideos/vehiculo-autonomo-golpea-nina-santa-monica/2833951/

うち1つはGrant Elementary Schoolという学校名を報道しており、大まかな位置が判明します。

https://maps.app.goo.gl/hZoavcTBaj4T4tn79

報道ではWaymoの当該車両が事故後に住宅の前で停車している写真が使われており、その場所はGoogle StreetViewによってすぐに判明するため、これを答えたもののincorrectでした。

よく考えると、事故後に移動した可能性もあるため「発生した場所」とは限らないわけです。さらに調査が必要です。

これまで得られた情報と、住宅地の中での細かい位置に言及した資料を探すため "grant elementary school waymo house" で検索すると、以下の記事でNational Transportation Safety Board (NTSB)が調査報告書に掲載した事故の詳しい地図が見つかりました。

https://www.surfsantamonica.com/ssm_site/the_lookout/news/News-2026/March-2026/03_11_2026_Details_of_Waymo_Collision_with_Child_Released.html

事故地点がXで示されているので、Google Mapsにて座標が34.0197, -118.4626であることを確認し、flag要件に合わせて加工すれば完了です。

:::details flag  (Click to expand)
```
flag{34.019_-118.462}
```
:::

# Keyfob Category

## Parking Lot Whispter (tutorial, 100pt, 91 solves)
### 課題
>English
> An IQ capture in the 433 MHz band was obtained near a parking area. The capture contains transmissions from multiple wireless devices.
> 
> Identify the Falcon X1 keyfob signal and determine the following:
> 
> - Center frequency
> - Modulation type
> - Symbol period
> The attached file is a raw IQ recording captured under the following conditions:
> 
> - Center Frequency: 433.92 MHz
> - Sample Rate: 2,000,000 samples/sec
> - Format: Complex float32, little-endian
>
> Flag format: flag{freq_mod_period}
> Provide the frequency in Hz and the period in us. For the symbol period, answer with the shortest fundamental period.
> Example: flag{500000000_ASK_250}
> 
> 日本語
> 駐車場周辺で取得した433 MHz帯のIQデータを入手した。 この中には複数の無線機器の送信が含まれている。 Falcon X1のKeyfob信号を特定し、以下を答えよ。
> 
> - 中心周波数
> - 変調方式
> - シンボル周期
> なお、添付ファイルは以下の条件で記録されたraw IQデータである。
> 
> - Center Frequency: 433.92 MHz
> - Sample Rate: 2,000,000 samples/sec
> - Format: Complex float32, little-endian
>
> フラグフォーマット: flag{freq_mod_period}
> 周波数はHz、周期はusで答えること。シンボル周期は最短の基本周期を答えること。
> 例: flag{500000000_ASK_250}

### 解法
（筆者注：以下Keyfobの問題は基本的にCodexが解いてくれました。以下Codexより）

配布された `.bin` は IQ データでした。IQ データは、受信した電波を複素数の列として保存したものです。

Falcon X1 keyfob の物理層パラメータを求めます。問題文には、中心周波数が 433.92 MHz、サンプルレートが 2 Msps と書かれていました。

配布データは複素 float32 little-endian の IQ データでした。I と Q が `float32` で交互に並んでいるので、次のように複素数列へ戻せます。

```python
import numpy as np

FS = 2_000_000
FC = 433_920_000


def load_iq(path):
    raw = np.memmap(path, dtype="<f4", mode="r")
    return raw[0::2] + 1j * raw[1::2]
```

どの周波数に信号がいるかを見るには FFT を使いました。FFT は Fast Fourier Transform の略で、時間方向の信号を周波数方向へ変換する計算です。無線解析では、中心周波数から何 Hz ずれたところに強い信号があるかを見るために使います。

主な信号は 3 種類ありました。OOK は On-Off Keying の略で、搬送波を出す・止めることで 0/1 を表す変調方式です。

```text
433.920 MHz 付近: 長い OOK フレームが3回ずつ繰り返し
433.960 MHz 付近: 9 ms 程度の短い交互パルス
433.840 MHz 付近: 28 ms 程度の単純パターン
```

Falcon X1 らしいのは、433.920 MHz ちょうどの信号でした。80 ms 程度のフレームが 3 回ずつ再送されており、リモコンのボタン送信らしい形です。

OOK のパルス幅を測ると、ON/OFF の長さが `500 us` と `1000 us` にそろっていました。

```python
iq = load_iq("capture_c1c2.bin")

# 3.2 秒付近の 433.920 MHz フレームを切り出す
start = int(3.2 * FS)
dur = int(0.082 * FS)
x = iq[start:start + dur]

env = np.convolve(np.abs(x), np.ones(40) / 40, mode="same")
lo, hi = np.percentile(env, [10, 90])
on = env > ((lo + hi) / 2)

idx = np.where(on)[0]
runs = []
state = on[idx[0]]
run_start = idx[0]
for i in range(idx[0] + 1, idx[-1] + 1):
    if on[i] != state:
        runs.append((state, (i - run_start) / FS * 1e6))
        state = on[i]
        run_start = i
runs.append((state, (idx[-1] + 1 - run_start) / FS * 1e6))

print(runs[:20])
```

出てくる幅は次のような値です。

```text
500.0 us, 1000.0 us, 1000.5 us, 499.5 us, ...
```

したがって、中心周波数は `433920000 Hz`、変調は OOK、最短の基本周期は `500 us` です。問題文の例は `ASK` でしたが、ジャッジ上は OOK 表記でした。OOK は ASK の一種ですが、搬送波を完全に ON/OFF する場合は OOK と呼ぶのが自然です。


:::details flag  (Click to expand)
```
flag{433920000_OOK_500}
```
:::

## Read the simple fob (easy, 100pt, 83 solves)
### 課題
>English
>The file capture_c1c2.bin used in the previous challenge contains a Falcon X1 keyfob signal. The Unlock frame of this keyfob is Manchester-encoded. Locate a complete Unlock frame in the IQ data, perform Manchester decoding, and analyze the decoded data to recover the Device ID. The Device ID is defined as the 3 bytes immediately following the sync word 0xD5.
>
>Flag format: flag{XXXXXX}
>XXXXXX must be an uppercase hexadecimal string, without the 0x prefix.
>Example: flag{AABBCC}
>
>日本語
>一つ前のチャレンジで使用したcapture_c1c2.binには、Falcon X1のKeyfob信号が含まれている。 このKeyfobのUnlockフレームはManchester 符号化されている。 IQデータ中から完全なUnlockフレームを特定し、Manchester 復号後のデータを解析してDevice IDを求めよ。 Device IDは、同期語0xD5の直後から始まる3 byteとする。
>
>フラグフォーマット: flag{XXXXXX}
>XXXXXXは大文字16進数とし、0xを付けないこと。
>例: flag{AABBCC}

### 解法

2 問目では、同じ `capture_c1c2.bin` から Unlock フレームを Manchester 復号し、Device ID を求めます。Device ID はリモコン個体を表す ID です。問題文では、同期語 `0xD5` の直後 3 バイトと定義されていました。

Manchester 符号化は、1 ビットを 2 つの短いチップに分け、`01` や `10` のような遷移で 0/1 を表す符号化です。クロックを復元しやすいため、リモコンや低速無線でよく見ます。復号では、2 チップを 1 ビットへ戻します。

```python
def manchester_decode(chips):
    pairs = [chips[i:i+2] for i in range(0, len(chips) - 1, 2)]
    table = {"01": "0", "10": "1"}
    bits = "".join(table.get(p, "?") for p in pairs)
    if "?" in bits:
        raise ValueError("invalid Manchester pair found")
    return bits


def bits_to_hex(bits):
    n = len(bits) // 8 * 8
    return "".join(f"{int(bits[i:i+8], 2):02X}" for i in range(0, n, 8))
```

OOK の復調は、対象周波数を 0 Hz 付近へ移動（ミックスダウン）してから振幅を見ます。

```python
def mix_down(iq, offset_hz, fs=FS):
    t = np.arange(len(iq), dtype=np.float64) / fs
    return iq * np.exp(-1j * 2 * np.pi * offset_hz * t)


def ook_bits_from_envelope(x, chip_us, fs=FS, smooth_us=10):
    win = max(1, int(smooth_us * 1e-6 * fs))
    env = np.convolve(np.abs(x), np.ones(win) / win, mode="same")
    lo, hi = np.percentile(env, [10, 90])
    b = env > ((lo + hi) / 2)

    idx = np.where(b)[0]
    start, end = idx[0], idx[-1]
    chip = int(chip_us * 1e-6 * fs)

    chips = []
    for k in range((end - start + 1) // chip):
        a = start + k * chip + chip // 4
        c = start + (k + 1) * chip - chip // 4
        chips.append("1" if np.mean(b[a:c]) > 0.5 else "0")
    return "".join(chips)
```

前問で分かった通り、Falcon X1 は 433.920 MHz、OOK、500 us チップです。チップ列を取り出して Manchester 復号します。

```python
iq = load_iq("Parking Lot Whispter/capture_c1c2.bin")

def extract_falcon_frame(start_s):
    start = int((start_s - 0.001) * FS)
    x = iq[start:start + int(0.084 * FS)]
    chips = ook_bits_from_envelope(x, chip_us=500)
    bits = manchester_decode(chips)
    return bits_to_hex(bits)

for t in [3.2000, 3.2911, 3.3833, 7.0994, 7.1916, 7.2837]:
    print(t, extract_falcon_frame(t))
```

得られたフレームは同一でした。

```text
AA AA D5 7A 21 CC 20 01 37 5A
```

`D5` の直後 3 バイトが Device ID です。

:::details flag  (Click to expand)
```
flag{7A21CC}
```
:::

## Next Counter (easy, 100pt, 73 solves)
### 課題

>English
>Falcon X1 keyfob transmissions from a different device were captured multiple times. Analyze the frames, identify the pattern exhibited by this keyfob, and predict the frame that will be transmitted during the next Unlock action.
>
>Flag format: flag{FRAMEHEX}
>FRAMEHEX must be an uppercase hexadecimal string without the 0x prefix.
>Example: flag{1111FFAABBCC10EEEEFF}
>
>日本語
>Falcon X1の別個体のKeyfobから送信された複数回分のUnlock信号を取得した。 各フレームを解析し、このKeyfobに見られる規則性を分析し、次のUnlock操作で送信されるフレームを予測せよ。
>
>フラグフォーマット: flag{FRAMEHEX}
>FRAMEHEXは大文字16進数とし、0xを付けないこと。
>例: flag{1111FFAABBCC10EEEEFF}

### 解法
3 問目では、別個体の Falcon X1 keyfob から複数回の Unlock 信号が与えられます。次の Unlock フレームを予測する問題です。

`capture_c3.bin` も物理層は同じでした。433.920 MHz の OOK 信号を探すと、80 ms 程度のフレームが 15 本ありました。3 回ずつ同じ内容が出ているので、5 回の Unlock 操作が記録されています。

バイトごとに見ると、変化しているのは 9 バイト目だけでした。

```text
AA AA D5 9C 4E 2B 20 01 37 5A
AA AA D5 9C 4E 2B 20 01 38 5A
AA AA D5 9C 4E 2B 20 01 39 5A
AA AA D5 9C 4E 2B 20 01 3A 5A
AA AA D5 9C 4E 2B 20 01 3B 5A
```

ここでは `0x0137` から `0x013B` までの 16 bit カウンタが入っていると読めます。カウンタは、ボタン操作ごとに増える値です。次の Unlock 操作では `0x013C` になります。

:::details flag  (Click to expand)
```text
flag{AAAAD59C4E2B20013C5A}
```
:::

## Predict Next UNLOCK (easy, 100pt, 71 solves)
### 課題
> English
>Multiple Unlock transmissions from another Falcon X1 keyfob were captured. Analysis of the frames shows that, in addition to the Counter, there is another value that changes between transmissions. Based on the observed pattern, predict the unique valid Unlock frame that will be transmitted next by this keyfob.
>
>Flag format: flag{FRAMEHEX}
>FRAMEHEX must be an uppercase hexadecimal string, without the 0x prefix.
>
>日本語
>Falcon X1の別個体のKeyfobから送信された複数回分のUnlock信号を取得した。 各フレームを解析すると、カウンタとは別に、もう1つ変化する値が含まれている。 観測された規則性に基づき、このKeyfobから次回送信される一意の有効なUnlockフレームを予測せよ。
>
>フラグフォーマット: flag{FRAMEHEX}
>FRAMEHEXは大文字16進数とし、0xを付けないこと。

### 解法
4 問目では、カウンタに加えてもう 1 つ変化する値が入ります。`capture_c4.bin` を同じ方法で復号すると、12 バイトのフレームが 4 操作分得られました。

```text
AA AA D5 B8 4F 62 20 02 04 C1 8B 5A
AA AA D5 B8 4F 62 20 02 05 D4 C2 5A
AA AA D5 B8 4F 62 20 02 06 E7 F9 5A
AA AA D5 B8 4F 62 20 02 07 FB 30 5A
```

カウンタは `0x0204` から `0x0207` へ 1 ずつ増えています。さらに、その次の 2 バイトを見ると次のようになります。

```text
C18B
D4C2
E7F9
FB30
```

差分を取ると、毎回 `0x1337` 加算です。

```python
vals = [0xC18B, 0xD4C2, 0xE7F9, 0xFB30]
for a, b in zip(vals, vals[1:]):
    print(hex((b - a) & 0xFFFF))
    # 0x1337
```

したがって次は、カウンタ `0x0208`、追加値 `0xFB30 + 0x1337 = 0x0E67` です。

:::details flag  (Click to expand)
```
flag{AAAAD5B84F622002080E675A}
```
:::

## Classic KeeLoq Garage: Find the Key (medium, 100pt, 67 solves)
### 課題
>English
>An RF capture from a garage remote was obtained, and an installer’s note was also recovered at the scene. It was determined that this system uses a classic rolling code scheme. Analyze the RF capture and the note, and recover the 64-bit DeviceKey used by this remote. The synchronization word is the same as in the previous challenges.
>
>Flag format: flag{KEY64}
>KEY64 must be an uppercase hexadecimal string without the 0x prefix.
>Example: flag{AABBCCDDEEFF0011}
>
>日本語
>あるガレージ用リモコンのRFキャプチャを入手した。また、現場から設置業者のメモも回収されている。 このシステムは古典的なrolling codeを使用していることがわかった。 RFキャプチャとメモを分析し、このリモコンで使用されている64bitのDeviceKeyを復元せよ。 なお、同期語はこれまでのチャレンジと同様である。
>
>フラグフォーマット: flag{KEY64}
>KEY64は大文字16進数とし、0xを付けないこと。
>例: flag{AABBCCDDEEFF0011}

### 解法
5 問目から KeeLoq に入ります。KeeLoq は Microchip 系の車両・ガレージリモコンで古くから使われてきた 32 bit ブロック、64 bit 鍵の rolling code 暗号です。

リモコンは毎回カウンタを暗号化した Hop、つまり hopping code を送信します。受信機は同じ DeviceKey で復号し、カウンタが許容範囲内なら受理します。

`memo_c5c6.txt` が配布されました。

```text
Model: KG-370
Button map: 0x01 = OPEN
Fixed part: SERIAL(32) || BTN(8)
Legacy derive: K = SEED || (SEED XOR SERIAL)
Seed: 6D3A91C4
Disc bits: lower 10 bits
Learn window: 16
```

`SERIAL(32) || BTN(8)` は、32 bit の Serial と 8 bit の Button をそのまま後ろへ並べるという意味です。

`capture_c5c6.bin` を Manchester 復号すると、次のようなフレームが得られました。

```text
AA AA D5 91 3B 00 D7 01 D4 A2 B7 01
AA AA D5 00 20 D1 FB 01 D4 A2 B7 01
AA AA D5 80 ED 14 5B 01 D4 A2 B7 01
AA AA D5 31 4C 03 E4 01 D4 A2 B7 01
```

`D5` の後は `ENC(32) || SERIAL(32) || BTN(8)` と読めます。`ENC(32)` は暗号化された 32 bit Hop です。固定部は常に `01D4A2B7 01` なので、Serial は `01D4A2B7`、Button は `01` です。メモの `0x01 = OPEN` と一致します。

メモにある古い導出式は単純です。

```text
K = SEED || (SEED XOR SERIAL)
```

```python
seed = 0x6D3A91C4
serial = 0x01D4A2B7
key = (seed << 32) | (seed ^ serial)
print(f"{key:016X}")
# 6D3A91C46CEE3373
```

:::details flag  (Click to expand)
```
flag{6D3A91C46CEE3373}
```
:::

## Classic KeeLoq Garage: Next HOP (medium, 100pt, 61 solves)
### 課題
>English
>Using the DeviceKey recovered in Find the Key, analyze the rolling code used by this garage remote. The Hop field is encrypted using the standard KeeLoq algorithm. Predict the next valid Open-signal frame. The Hop field is a 32-bit Encrypted Hop, and the counter is 16 bits.
>
>Flag format: flag{FRAMEHEX}
>FRAMEHEX must be an uppercase hexadecimal string without the 0x prefix.
>
>日本語
>Find the Keyで求めたDeviceKeyを用いて、このガレージ用リモコンのrolling codeを解析せよ。 Hopの暗号化には標準のKeeLoqアルゴリズムが用いられている。 次に有効となるOpen信号のフレームを予測せよ。 Hopは32-bit Encrypted Hop, カウンタは16bitである。
>
>フラグフォーマット: flag{FRAMEHEX}
>FRAMEHEXは大文字16進数とし、0xを付けないこと。

### 解法
KeeLoq の標準アルゴリズムは 528 ラウンドの非線形シフトレジスタです。NLF は Non-Linear Function の略で、KeeLoq 内部で使う 5 入力の非線形関数です。ここでは一般的な定数 `0x3A5C742E` を使います。

```python
NLF = 0x3A5C742E


def bit(x, n):
    return (x >> n) & 1


def g5(x, a, b, c, d, e):
    return (
        bit(x, a)
        | (bit(x, b) << 1)
        | (bit(x, c) << 2)
        | (bit(x, d) << 3)
        | (bit(x, e) << 4)
    )


def keeloq_encrypt(data, key):
    x = data
    for r in range(528):
        fb = (
            bit(x, 0)
            ^ bit(x, 16)
            ^ bit(NLF, g5(x, 1, 9, 20, 26, 31))
            ^ bit(key, r & 63)
        )
        x = ((x >> 1) | ((fb & 1) << 31)) & 0xFFFFFFFF
    return x


def keeloq_decrypt(data, key):
    x = data
    for r in range(528):
        fb = (
            bit(x, 31)
            ^ bit(x, 15)
            ^ bit(NLF, g5(x, 0, 8, 19, 25, 30))
            ^ bit(key, (15 - r) & 63)
        )
        x = (((x << 1) & 0xFFFFFFFF) | (fb & 1)) & 0xFFFFFFFF
    return x
```

前問の Hop を復号すると、カウンタが連番になりました。

```python
key = 0x6D3A91C46CEE3373
for enc in [0x913B00D7, 0x0020D1FB, 0x80ED145B, 0x314C03E4]:
    print(f"{enc:08X} -> {keeloq_decrypt(enc, key):08X}")
```

出力です。

```text
913B00D7 -> ADC40040
0020D1FB -> ADC40041
80ED145B -> ADC40042
314C03E4 -> ADC40043
```

次の Hop 平文は `ADC40044` です。これを暗号化します。

```python
next_plain = 0xADC40044
next_enc = keeloq_encrypt(next_plain, key)
print(f"{next_enc:08X}")
# 65317ADF
```

固定部は `SERIAL || BTN = 01D4A2B7 || 01` のままです。

:::details flag  (Click to expand)
```
flag{AAAAD565317ADF01D4A2B701}
```
:::


## Real KeeLoq Garage: Normal Learn (hard, 100pt, 54 solves)
### 課題
>English
>Another RF capture from a garage remote was obtained. A note recovered at the scene contains information about the receiver-side configuration. Analyze the RF capture and the note, and recover the 64-bit DeviceKey used by this remote. The synchronization word is the same as in the previous challenges.
>
>Flag format: flag{KEY64}
>KEY64 must be an uppercase hexadecimal string without the 0x prefix.
>Example: flag{AABBCCDDEEFF0011}
>
>日本語
>別のガレージ用リモコンのRFキャプチャを入手した。 現場から回収されたメモには、受信機側の構成に関する情報が残されている。 RFキャプチャとメモを分析し、このリモコンで使用されている64bitのDeviceKeyを復元せよ。 なお、同期語はこれまでのチャレンジと同様である。
>
>フラグフォーマット: flag{KEY64}
>KEY64は大文字16進数とし、0xを付けないこと。
>例: flag{AABBCCDDEEFF0011}

### 解法
7 問目は、より HCS301 互換に近い KeeLoq です。HCS301 は Microchip の KeeLoq encoder で、車両・ガレージリモコンの rolling code 実装としてよく参照されます。

`memo_c7c8.txt` はかなり重要でした。

```text
Model: KG-401
RX board: HCS500 rev.B
Encoder family: HCS301-compatible
Remote label: ??7A4C3D
Button wiring: S0 = OPEN
Learn mode: Normal
Algorithm: KeeLoq
Mfr Code: 89ABCDEF01234567

Disc check: lower bits
Window: 16 / 32K
Seed TX: disabled
Repeat: enabled
Old sticker: one sample looked clipped
```

S0 はボタン入力名です。`S0 = OPEN` なので、S0 に対応するボタンコードが Open 操作です。Discrimination、略して Disc は、復号結果が正しいリモコンのものかを確認するために入る短い識別値です。メモの `Disc check: lower bits` は、Serial の下位ビットが Disc として使われることを示しています。

`capture_c7c8.bin` の本命信号は 434.040 MHz 付近でした。前半の Falcon X1 より短く、250 us チップの Manchester でした。復号すると、66 bit の KeeLoq データ部が出ます。先頭に `AA AA D5` があり、その後に 32 bit Hop と 34 bit 固定部が続きます。

```text
AA AA D5 89 55 C1 B4 04 1E 93 0F + trailing bits
AA AA D5 86 7E 2B 59 04 1E 93 0F + trailing bits
AA AA D5 39 EB 12 8D 04 1E 93 0F + trailing bits
AA AA D5 13 47 2C 1C 04 1E 93 0F + trailing bits
```

固定部 34 bit をそのまま取り出すと、次のビット列でした。

```text
0000010000011110100100110000111101
```

HCS301 互換の固定部は、ここでは `status(2) || button(4) || serial(28)` と読めます。

```text
status = 00
button = 0001
serial = 0000011110100100110000111101 = 07A4C3D
```

メモの `Remote label: ??7A4C3D` は、Serial の下位 24 bit が `7A4C3D` であることを示していました。RF から上位 nibble `0` が補え、Serial は `07A4C3D` と確定します。

Normal Learn は、受信機が Serial と Mfr Code から DeviceKey を作る学習方式です。ここでは HCS500/HCS301 互換の Normal Learn として、次の 2 つの Source を Mfr Code で KeeLoq 復号しました。

```text
SourceH = 0x60000000 | serial
SourceL = 0x20000000 | serial
```

```python
mfr = 0x89ABCDEF01234567
serial = 0x07A4C3D

source_h = 0x60000000 | serial
source_l = 0x20000000 | serial

key_h = keeloq_decrypt(source_h, mfr)
key_l = keeloq_decrypt(source_l, mfr)
device_key = (key_h << 32) | key_l

print(f"SourceH = {source_h:08X}")
print(f"SourceL = {source_l:08X}")
print(f"KeyH    = {key_h:08X}")
print(f"KeyL    = {key_l:08X}")
print(f"Key     = {device_key:016X}")
```

出力です。

```text
SourceH = 607A4C3D
SourceL = 207A4C3D
KeyH    = BF61DA58
KeyL    = FE545128
Key     = BF61DA58FE545128
```

検算として、この DeviceKey で観測 Hop を復号します。

```python
key = 0xBF61DA58FE545128
for enc in [0x8955C1B4, 0x867E2B59, 0x39EB128D, 0x13472C1C]:
    print(f"{enc:08X} -> {keeloq_decrypt(enc, key):08X}")
```

出力です。

```text
8955C1B4 -> 103D0040
867E2B59 -> 103D0041
39EB128D -> 103D0042
13472C1C -> 103D0043
```

下位 16 bit が `0x0040` から `0x0043` へ増えています。さらに、bits 16..25 は `0x03D` で、Serial `0x07A4C3D` の下位 10 bit と一致します。これで鍵が正しいと確認できます。

:::details flag  (Click to expand)
```
flag{BF61DA58FE545128}
```
:::

## Real KeeLoq Garage: Next HOP (hard, 238pt, 32 solves)
### 課題
>English
>Using the 64-bit DeviceKey recovered in Normal Learn, analyze the rolling code used by this garage remote. Decrypt the 32-bit Encrypted Hop contained in the RF capture, and identify the pattern in the observed Open-signal sequence. Based on that result, predict the next valid logical frame for the Open signal.
>
>Flag format: flag{FRAMEHEX}
>FRAMEHEX must be an uppercase hexadecimal string without the 0x prefix.
>
>日本語
>Normal Learnで特定した64-bitのDeviceKeyを用いて、このガレージ用リモコンのrolling codeを解析せよ。 RFキャプチャに含まれる32-bit Encrypted Hopを復号し、観測されたOpen信号列の規則性を明らかにせよ。 その結果をもとに、次に有効となるOpen信号の論理フレームを予測せよ。
>
>フラグフォーマット: flag{FRAMEHEX}
>FRAMEHEXは大文字16進数とし、0xを付けないこと。

### 解法

最後の問題では、7 問目の DeviceKey で次の Open フレームを予測します。

復号結果は次の通りでした。

```text
8955C1B4 -> 103D0040
867E2B59 -> 103D0041
39EB128D -> 103D0042
13472C1C -> 103D0043
```

HCS301 互換の Hop 平文は、ここでは `button(4) || overflow/status(2) || discrimination(10) || counter(16)` のように読めます。上位 nibble `1` は S0、つまり Open を示します。Disc は `0x03D`、カウンタは `0x0040` から `0x0043` へ増えています。

したがって次の Hop 平文は `0x103D0044` です。

```python
key = 0xBF61DA58FE545128
next_plain = 0x103D0044
next_hop = keeloq_encrypt(next_plain, key)
print(f"{next_hop:08X}")
# 2675F6FA
```

この問題で一番はまった点は、固定部を HEX にする読み方でした。RF のビット列としては固定部 34 bit が次のように並んでいます。

```text
0000010000011110100100110000111101
```

これを送信ビット列としてそのまま右 0 padding して HEX にすると、次のようになります。

```text
0000010000011110100100110000111101 00
= 041E930F4
```

しかし、ジャッジが求めていたのは送信ビット列の padding 表現ではなく、HCS301 の論理フィールドとしての表現でした。固定部を `status(2) || button(4) || serial(28)` と分けると、

```text
status = 00
button = 0001 = 1
serial = 07A4C3D
```

メモの `Remote label: ??7A4C3D` と `Button wiring: S0 = OPEN` を合わせると、論理フレームに入れるべき固定値は `button || serial` です。

```text
button || serial = 1 || 07A4C3D = 107A4C3D
```

`Old sticker: one sample looked clipped` は、ラベルの上位 1 バイトが欠けているというヒントでした。RF から button 1と Serial 上位 nibble 0 を補うと `107A4C3D` になります。

最終的な論理フレームは、前問までと同じく `AA AA D5` を付けて、

```text
AA AA D5 | Encrypted Hop | Button+Serial
AA AA D5 | 2675F6FA      | 107A4C3D
```

です。

```text
flag{AAAAD52675F6FA107A4C3D}
```

# RF Category
## D's Signpost (medium, 272pt, 30 solves)
### 課題
> English
> A radio signal was captured and saved in SigMF format. The capture appears to contain multiple transmissions, and one of them may lead to a hidden message. Can you analyze the signal and recover the transmitted contents?
> 
> 日本語
> ある無線信号をキャプチャしてSigMF形式で保存しました。 このキャプチャには複数の送信が含まれており、その中のどれかが隠されたメッセージにつながっているようです。 信号を解析し、送信された内容を復元できますか？

### 解法
#### 概要
無線信号のキャプチャから隠されたデータを復元する問題です。配布ファイルには SigMF 形式のデータが入っていました。SigMF は無線信号のサンプル本体と、サンプルレートや中心周波数などのメタ情報を組にして扱う形式です。

最終的には 5 本の波を順に調べ、`+115 kHz` のパケット通信からパスワードを得て、`+170 kHz` の位相差に載っていたデータから ZIP ファイルを復元しました。ZIP の中にある `flag.txt` を読むとフラグが得られます。

:::details flag  (Click to expand)
```
FLAG{R4D10_FR3Q_L1TTL3_M4Z3}
```
:::


#### 初期分析

`capture.sigmf-meta` を見ると、サンプルレートは 500 kHz、中心周波数は 915 MHz でした。500 kHz なので、1 秒あたり 500,000 個の IQ サンプルがあります。中心周波数は受信機が合わせていた周波数で、今回の IQ データでは 915 MHz を 0 Hz の基準として、その周辺の信号が相対周波数で入っています。

```json
{
  "core:datatype": "cf32_le",
  "core:sample_rate": 500000.0,
  "core:frequency": 915000000.0
}
```

まず全体を FFT しました。図の横軸は 915 MHz からのずれ、縦軸は信号の強さです。

![全体スペクトル](/images/ndias-automotive-iot-ctf-2026-kn1cht/overall_spectrum.png)

強いピークが 5 本あります。ピークを拾うと、おおよそ次の周波数でした。

```text
 +45013.4 Hz
 -70007.3 Hz
-179992.7 Hz
+170013.4 Hz
+114990.2 Hz
```

中心周波数からの相対値として読みやすく書くと、-180 kHz, -70 kHz, +45 kHz, +115 kHz, +170 kHz です。この時点では、どの波がフラグに直結するかは分かりません。そこで、各ピークを個別に切り出して観察することにしました。

Python では、まず `capture.sigmf-data` を `float32` として読み、I 成分と Q 成分を組にして複素数列に戻しました。

```python
import numpy as np

SAMPLE_RATE = 500_000
raw = np.memmap("capture.sigmf-data", dtype=np.float32, mode="r")
iq = raw[0::2] + 1j * raw[1::2]

print(len(iq) / SAMPLE_RATE)  # 30.0 seconds
```

#### チャンネルの切り出し

各ピークを調べるには、対象の周波数を 0 Hz 付近に移動してから低域だけを残します。この処理をミックスダウンと呼びます。たとえば +115 kHz の信号を見たいときは、+115 kHz を打ち消す複素正弦波を掛けることで、その信号をベースバンド、つまり 0 Hz 付近に移動できます。

そのあとローパスフィルタで対象帯域だけを残し、サンプル数を減らしました。ローパスフィルタは低い周波数だけを通すフィルタです。今回使った FIR フィルタは、周囲のサンプルに重みをかけて足し合わせる素直なデジタルフィルタです。フィルタを入れずにサンプルを間引くと、別の周波数成分が折り返して混ざるエイリアシングが起きやすくなります。

切り出しの中核は次のような処理です。

```python
def lowpass_taps(cutoff_hz, taps=401):
    n = np.arange(taps) - (taps - 1) / 2
    h = 2 * cutoff_hz / SAMPLE_RATE * np.sinc(2 * cutoff_hz / SAMPLE_RATE * n)
    h *= np.hamming(taps)
    h /= np.sum(h)
    return h


def extract_channel(iq, freq, dec=20, cutoff=8_000):
    t = np.arange(len(iq), dtype=np.float64)
    mixed = iq * np.exp(-1j * 2 * np.pi * freq / SAMPLE_RATE * t)

    h = lowpass_taps(cutoff)
    keep = len(iq) - len(h)
    out_len = keep // dec
    y = np.zeros(out_len, dtype=np.complex128)
    for k, c in enumerate(h):
        y += c * mixed[k:k + out_len * dec:dec]
    return y, SAMPLE_RATE / dec
```

#### -180 kHz のモールス信号

-180 kHz を拡大して見ると、振幅がオン・オフ的に切り替わっていました。AM は Amplitude Modulation の略で、振幅の変化に情報を載せる方式です。ここでは細かい搬送波そのものではなく、振幅の外形、つまり AM 包絡を見ます。

![morse -180 kHz](/images/ndias-automotive-iot-ctf-2026-kn1cht/morse_m180000_am.png)

時間軸で確認すると、ON になっている長さが短いものと長いものの 2 種類に分かれていました。さらに OFF の長さにも、同じ文字内の区切り、文字間の区切り、単語間の区切りらしい規則がありました。短い ON と長い ON が規則的に並ぶのは、モールス符号の典型的な特徴です。

そこで、AM 包絡を平滑化して閾値で ON/OFF に分け、ON 時間が短いものを ・、長いものを ー として書き起こしました。

```python
y, sr = demod(iq, -179_993, "am")
env = np.convolve(y, np.ones(int(0.012 * sr)) / int(0.012 * sr), mode="same")
threshold = np.percentile(env, 10) + (np.percentile(env, 90) - np.percentile(env, 10)) * 0.45
bits = env > threshold

# bits の連続区間を測り、ON の短い区間を dot、長い区間を dash として読む
```

書き起こしたモールス符号です。

> ー ・・・・ ・・ ・・・ / ・・ー・ ・・ ・ー・・ ・ / ・・・・ ・ー ・・・ / ・・・・・ / ・ーー ・ー ・・・ー ・ ・・・


これを文字に戻すと、次のメッセージになります。

```text
this file has 5 waves
```

これで、スペクトル上の 5 本のピークが問題の構造そのものだと分かりました。

#### 音声化とヒント

5 本の波について、AM 復調と FM 復調を試し、WAV とスペクトログラムを作りました。FM は Frequency Modulation の略で、周波数の揺れに情報を載せる方式です。IQ データでは、隣り合うサンプルの位相差を見ることで FM 復調できます。

```python
def demod(iq, freq, mode):
    x, sr = extract_channel(iq, freq)
    if mode == "am":
        y = np.abs(x)
    elif mode == "fm":
        y = np.angle(x[1:] * np.conj(x[:-1])) * sr / (2 * np.pi)
        y = np.r_[y[0], y]
    return y, sr
```

（筆者注）Codexはここからノイズをなるべく除去してクリーンな音声を取り出すことに20分ほどを費やし始めました。その間、ローカルフォルダに保管されたWAVファイルを聴くことができたので、人力で文字起こししてLLMにヒントとして提供することで分析が進行しました。

+45 kHz は人の声のように聞こえる波でした。

![+45 kHz baseband](/images/ndias-automotive-iot-ctf-2026-kn1cht/speech_p45013_baseband.png)

ユーザが直接 WAV を聴取すると、不完全ながら次のように聞こえました。

```text
Do you know APRS? APRS has a super secret password.
```

ユーザはATRSかAPRSのどちらなのか迷っていましたが、この名称が重要でした。APRS は Automatic Packet Reporting System の略で、アマチュア無線で短いメッセージや位置情報を送るために使われるパケット通信です。APRS はしばしば 1200 bps の Bell 202 AFSK で送られます。AFSK は Audio Frequency Shift Keying の略で、2 種類の音の高さを使って 0/1 を送る方式です。

-70 kHz にも音声ヒントがありました。

![-70 kHz phase hint](/images/ndias-automotive-iot-ctf-2026-kn1cht/phase_hint_m70007_baseband.png)

こちらは次のように聞こえました。

```text
This is ... data in phase changes.
Before each symbols by comparing it with the previous.
```

正確な英文ではないかもしれませんが、phase changes と previous が聞こえる点が重要です。これは、各シンボルの絶対的な位相ではなく、前のシンボルからどれだけ位相が変わったかを見る方式を示しています。つまり、差動位相変調を疑うヒントです。DBPSK は 2 種類の位相差で 1 ビットを表し、DQPSK は 4 種類の位相差で 2 ビットを表します。

#### +115 kHz の APRS 復号

+115 kHz は短い送信が複数回出ている波でした。

![+115 kHz APRS candidate](/images/ndias-automotive-iot-ctf-2026-kn1cht/aprs_p114990_baseband.png)

APRS は AX.25 というフレーム形式を使います。AX.25 は無線パケット用の形式で、宛先、送信元、本文、誤り検出用の FCS を持ちます。FCS は CRC の一種で、復号したビット列が正しいかをかなり強く判定できます。

復号では、FM 復調した信号から 1200 Hz と 2200 Hz のどちらが強いかを見ました。Bell 202 AFSK では、この 2 つのトーンがビットの状態に対応します。その後、AX.25 で使われる NRZI を戻し、HDLC フラグ `0x7e` を探し、ビット詰めを外してから CRC を確認しました。NRZI は、信号レベルそのものではなく、状態が反転したかどうかでビットを表す符号化です。HDLC フラグはパケットの区切りを示す目印です。

シンボル境界、信号反転、トーンの順序、切り出し時間は総当たりしました。無線信号では、サンプルの開始位置がビット境界に揃っているとは限らないため、このような小さな総当たりが効きます。

```python
def try_decode(freq, mark, space, start=0, end=30):
    y, sr = demod(iq, freq, "fm")
    y = y[int(start * sr):int(end * sr)]
    metric = tone_metric(y, sr, mark=mark, space=space)

    for phase in np.linspace(0, sr / 1200.0, 64, endpoint=False):
        states = symbol_states(metric, sr, phase=phase)
        for inverted in (False, True):
            bits = nrzi_decode(states, inverted)
            for frame in find_ax25_frames(bits):
                yield frame
```

CRC が通った APRS パケットは次の内容でした。

```text
N0CALL-1 > APCTF
>The password is dWt4Wxm6Xfn1ot02
```

ここで、後で使うパスワードらしい文字列が得られました。

```text
dWt4Wxm6Xfn1ot02
```

#### +170 kHz は APRS ではない

![+170 kHz data candidate](/images/ndias-automotive-iot-ctf-2026-kn1cht/dqpsk_p170013_baseband.png)

最初は +115 kHz と同じく APRS を疑いました。しかし APRS デコーダにかけても、CRC が通る AX.25 フレームは出ませんでした。そこで -70 kHz の音声メッセージにある「前のシンボルと比較する」というヒントに戻り、位相差を詳しく見ることにしました。

序盤は DBPSK として 0/1 を取り出し、ビット反転、ビットずれ、MSB first/LSB first、XOR、base64 などを試しました。MSB first と LSB first は、1 バイトを上位ビットから読むか、下位ビットから読むかの違いです。しかしノイズ由来のそれらしい候補が多く、決定打がありませんでした。DBPSK は 2 値の位相差しか使わないため、今回の信号を説明するには足りなさそうでした。

そこで +170 kHz を 250 baud と仮定してシンボルごとにサンプリングし、隣り合うシンボルの位相差をヒストグラムにしました。baud は 1 秒あたりのシンボル数です。250 baud なら 1 秒に 250 個のシンボルが送られます。切り出し後のサンプルレートは 25 kHz なので、1 シンボルは 100 サンプルになります。

![DQPSK phase histogram](/images/ndias-automotive-iot-ctf-2026-kn1cht/dqpsk_phase_hist_p170013.png)

位相差は 0, +pi/2, -pi/2, pi 付近に山を作っています。これは 2 値ではなく 4 値の差動位相変調です。つまり、DBPSK ではなく DQPSK と考えられます。DQPSK は Differential Quadrature Phase Shift Keying の略で、4 種類の位相変化を使って 1 シンボルあたり 2 ビットを運びます。

DQPSK として読む部分は、次のような処理です。

```python
def sample_symbols(z, boundary, sps=100):
    centers = np.arange(boundary + sps // 2, len(z), sps, dtype=int)
    return np.array([np.mean(z[c-20:c+20]) for c in centers])


def dqpsk_bits(symbols, phase_map):
    dphi = np.angle(symbols[1:] * np.conj(symbols[:-1]))
    q = np.round(dphi / (np.pi / 2)).astype(int) % 4

    bits = []
    for v in q:
        bits.extend(phase_map[v])
    return np.array(bits, dtype=np.uint8)
```

#### ZIP の復元

+170 kHz について、切り出す時間範囲、残留周波数ずれ、シンボル境界、4 種類の位相差と 2 ビット値の対応、ビット反転、バイト境界、MSB first/LSB first を探索しました。

ZIP ファイルは先頭付近に `PK\x03\x04` という特徴的なシグネチャを持ちます。そのため、復元したバイト列の中に `PK` や `flag.txt` が見えるかを手がかりにできます。

```python
raw = bits_to_bytes(bits[skip:], lsb=False)

if b"PK\x03\x04" in raw or b"flag" in raw.lower():
    start = raw.find(b"PK\x03\x04")
    blob = raw[start:]
```

最終的には、`+170 kHz` の 21.3 秒から 25.35 秒あたりを 250 baud DQPSK として読むと、ZIP のローカルヘッダと `flag.txt` が見えました。

```text
PK 03 04 ... flag.txt
```

この ZIP はパスワード付きでした。ここで +115 kHz の APRS から得た `dWt4Wxm6Xfn1ot02` を使うと、中身を展開できました。

```python
import io
import zipfile

with zipfile.ZipFile(io.BytesIO(blob)) as zf:
    flag = zf.read("flag.txt", pwd=b"dWt4Wxm6Xfn1ot02").decode().strip()
print(flag)
```

#### 最終ソルバ
```python
import io
import itertools
import zipfile

from aprs_decode import try_decode
from clean_extract import load_iq, extract_channel
from dbpsk250_extract import carrier_correct
from dqpsk250_decode import dqpsk_bits, bits_to_bytes, sample_symbols


def recover_aprs_password():
    frames = try_decode(114_990, 1200, 2200, 0, 30)
    for _, _, (_, _, pkt) in frames:
        marker = "The password is "
        if marker in pkt["info"]:
            return pkt["info"].split(marker, 1)[1].strip()
    raise RuntimeError("APRS password was not found")


def recover_zip_blob(iq):
    x, sr = extract_channel(iq, 170_013, cutoff=12_000)
    z = x[int(21.3 * sr):int(25.35 * sr)]
    z, _ = carrier_correct(z, sr)

    phase_map = list(itertools.permutations([(0, 0), (0, 1), (1, 0), (1, 1)]))[0]
    symbols = sample_symbols(z, boundary=13, sps=100)
    bits = dqpsk_bits(symbols, phase_map)
    raw = bits_to_bytes(bits[26:], lsb=False)

    start = raw.find(b"PK\x03\x04")
    if start < 0:
        raise RuntimeError("ZIP local header was not found")
    return raw[start:]


iq = load_iq()
password = recover_aprs_password()
blob = recover_zip_blob(iq)

with zipfile.ZipFile(io.BytesIO(blob)) as zf:
    flag = zf.read("flag.txt", pwd=password.encode()).decode().strip()

print(f"APRS password: {password}")
print(f"Flag: {flag}")
```

出力は次のようになります。

```text
Loading SigMF data...
APRS password: dWt4Wxm6Xfn1ot02
Flag: FLAG{R4D10_FR3Q_L1TTL3_M4Z3}
```

#### フラグ
:::details flag  (Click to expand)
```
FLAG{R4D10_FR3Q_L1TTL3_M4Z3}
```
:::
