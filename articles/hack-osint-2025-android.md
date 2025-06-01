---
title: "HACK'OSINT 2025 Write-up (Le cote obscur/Communication)"
emoji: "📱"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - OSINT
  - CTF
published: true
---

2025年5月23日から25日にかけて開催された[HACK'OSINT 2025](https://ctf.hackolyte.fr/)のWrite upです。

このCTFでは、プレイヤーはAPT-509と呼ばれる架空のサイバー犯罪集団のメンバーについて調査します。調査の途中、Mikeという人物が作成した釣りに関するスマートフォンアプリが彼らの隠れた連絡手段であることがわかり、アプリについて調査する課題が2つ与えられました。

オフィシャルWrite upによると、想定される解法はアプリを実際に動作させることでした。

![](/images/hack-osint-2025-android/fishingapp.png)

ただ、サイバー犯罪組織が作成したアプリをいきなりインストールするのには抵抗がある（もちろん、CTFのための架空の組織です）うえ、検証に使用しやすいAndroid端末が私の手元にありませんでした。そこで、このWrite upでは、アプリを実行せずに静的な解析手段を用いて2問を解いてみます。

## Le cote obscur (50 points / 36 solves)
>🇫🇷 – Quelle drôle d'application ! Une fois lancée, celle-ci semble être une façade dissimulant les activités cybercriminelles du groupe APT-509. L’un de nos experts a analysé l'application et a constaté, en examinant son code source, qu'une adresse e-mail y était potentiellement dissimulée. Malheureusement, il n'a pas réussi à la localiser.
>
>En naviguant sur l'application, pouvez-vous aider notre expert à retrouver cette adresse ?
>
>🇬🇧 – What a strange application! Once launched, it appears to be a front hiding the cybercriminal activities of the APT-509 group. One of our experts analyzed the app and discovered, while inspecting its source code, that an email address might be hidden within it. Unfortunately, he was unable to locate it.
>
>While navigating through the application, could you help our expert find this address ?
>
>Flag format : cenestpasladressemaildhackolytequonrecherche@stpneflagpasca.fr

アプリケーションに隠されているEメールアドレスを探す課題です。

アプリのXAPKファイルはAPK配布サイトでダウンロードできます。

https://apkpure.net/fishingapp/com.hackosint.myapplication

XAPKやAPKファイルの実態はZIPアーカイブなので、拡張子を `.zip`に変更すると簡単に解凍できます。

![](/images/hack-osint-2025-android/xapk-extract.png)

5つのAPKファイルが含まれていますが、`com.hackosint.myapplication.apk`がメインだと思われます。これをさらにunzipしてもよいのですが、デコンパイルも同時に行った方が楽なため`apktool`を使用します。

https://github.com/iBotPeaches/Apktool

```bash
$ apktool d com.hackosint.myapplication.apk
$ ls com.hackosint.myapplication
AndroidManifest.xml  apktool.yml  assets  original  res  smali
  smali_classes2  unknown
```

抽出されたファイルは25,696個もあり、手動で探すのは大変そうです。今回探したいのはEメールアドレスなので、`***@***.***`のようなパターンをgrepで探します。
（一般的なメールアドレスの形式にマッチする正規表現をLLMに教えてもらいました。）

```bash
$ grep -rE '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' com.hackosint.myapplication
com.hackosint.myapplication/smali_classes2/com/hackosint/myapplication/
MainActivityKt$CreditsScreen$1.smali:    const-string v0, "str3etf1sher@mail.com"
com.hackosint.myapplication/smali_classes2/com/hackosint/myapplication/
MainActivityKt$CreditsScreen$1.smali:    const-string v0, "str3etf1sher@mail.com"
```

これがこのアプリケーションに含まれている唯一のEメールアドレスのようです。

flag: `str3etf1sher@mail.com`

## Communication (50 points / 37 solves)
>🇫🇷 – Il semble également qu'APT-509 utilise un autre canal de communication, en plus de cette application, pour échanger entre ses membres. Pourriez-vous nous préciser la date de création de cet autre canal ?
>
>🇬🇧 – It also appears that APT-509 is using another communication channel, in addition to this application, to exchange information among its members. Could you please provide us with the creation date of this other channel?
>
>Flag format : JJ/MM/AAAA

FishingApp以外にもAPT-509が使用しているコミュニケーション手段を突き止め、作成日を答える課題です。これまでの調査で、FishingAppの作者のコードネームはMikeであることがわかっています。

```bash
$  $ grep -r 'Mike' com.hackosint.myapplication
com.hackosint.myapplication/smali_classes2/com/hackosint/myapplication/
MainActivityKt.smali:    const-string v10, "Mikee"
```

有望なsmaliファイルがヒットしました。smaliファイルはAndroidアプリケーションで用いられるバイトコードを人間でも読めるようにしたものです。ヒットした部分の周辺を見てみると、チャットの記録がそのまま書かれているように見えます。

```smali:MainActivityKt.smali
    .line 848
    new-instance v4, Lcom/hackosint/myapplication/ChatMessage;

    const-string v8, "2"

    const-string v9, "user3"

    const-string v10, "Mikee"

    const-string v11, "Merci de l\'information!"

    const-string v12, "10:34"

    move-object v7, v4

    invoke-direct/range {v7 .. v19}, Lcom/hackosint/myapplication/ChatMessage;-><init>(Ljava/lang/String;Ljava/lang/String;Ljava/lang/String;Ljava/lang/String;Ljava/lang/String;ZZLjava/lang/String;Ljava/lang/String;Ljava/lang/String;ILkotlin/jvm/internal/DefaultConstructorMarker;)V

    aput-object v4, v3, v6
```

チャットは長くないためそのまま読んでも良いですし、ChatGPTに渡してさらに見やすい形式に変えてもらうこともできます。

| ID | user  | nickname  | message                                                                                        | time  |
|----|-------|-----------|------------------------------------------------------------------------------------------------|-------|
| 1  | user1 | G0lf-GTI  | Salut tout le monde, #A nous a fourni un nouveau moyen de communication temporaire : APT509TMP | 10:30 |
| 2  | user3 | Mikee     | Merci de l'information!                                                                        | 10:34 |
| 3  | user1 | Br4v0     | je venais à peine de trouver comment arriver ici et je dois déjà changer..                     | 10:35 |
| 4  | user3 | n0vember  | J'essaye de rejoindre au plus vite merci!                                                      | 10:37 |
| 5  | user3 | n0vember  | T'as pu rejoindre grâce à une super guide :)                                                   | 10:42 |

通信手段はAPT-509TMPという名前のようです。これはTelegramのグループ名ではないかと想像され、実際 t.me/APT509TMP にアクセスできました。

![](/images/hack-osint-2025-android/apt509tmp.png)

Telegramのメッセージの先頭は4月26日の2:51に投稿されていました。

![](/images/hack-osint-2025-android/telegram.png)

しかし、私たちのチームは日本（UTC+9）にいましたが、これはフランスのCTFなので、現地の時間では4月25日だと考えるのがより適切です。

flag: `25/04/2025`
