---
title: "GitHubの自分の全リポジトリをNASに自動バックアップする"
emoji: "💾"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - git
  - github
  - nas
  - 自動化
published: true
---

# TL;DR
- GitHubに保管しているリポジトリを、自宅NASに自動で同期するバックアップシステムを作りました
- NAS（Synology製）にGitとPythonをインストールし、NASだけでバックアップのためのスクリプトを定期実行します

![バックアップされたリポジトリたち](/images/github-backup-nas/main.png)

書いたコードはこちらのリポジトリにあります。

https://github.com/kn1cht/backup_github

# GitHubに置いたデータをローカルでも保管したい
クラウド全盛の時代に逆行するようですが、**GitHubの自分の全リポジトリをローカルにバックアップ**したくなりました。
理由はいくつかあります。

1. データがクラウドだけにある状態は不安
1. GitHubが落ちることもたまにはある
1. PCのローカルリポジトリがバックアップとして万全とは限らない

## ① データがクラウドだけにある状態は不安
人によってデータに関する考え方は違うと思いますが、筆者自身は「**自分が持つデータは全てローカルストレージに置いておきたい**」派です。クラウドも便利に活用した上で、必ず同じデータを自室のPCやNAS、外付けHDDにも保管するようにしています。

もちろんクラウドストレージは便利で、データの保護についても個人宅よりも大企業のデータセンターのほうが上でしょう。しかし、クラウドストレージには検閲やアカウント凍結のリスクがつきまといます。

例えば、Google Driveに保管したファイルが誤って「不適切だ」と判断されGoogleアカウントが凍結されてしまった事例があります。

https://gigazine.net/news/20210924-google-lock-account/

https://hitoxu.com/027441

また、近年発達している音楽・動画配信サービスでも、アーティストが逮捕されてしまうと、過去の楽曲まで配信停止されて突然聴けなくなることが繰り返されています。

https://www.itmedia.co.jp/business/articles/1907/11/news040_2.html

GitHubに保管されているものはソースコードがメインですから、違法なコンテンツと見なされることはそれほどないかもしれません。ただ、2020年には動画ダウンロードライブラリyoutube-dlが、**著作権侵害とされて一時削除される**という騒動が起きました。
清く正しくGitHubを使っていると方であろうと、自分のリポジトリが突然消される心配が絶対にないとは言えないのではないでしょうか。

https://www.itmedia.co.jp/news/articles/2011/17/news070.html

## ② GitHubが落ちることもたまにはある
[GitHubの障害履歴を見ると](https://www.githubstatus.com/history)、月に数回は小規模な障害が発生しているのが分かります。GitHubで何かがエラーになっているとTwitterが阿鼻叫喚になり、トレンド入りしてしまうのももはや恒例です。

GitHubが全く使えないほどの大きな障害は大変まれではあるものの、万が一必要なときリポジトリにアクセスできなかったら困ってしまいます。

## ③ PCのローカルリポジトリがバックアップとして万全とは限らない
そもそも、GitHubは分散型バージョン管理ですから、ローカルリポジトリが手元にあればリモート環境がなくても開発できるのが強みです。ただし、バックアップとして考えると万全とは限りません。

ローカルリポジトリを常にリモートと同じに保っておくためには、各リポジトリで定期的に`git pull`し続けなければなりません。さらに、開発用PCであれば、そのリポジトリを編集していてリモートと異なる状態になることもあるでしょう。**常にGitHubのリモートリポジトリと同じ状態のデータを保管したい用途には、PCのローカルリポジトリは不向き**だと考えられます。

# GitHubの自分の全リポジトリをNASに自動バックアップする
ここまで述べた問題を解決するために、**GitHubのリポジトリをNASに自動同期する**システムを作りました。
同期先はNASでなくてもOKですが、筆者はPCにある開発用リポジトリとは場所を分けたかったためNASを対象としました。
[公開しているコード](https://github.com/kn1cht/backup_github)自体はNASでなくても使えるはずです。

## ハードウェア環境
- Synology DS218
    - Synology社の2ベイNASです
    - 自作のプログラムを簡単に動かせるNASとしては他にQNAP社のものもありますが、本記事の説明はSynologyのNASのものに限定します

![NASの外観](/images/github-backup-nas/ds218.jpg =400x)
*NASの外観*

## ソフトウェア環境
- DSM (Synology NASのOS) `7.0.1`
- [Git Server](https://kb.synology.com/ja-jp/DSM/help/Git/git?version=6) `2.33.0-1016`
    - 公式で用意されているGitパッケージです
    - 主な用途はNASにGitサーバーを建てることですが、今回はクライアントとして使います
- Git LFS `3.1.2`
- [Z shell](https://synocommunity.com/package/zsh) `v5.8-11`
    - Synology NASのオープンソースコミュニティ"SynoCommunity"が配布する`zsh`です
    - CLIでの作業を楽にするために使いました
- Python `3.9.5`

# Synology NASでのCLI開発環境構築
SynologyのNASはLinuxをベースにした独自OSです。パソコンではなくNASなのでGUI側でやれることには制約があります。ただし、**SSHで直接コマンドを叩けるように設定**してしまえば一気にカスタマイズの幅が広がります。

:::message alert
NASでの自作プログラム実行や非公式パッケージの導入は自己責任で慎重に実施してください。誤った操作をすると、システムが破損したり、大事なデータが消えてしまったりと重大な影響が出る場合があります。
SSH経由でroot権限を使っての作業には特に気をつけてください。
:::

## SSHの有効化
「コントロール パネル」→「端末とSNMP」でSSHを有効化します。ポートはデフォルトの22以外に設定することが推奨されています。

![SSH設定画面](/images/github-backup-nas/ssh-config.png)

`ssh {ユーザ名}@{ホスト名 or IPアドレス} -p 22222` のように`ssh`コマンドを実行し、ログインパスワードを入力すればSSHでログインできます。

![SSH接続した様子](/images/github-backup-nas/ssh-shell.png)

デフォルトシェルは`/bin/sh`でした。あまり使い勝手が良くないので、あとで`zsh`に変更します。

## Git（& Git LFS）のインストール
公式で配布されている[Git Server](https://kb.synology.com/ja-jp/DSM/help/Git/git?version=6)を「パッケージ センター」からインストールします。

![Gitをインストール](/images/github-backup-nas/git.png)

完了したら、SSH経由でコマンドを叩いてGitが使えることを確認します。

```sh
$ git --version
git version 2.33.0
```

もし**Git Large File Storage（LFS）** を使っているリポジトリがある場合、標準のGitだけではLFS管理下のファイルを取得できません。Git LFSもインストールしましょう。
Synology NASで使えるパッケージマネージャでGit LFSを見つけられなかったため、[GitHubのリリースページ](https://github.com/git-lfs/git-lfs/releases/)からビルド済みバイナリを取得して使うことにします。

Git LFSのLinux向けバイナリは"Linux 386（32ビットCPU向け）"、"Linux AMD64（64ビットCPU向け）"、"Linux ARM（ARMの32ビットCPU向け）"などに分かれているため、NASに搭載されたCPUを調べて適合するものを使います。

https://kb.synology.com/ja-jp/DSM/tutorial/What_kind_of_CPU_does_my_NAS_have

DS218のCPUは`Realtek RTD1296`で、これは64bitのARMアーキテクチャであるため"Linux ARM64"をダウンロードしました（モデルによってCPUは結構違うため、Synologyのページで必ずご確認ください）。

```sh
$ wget https://github.com/git-lfs/git-lfs/releases/download/v3.1.2/git-lfs-linux-arm64-v3.1.2.tar.gz
$ echo "c6152c4e24e0575396ee80be8049bf258659fec552f81b410705beed25712ba0 git-lfs-linux-arm64-v3.1.2.tar.gz" | sha256sum -c
git-lfs-linux-arm64-v3.1.2.tar.gz: OK
$ tar -xvf git-lfs-linux-arm64-v3.1.2.tar.gz
$ ./install.sh
$ sudo ./install.sh
Git LFS initialized.
$ git lfs
git-lfs/3.1.2 (GitHub; linux arm64; go 1.17.6)
git lfs <command> [<args>]
```

`git lfs`が動けばOKです。

## zshのインストール
シェルの変更は必須でないので飛ばしてもOKですし、`bash`や`fish`などをお使いなら合わせてよいと思います。

https://3os.org/infrastructure/synology/Install-oh-my-zsh/

`zsh`はSynologyの公式ではなくコミュニティが提供するパッケージの中にあります。おおむね上の解説ページに沿って進めていきます。
「パッケージ センター」で「設定」の「パッケージ ソース」タブを開き、「追加」から`https://packages.synocommunity.com`を追加します。

![パッケージセンターでのソース追加](/images/github-backup-nas/package-synocommunity.png)

すると、パッケージセンターの左メニューに「**コミュニティ**」ボタンが出てくるので、クリックして`Z shell`を探しインストールします。
終わったらコマンドで入っているか確認します。

```sh
$ which zsh
/usr/local/bin/zsh
```

Synology NASにはログインシェルを変更する`chsh`コマンドが無いようなので、代わりに`~/.profile`に`zsh`を自動で起動する設定を入れます。

```sh:~/.profile
if [[ -x /usr/local/bin/zsh ]]; then
  export SHELL=/usr/local/bin/zsh
  exec /usr/local/bin/zsh
fi
```

筆者はメインPCから`.zshrc`や`.vimrc`などのdotfilesを移植して、そこそこ便利に使えるようにしています（わざわざNASで開発する機会は別に多くありませんが……）。

## Pythonのインストール
GitHubを巡回してリポジトリを自動同期するには、便利なPythonを使うことにします。
Synologyの公式パッケージにもPythonはあるものの、現在の**標準パッケージには古いPython 2しかありません**（今見たらベータパッケージとしてPython 3.9が増えていました！　これでもいいかもしれません）。

先駆者の方が「[SynologyNAS に最新バージョンの Python 3.8 を入れてみた ～だってパッケージセンターは 3.5 止まりなんだもの～](https://b8a4avtof30320dmspo.blogspot.com/2020/11/synologypython-35.html)」という記事を書かれていたので、筆者はこれに従って最新のPythonを入れることにしました。

https://b8a4avtof30320dmspo.blogspot.com/2020/11/synologypython-35.html

この記事では、Synology NAS用のパッケージマネージャEntware-ngを導入しています。Pythonに限らず様々なパッケージを管理できるので入れて損はありません。
`python3`、`pip3`が動いたら完了です。

# GitHubからリポジトリを自動clone or pullするプログラム
開発環境ができたので、いよいよ本筋のバックアッププログラムを作ります。[ソースコード全文はGitHubに置いてあります](https://github.com/kn1cht/backup_github)。
使用するには以下のパッケージが必要です。

- `GitPython` : GitをPythonから操作するパッケージ
- `PyGithub` : GitHubをPythonから操作するパッケージ
- `python-dotenv` : `.env`ファイルから設定を読み込むパッケージ。GitHubトークンなど機密情報をコードに書かずに済みます

## 自分のGitHubリポジトリのリストを取得する

https://qiita.com/yshr10ic/items/a416ba6fbea7637be552

既に解説記事があるように、`PyGithub`パッケージを使って`g.get_user().get_repos(type='owner')`のように実行すると、Github APIからリポジトリ一覧が得られます。
privateリポジトリも確認できるように、 [https://github.com/settings/tokens](https://github.com/settings/tokens)で"repo"スコープを付けたpersonal access tokenを生成しておきましょう。

クローンに必要な`repo.clone_url`だけをリストに抜き出しておきます。

```python:backup_github.py
###### prepare github client ######
github_scheme = f'https://{github_token}:x-oauth-basic@' # for cloning private repos
g = Github(github_token)

###### get urls from github (the user and organizations) ######
clone_urls = [repo.clone_url for repo in g.get_user().get_repos(type='owner')]
```

## OrganizationのGitHubリポジトリのリストを取得する
GitHubには、企業やOSSチーム単位でリポジトリを管理できるOrganization機能があります。そこで、Organizationで作ったリポジトリもローカルに同期できるようにします。
ユーザのリポジトリと同様に、`g.get_organization({Org名}).get_repos(type='owner')`でリポジトリ一覧を取得できます。

Organization用の設定を`repository_settings.json`に書き、**指定したOrganizationから正規表現のパターンに一致するリポジトリのURLを集める**コードを書きました。


```python:backup_github.py
with open(os.path.join(os.path.dirname(__file__), 'repository_settings.json')) as f:
    repos = json.load(f)

# （中略）

clone_urls_org = set()
for org in repos['orgs']:
    for repo in g.get_organization(org['name']).get_repos(type='owner'):
        if 'patterns' in org and len(org['patterns']) != 0:
            for pattern in org['patterns']:
                if re.search(pattern, repo.name) is not None:
                    clone_urls_org.add(repo.clone_url)
        else:
            clone_urls_org.add(repo.clone_url)

clone_urls += list(clone_urls_org)
```

## バックアップ先ディレクトリにリポジトリがまだない場合はgit cloneする
ここからは`GitPython`でNAS上のGitを操作していきます。
まず、GitHubのURLを加工して`{ドメイン名}/{ユーザ名 or Org名}/{リポジトリ名}`の形式にし、これをそのままバックアップ先のディレクトリ構成にします。これは、motemen氏のGitリポジトリ管理ソフト[`ghq`](https://github.com/x-motemen/ghq)のやり方を参考にしたものです。

```python:backup_github.py
###### perform git clone or git pull ######
for url in clone_urls:
    parsed_url = urllib.parse.urlparse(os.path.splitext(url)[0])
    repo_path = parsed_url.netloc + parsed_url.path # e.g. github.com/github/gitignore
```

これでclone先のパスを生成できたので、`git clone`をリポジトリごとに実行します。しかし、2回目以降で既存のディレクトリに`git clone`しようとするとGitがエラーを出し、Pythonも異常終了してしまいます。

従って、`os.path.exists()`でディレクトリがなかったときにだけcloneするようにしました。

```python:backup_github.py
###### perform git clone or git pull ######
for url in clone_urls:
    # （中略）
    if not os.path.exists(f'{clone_dir}{repo_path}'):
        try:
            git.Repo.clone_from(f'{github_scheme}{repo_path}', f'{clone_dir}{repo_path}', multi_options=['--recursive'])
            print(f'[{repo_path}] performed git clone')
        except git.exc.GitCommandError as e:
            # print(str(e))
            print(f'[{repo_path}] clone failed')
```

## 各リポジトリでgit pullする
`git pull`で各リポジトリを最新状態に保ちます。
次節で述べますが**実行後に標準出力をメールで受け取れる**ので、バックアップができているのを確認できるように新旧のコミットハッシュをprintします。

```python:backup_github.py
###### perform git clone or git pull ######
for url in clone_urls:
    # （中略）
    repo = git.Repo(f'{clone_dir}{repo_path}')
    repo.git.checkout('HEAD', '--force')
    info = repo.remote().pull()[0]
    if info.old_commit is not None:
        print(f'[{repo_path}] performed git pull: {info.old_commit}...{info.ref}')
```

# NAS上で定期実行する
プログラム定期実行の定番といえばcronですが、なんとSynology NASのOS（DSM）には**GUIで設定できるタスクスケジューラ**があります。
「コントロール パネル→タスク スケジューラ」を開き、「作成→予約タスク→ユーザー指定のスクリプト」と進みます。

![スケジュール設定画面](/images/github-backup-nas/task-create.png)

「スケジュール」タブで実行する時間を曜日・時刻で設定できます。筆者は毎日指定時刻に実行されるようにしています。

「タスク設定」タブでは、**実行するシェルスクリプトとメール通知**を設定します。Pythonやスクリプトの場所は絶対パスで書いてあげたほうが確実に動くと思います。

```sh
/opt/bin/python3 /{絶対パス}/backup_github/backup_github.py
```

Eメール通知にチェックを入れておくと、標準出力の内容が指定したメールアドレスに届いて便利です。

![タスク設定画面](/images/github-backup-nas/task-script.png)
*設定*

![届くメールの例](/images/github-backup-nas/task-email.png)
*届くメールの例*



# 現時点で対応していないこと
- GitHub Wikiのバックアップ
    - Wikiは別のリポジトリとしてgitから操作できます
    - Wikiになくしたくない情報がある方の場合は、Wikiのリポジトリも同様に自動バックアップする機能が必要になるでしょう
