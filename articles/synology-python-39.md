---
title: "SynologyのNASに公式の手段でPython3.9をインストールしてpipも使えるようにする"
emoji: "🔧"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - python
  - nas
published: true
---

# TL;DR
- 現在のSynology NASでは標準機能でPython3.9をインストールできます
- pipを使うには、ensurepipパッケージでpipをインストールする、またはvenvで仮想環境を作成するとよいです

# 背景
Synology製のNAS（Network Attached Storage）では、様々な自作のプログラムを動かすことができます。特に、Pythonを使うことでWebサイトの監視、IoT、バックアップなど幅広い処理が可能です。筆者は、GitHubの自分のリポジトリを自動でNASに反映させるプログラムを定期実行しています。

https://zenn.dev/kn1cht/articles/github-backup-nas

以前から、Pythonをパッケージセンター（標準のパッケージマネージャ）から導入できましたが、バージョンが古めであるため**外部のパッケージマネージャやDockerを経由させる事例**が多く見られました。

2022年ごろから、パッケージマネージャから標準で利用可能なパッケージに**Python 3.9**が追加されました。2023年現在、ベータの表示も消え安心して使えるようになっています。ただ、そのままの状態では`pip`コマンドが使えないなど、快適に使うには少しだけ工夫が必要です。
そこで、本記事では標準のPython 3.9でpipコマンドを使えるようにする方法を解説します。

![Python 3.9パッケージのインストール画面](/images/synology-python-39/packagecenter.png)

## 環境
- Synology DS218
- DSM (Synology NASのOS) `7.2-64570`
- Python 3.9パッケージ `3.9.14-0010`
- 事前準備として、SSHでログインしてコマンドを打てる状態にしておきます
    - [SSH 経由で root 権限により DSM/SRM にサインインする方法は？ - Synology ナレッジセンター](https://kb.synology.com/ja-jp/DSM/tutorial/How_to_login_to_DSM_with_root_permission_via_SSH_Telnet)


## Pythonのインストール
パッケージセンターを開き、Python 3.9を一覧から探すか検索してインストールします。

![Python 3.9パッケージのインストール画面](/images/synology-python-39/packagecenter.png)

SSHでログインし、`python3.9`を実行して使用できることを確認します。

```bash
$ python3.9
Python 3.9.14 (main, Mar 24 2023, 10:05:06)
[GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
```

# pipを使えるようにする
一方で、`pip`などと入力してもコマンドが見つからないと表示されるはずです。これでは、Pythonパッケージをインストールできないので、pipを導入していきます。


[Web Stationを使ってモジュールをインストールする方法もある](https://ysand.myds.me/2022/09/11/synology-dsm7-python-pip/)ようですが、GUIでの作業が多く大変なので、本記事ではコマンドで完結する方法2つをご紹介します。

# pipをそのまま入れる
参考にしたページ：[How to install Python pip on Synology NAS - Sindastra's info dump](https://www.sindastra.de/p/2290/how-to-install-python-pip-on-synology-nas)

https://www.sindastra.de/p/2290/how-to-install-python-pip-on-synology-nas

Python3.4から用意されている`ensurepip`モジュールを使用する方法です。

```bash
$ sudo python3.9 -m ensurepip --upgrade
$ sudo python3.9 -m pip install --upgrade pip
```

インストールが終わると、インストール先のディレクトリ（筆者の環境では`/var/packages/Python3.9/target/usr/bin`）と、PATHに追加するように促すメッセージが表示されます。PATHに反映させると、`pip3.9`などのコマンドが有効になります。

```bash
$ echo export PATH='/var/packages/Python3.9/target/usr/bin:$PATH' >> ~/.bashrc
$ source ~/.bashrc
```

（zshなど別のシェルをお使いの方は`~/.zshrc`などに読み替えてください）

あとは、好きなようにパッケージを入れればOKです。

```bash
$ sudo pip3.9 install numpy
```

# venvで仮想環境を作成する
参考にしたページ：[Synology NASに Python 仮想環境をセットアップするにはどうすればよいですか？ - Synology ナレッジセンター](https://kb.synology.com/ja-jp/DSM/tutorial/Set_up_Python_virtual_environment_on_NAS)

https://kb.synology.com/ja-jp/DSM/tutorial/Set_up_Python_virtual_environment_on_NAS

上の方法でpipを使っていると、「rootで使うのは危険だから仮想環境を使いなよ」と警告されます。そこで、今度は最近のPythonに組み込まれている仮想環境`venv`を使用してpipを使ってみます。

```bash
$ cd # 仮想環境のためのファイル群を設置したいディレクトリに移動
$ python3.9 -m venv default
$ source default/bin/activate
```

これで仮想環境が有効になり、pipも`{環境名}/bin`以下に用意されます。この方法の場合、sudoは不要です。

```bash
$ pip3.9 install numpy
```

[タスクスケジューラー](https://kb.synology.com/ja-jp/DSM/help/DSM/AdminCenter/system_taskscheduler?version=7)で自動的に呼び出す場合、`source /{仮想環境のディレクトリまでのパス}/bin/activate`を先に実行しておかないとエラーになるので注意しましょう。
