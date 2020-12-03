---
title: "学振特別研究員申請のための科研費LaTeX Tips"
emoji: "🖋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - latex
  - tex
  - academic
published: false
---

この記事は，[日本学術振興会の特別研究員制度](https://www.jsps.go.jp/j-pd/pd_sin.html)（いわゆる学振）の申請書を科研費LaTeXで準備する際のTipsをまとめたものです．
筆者の申請を準備する中で調べたTipsですので，網羅的なまとめではないことにご注意ください．
記事を上から下まで読んで真似していただいても，「こんなことできないかな？」と思った時に逆引きマニュアル的に使っていただいても結構です．

## はじめに
### 科研費LaTeXとは
学振の特別研究員（DC1・DC2・PD）の申請には，所定の様式を使って作成した**申請書内容ファイル**が必要です．
公式から配布されているものはWordとPDFだけですが，サポート対象外という注記付きで「LaTeX形式の様式」も公式に紹介されています．
それが，本記事で扱う「[**科研費LaTeX**](http://osksn2.hep.sci.osaka-u.ac.jp/~taku/kakenhiLaTeX/)」です．

科研費という名前になっていますが，特別研究員と科学研究費療法の申請に使えるテンプレートです．
特に特別研究員の様式は枠で囲まれている形式であるため，Wordで直面する**枠のズレなどのストレスをLaTeXでは回避できます**．
また，**図表や参考文献を挿入・参照するのにLaTeXの機能を使える**のも，論文執筆等で慣れている方には大きなメリットです．
さらに，ソースファイルをGit管理すれば，添削を受けて修正した部分をGitHubなどで簡単に差分として確認できます．

科研費LaTeXは，大阪大学の山中卓先生が[2006年から](http://www.kagami.org/diary/2006-10-16-1.html)継続してメンテナンスしてくださっているものです．
最新の申請書の様式を快適に使えることに感謝しながら申請書を作成しましょう．

https://twitter.com/taku_ymnk/status/1234412367682752513?s=20

### 科研費LaTeXの情報源
- [科研費LaTeX FAQ](http://osksn2.hep.sci.osaka-u.ac.jp/~taku/kakenhiLaTeX/faq.html)
    - 公式のよくある質問集．Tipsからエラー対応までかなり充実しています
- [科研費マクロ&LaTeX掲示板](http://www.cml-office.org/kakenhibbs/list.php)
    - 最新の様式で見つかった問題点が議論されているかもしれません
    - 過去のログも有用なので検索してみましょう

### この記事の趣旨
科研費LaTeXは，**学振申請書特有の様式に対応**するために様々な工夫がなされています．
また，記入する側も**普通の論文とは違った書き方**をセざるを得ないケースが多いです．
こうした背景から，筆者が自身のDC1申請書を準備するにあたって工夫した点をTipsとしてまとめました．

- DCの様式を基準に解説するので，PDなど他の科研費LaTeXと異なる部分がある可能性があります
- もしミスを見つけたら，ぜひご指摘ください

### 筆者の環境

- OS:     Windows 10 Version 1903
- LaTeX:  TeXLive 2020
- Editor: Visual Studio Code

## デザイン

### 見出しを作ろうと思ってsubsection, subsubsectionを書いても表示されない

#### 現象

LaTeXの見出しコマンドを使い，文章の構造を整理したくなる人は多いと思います．

```latex:dc.tex
\newcommand{\現在までの研究状況}{%
%begin  現在までの研究状況＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝
	\section{sectionだよ}

	↑sectionだよ

	\subsection{subsectionだよ}

	↑subsectionだよ

	\subsubsection{subsubsectionだよ}

	↑subsubsectionだよ
```

これをコンパイルすると，次のようになります．
なぜか**sectionとsubsectionが表示されず**，subsubsectionだけがPDFに反映されました．

![section,subsectionが表示されない](https://storage.googleapis.com/zenn-user-upload/jy3n626ypmy7z246b8tfjj8wwm73)

原因を探るためテンプレートを検索すると，`forms/preamble.tex`にこんな指定がありました．

```latex:forms/preamble.tex
% Dummy section and subsection commands.
% With these, some editors (such as TeXShop, etc.) can jump to the (sub)sections.
\newcommand{\dummy}{dummy}% 
\renewcommand{\section}[1]{\renewcommand{\dummy}{#1}}
\renewcommand{\subsection}[1]{\renewcommand{\dummy}{#1}}
```

科研費LaTeXにおいては，`\section`と`\subsection`はエディタで**それぞれの欄にジャンプするためのダミー**となっているようです．
この部分を書き換えてしまってもよいのですが，テンプレートそのものを変更するのには少し抵抗があります．
また，山中先生自身も掲示板への投稿で「**欄の中での見かけを調節する目的でsectionを使うべきではない**」との見解を述べられています．

- [研究目的欄の中での見出し - 科研費マクロ&LaTeX掲示板](http://www.cml-office.org/kakenhibbs/msg.php?mid=451&form=tree)

#### 解決策

そこで，`dc.tex`のプリアンブルに**自分で使う見出しコマンドを新たに定義してしまう**ことにします（[掲示板で紹介されていた方法です](http://www.cml-office.org/kakenhibbs/list.php)）．
こうしておけば，見出しの見た目を一箇所で管理できます．
筆者は次のようにしました．

```latex:dc.tex
\usepackage{udline} % \ul command

\newcommand{\mysubsection}[1]{\noindent\subsection*{\textbf{\ul{#1}}}}
\newcommand{\mysubsubsection}[1]{\noindent\textbf{#1}}
```

中身をご覧になれば分かる通り，**インデントを消して太字や下線を付けただけ**ですので，節番号が自動でつくことはありません．
番号が必要であれば，以下のように自分で振りましょう．

```latex:dc.tex
\newcommand{\現在までの研究状況}{%
%begin  現在までの研究状況＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝
	\mysubsection{1. mysubsectionだよ}

	↑mysubsectionだよ

	\mysubsubsection{1.1. mysubsubsectionだよ}

	↑mysubsubsectionだよ

	\mysubsection{2. mysubsectionだよ}

	↑mysubsectionだよ

	\mysubsubsection{2.1. mysubsubsectionだよ}

	↑mysubsubsectionだよ
```

![mysubsection,mysubsubsectionの表示](https://storage.googleapis.com/zenn-user-upload/bmaggcnbrdnc3hl9udqz5z69nugs)

もう1つの注意点は，このオレオレ見出しコマンドは内部的には**見出しではなくただの段落**ということです．
従って，空行を入れなければ下の文章とくっついてしまいます．
ただ，この動作を悪用（？）して，どうしても欄に文章が入らない時に，**あえて見出しで改行せずに文章を始める**こともできます（乱用すると読みづらくなるので適度に行いましょう）．

```latex:dc.tex
	\mysubsection{1. mysubsectionだよ}

	↑mysubsectionだよ

	\mysubsection{2. mysubsectionだよ}	←スペース節約版mysubsectionだよ
```

![あえて空行を入れずにスペース節約](https://storage.googleapis.com/zenn-user-upload/6pa5hlyfr080dc77a00rz4ha6q9o)


### 見出しに網掛けしたい

Wordで申請書を作る方に多いのが，**見出しを網掛けで強調する**やり方です．
グレースケールで印刷され，欄も限られている学振書類では，色や余白を使わずに見出しを目立たせる方法として有効です．

LaTeXで網掛けをするには，コマンドを自作する必要があります．
いくつか方法はあるようですが，ここではcolorパッケージに含まれる[`\colorbox`コマンドを使った方法](https://www.lightstone.co.jp/latex/betteruse/shading.pdf)を用います．
先ほど作った`\mysubsection`を書き換えて，網掛けバージョンにしたものがこちらです．

```latex:dc.tex
\usepackage{color}

\newcommand{\mysubsection}[1]{\noindent\subsection*{\colorbox[gray]{0.8}{\textbf{#1}}}}
\newcommand{\mysubsubsection}[1]{\noindent\textbf{#1}}
```

![網掛けした見出し](https://storage.googleapis.com/zenn-user-upload/fss39kqmbsrzjhqpuo00mydtdgo4)

見出しが一目で判別できるようになりました．

### 丸数字を使いたい

[科研費LaTeX FAQ](http://osksn2.hep.sci.osaka-u.ac.jp/~taku/kakenhiLaTeX/faq.html)では`\textcircled`コマンドが紹介されています．
これでも表示できるものの，数字が円の中心からズレてしまいます．

![標準の丸数字](https://storage.googleapis.com/zenn-user-upload/3c32jf1txun7ginhv8tov3fm6g5f)

そこで，[LaTeXできれいな丸囲み文字を表示する方法 - 惰力飛行](https://livingdead0812.hatenablog.com/entry/20161005/1475654232)を参考に，文字を小さくした`\ctext`コマンドを用意して使用すればきれいな丸数字になります．

```latex:dc.tex
\newcommand{\ctext}[1]{\textcircled{\scriptsize{#1}}}
```

元記事で使っている`\raise`コマンドは，科研費LaTeXではエラーが出たため外しました．
これでも違和感はないと思います．

![きれいな丸数字](https://storage.googleapis.com/zenn-user-upload/2a8f6cwq8yqm8zcp3c1aqlf4uj1e)

### 複数ページにまたがる記入欄で，1行でも多く前のページに入れたい
孤立
![image](https://tex.stackexchange.com/questions/472316/always-allow-orphans-and-widows Always allow orphans and widows - TeX - LaTeX Stack Exchange)

## 参考文献と業績
### 複数箇所のthebibliographyで文献番号を連続にしたい
  - \usepackage{mycc}
  - \renewcommand{\refname}{} % \thebibliography で「参考文献」を消す https://twitter.com/ftake/status/80532576384724992
  - \let\OLDthebibliography\thebibliography
  - \renewcommand\thebibliography[1]{
      - \OLDthebibliography{#1}
      - \setlength{\parskip}{0pt}
      - \setlength{\itemsep}{0pt plus 0.3ex}
  - }

### 自分の業績を参照するときは別の記号にしたい

業績
[http://www.cml-office.org/kakenhibbs/msg.php?mid=1422&form=tree 科研費マクロ&LaTeX掲示板]
[http://www.cml-office.org/kakenhibbs/showthread.php?mid=1343&form=tree 科研費マクロ&LaTeX掲示板]
[http://konoyonohana.blog.fc2.com/blog-entry-58.html 天地有情 [LaTeX] enumitem --- リスト環境のレイアウトを制御]

## PDF出力
### PDFを自動でグレースケールにしたい
