# HACK'OSINT 2025 Write-up (Le cote obscur/Communication)

This is a write-up of [HACK'OSINT 2025](https://ctf.hackolyte.fr/), held from May 23 to 25, 2025.

(Japanese version: https://zenn.dev/kn1cht/articles/hack-osint-2025-android)

In this CTF, players investigate members of a fictional cybercriminal group called APT-509. During the investigation, it turned out that a fishing-related smartphone app created by someone named Mike served as their concealed means of communication, and two challenges regarding the app were given.

According to the official write-up, the expected solution was to run the app directly.

![](/images/hack-osint-2025-android/fishingapp.png)

However, I was hesitant to install an app created by a cybercriminal group (albeit fictional for the CTF), and I did not have an Android device handy for testing. Therefore, in this write-up, I will show how to solve both challenges using static analysis methods without running the app.

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

This challenge requires you to locate the email address hidden within the app.

The XAPK file of the app can be downloaded from an APK distribution site.

Since XAPK and APK files are essentially ZIP archives, you can simply change the extension to .zip to extract them.

There are five APK files included, but it appears that `com.hackosint.myapplication.apk` is the main one. Instead of unzipping it again, it is more convenient to use `apktool` to decompile it.

https://github.com/iBotPeaches/Apktool

```bash
$ apktool d com.hackosint.myapplication.apk
$ ls com.hackosint.myapplication
AndroidManifest.xml  apktool.yml  assets  original  res  smali
  smali_classes2  unknown
```

The extracted files number around 25,696, so searching manually would be lengthy. Since we are looking for an email address, we can use grep to search for patterns like `***@***.***`.

(I asked an LLM for a regular expression that matches the general email address format.)

```bash
$ grep -rE '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' com.hackosint.myapplication
com.hackosint.myapplication/smali_classes2/com/hackosint/myapplication/
MainActivityKt$CreditsScreen$1.smali:    const-string v0, "str3etf1sher@mail.com"
com.hackosint.myapplication/smali_classes2/com/hackosint/myapplication/
MainActivityKt$CreditsScreen$1.smali:    const-string v0, "str3etf1sher@mail.com"
```

This is the only email address contained in the app.

flag: `str3etf1sher@mail.com`

## Communication (50 points / 37 solves)
>🇫🇷 – Il semble également qu'APT-509 utilise un autre canal de communication, en plus de cette application, pour échanger entre ses membres. Pourriez-vous nous préciser la date de création de cet autre canal ?
>
>🇬🇧 – It also appears that APT-509 is using another communication channel, in addition to this application, to exchange information among its members. Could you please provide us with the creation date of this other channel?
>
>Flag format : JJ/MM/AAAA

This challenge involves identifying the communication channel used by APT-509 besides the FishingApp and providing its creation date. 
Previous investigations revealed that the codename of the FishingApp's creator is Mike.

```bash
$ grep -r 'Mike' com.hackosint.myapplication
com.hackosint.myapplication/smali_classes2/com/hackosint/myapplication/
MainActivityKt.smali:    const-string v10, "Mikee"
```

A promising smali file was found. Smali files are a human-readable representation of the bytecode used in Android applications. Examining the surroundings of the match shows that a chat log is written directly.

```smali
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

Since the chat log is short, you can read it as is or pass it to ChatGPT to convert it into a more readable format.

| ID | user  | nickname  | message                                                                                        | time  |
|----|-------|-----------|------------------------------------------------------------------------------------------------|-------|
| 1  | user1 | G0lf-GTI  | Salut tout le monde, #A nous a fourni un nouveau moyen de communication temporaire : APT509TMP | 10:30 |
| 2  | user3 | Mikee     | Merci de l'information!                                                                        | 10:34 |
| 3  | user1 | Br4v0     | je venais à peine de trouver comment arriver ici et je dois déjà changer..                     | 10:35 |
| 4  | user3 | n0vember  | J'essaye de rejoindre au plus vite merci!                                                      | 10:37 |
| 5  | user3 | n0vember  | T'as pu rejoindre grâce à une super guide :)                                                   | 10:42 |

It seems that the communication channel is named APT-509TMP. This appears to be a Telegram group name, and indeed t.me/APT509TMP is accessible.

![](/images/hack-osint-2025-android/apt509tmp.png)

The first Telegram message was posted at 2:51 on April 26.

![](/images/hack-osint-2025-android/telegram.png)

While our team was in Japan (UTC+9), since this is a French CTF, it is more appropriate to consider the local time (UTC+2).

flag: `25/04/2025`
