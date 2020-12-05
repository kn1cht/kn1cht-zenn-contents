---
title: "学振特別研究員申請のための科研費LaTeX Tips"
emoji: "🖋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - latex
  - tex
  - academic
published: true
---

この記事は、[TeX ＆ LaTeX Advent Calendar 2020](https://adventar.org/calendars/5111) 6日目の記事です。昨日の記事は、doraTeXさんの[帳票生成ツールとしての LaTeX の活用](https://doratex.hatenablog.jp/entry/20201205/1607102432)でした。

https://adventar.org/calendars/5111

-----

[日本学術振興会の特別研究員制度](https://www.jsps.go.jp/j-pd/pd_sin.html)（いわゆる学振）の申請書を **「科研費LaTeX」** で執筆する際に筆者が使ったTipsをまとめました。記事を上から下まで読んで真似していただいても、「こんなことできないかな？」と思った時に逆引きマニュアル的に使っていただいても結構です。

こちらの記事、2020年5月に筆者がDC1申請書を書き上げた後に半分くらい書き進めていたものの、その後放置してしまっていたものです。Advent Calendarのタイミングでなんとか完成させました。来年からの申請で、参考にしてくださる方がいらっしゃれば幸いです。

## 科研費LaTeXとは
学振特別研究員（DC1・DC2・PDなど）の申請には、所定の様式を使って作成した**申請書内容ファイル**（以下、申請書）が必要です。公式から配布されているものはWordとPDFだけですが、サポート対象外という注記付きで、LaTeX版の様式も公式に紹介されています。それが、本記事で扱う「[**科研費LaTeX**](http://osksn2.hep.sci.osaka-u.ac.jp/~taku/kakenhiLaTeX/)」です。「科研費」という名前ではあるものの、特別研究員と科学研究費両方の申請で使えるテンプレートが配布されています。

http://osksn2.hep.sci.osaka-u.ac.jp/~taku/kakenhiLaTeX/

Advent Calendar 5日目の記事では「[既存PDFを下敷きにして上から文字を貼り込む](https://doratex.hatenablog.jp/entry/20201205/1607102432#%E6%97%A2%E5%AD%98PDF%E3%82%92%E4%B8%8B%E6%95%B7%E3%81%8D%E3%81%AB%E3%81%97%E3%81%A6%E4%B8%8A%E3%81%8B%E3%82%89%E6%96%87%E5%AD%97%E3%82%92%E8%B2%BC%E3%82%8A%E8%BE%BC%E3%82%80)」という帳票作成方法が紹介されていました。科研費LaTeXもこの方針で作られていて、**枠や注意書きの部分は学振の公式テンプレートと同じ見た目**です。所属機関の事務から見た目がおかしいと指摘されにくくなっています。

### 科研費LaTeXを使うとなぜ嬉しいか
特別研究員の様式は、**記入欄が全て枠で囲まれ、サイズなどを変更してはいけない**ことになっています。この枠は記入すべき内容に対してギリギリの大きさなので、多くの申請者は隙間なく文字や図を詰め込んで提出します。これをWordで作成しようとすると、枠のズレや画像の配置の微調整といった難しい作業が必要になります。

![筆者の申請書のとあるページ](https://storage.googleapis.com/zenn-user-upload/pmensoj80v1xbenehrbuzkergc78 =250x)
*筆者の申請書のとあるページ*

従って、科研費LaTeXを学振申請に使うメリットには以下が挙げられます。

1. **レイアウトに割く労力を軽減**し、内容に集中できる
1. **図表や参考文献を挿入・参照**するのにLaTeXの機能を使える
1. ソースファイルをGit管理すれば、添削を受けて**修正した部分をGitHubなどで簡単に差分として確認**できる


科研費LaTeXは、大阪大学の山中卓先生が[2006年から](http://www.kagami.org/diary/2006-10-16-1.html)継続してメンテナンスしてくださっているものです。最新の様式を快適に使えることに感謝しながら申請書を作成しましょう。

@[tweet](https://twitter.com/taku_ymnk/status/1234412367682752513)

### 科研費LaTeXの情報源
- [科研費LaTeX FAQ](http://osksn2.hep.sci.osaka-u.ac.jp/~taku/kakenhiLaTeX/faq.html)
    - 公式のよくある質問集。Tipsからエラー対応までかなり充実しています
- [科研費マクロ&LaTeX掲示板](http://www.cml-office.org/kakenhibbs/list.php)
    - 最新の様式で見つかった問題点が議論されていることがあります
    - 過去のログも有用なので、何か困ったら検索してみましょう

## この記事の趣旨
科研費LaTeXは、**学振申請書特有の様式に対応**するため様々な工夫がなされています。また、記入する側も**普段の論文でのLaTexとは違った書き方**をせざるを得ないケースが多いです。こうした背景から、筆者が自身のDC1申請書を準備するにあたって工夫した点をTipsとしてまとめました。

- DCの様式を基準に解説するので、PDなど他の科研費LaTeXと部分的に異なる可能性があります
- もしミスを見つけたら、ぜひご指摘ください

### 筆者の環境

- LaTeX:  TeXLive 2020
- Editor: Visual Studio Code

## 見出しを作ろうと思って`\subsection`, `\subsubsection`を書いても表示されない

### 現象

LaTeXの見出しコマンドを使い、文章の構造を整理したくなる人は多いことでしょう。

```latex:dc_03-04_preparation_etc.tex
\newcommand{\現在までの研究状況}{%
%begin  現在までの研究状況＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝
  \section{sectionだよ}

  ↑sectionだよ

  \subsection{subsectionだよ}

  ↑subsectionだよ

  \subsubsection{subsubsectionだよ}

  ↑subsubsectionだよ
```

これをコンパイルすると、次のようになります。なぜか**sectionとsubsectionが表示されず**、subsubsectionだけがPDFに反映されました。

![section,subsectionが表示されない](https://storage.googleapis.com/zenn-user-upload/jy3n626ypmy7z246b8tfjj8wwm73)
*section,subsectionが表示されない*

原因を探るためテンプレートを検索すると、`forms/preamble.tex`にこんな指定がありました。

```latex:forms/preamble.tex
% Dummy section and subsection commands.
% With these, some editors (such as TeXShop, etc.) can jump to the (sub)sections.
\newcommand{\dummy}{dummy}%
\renewcommand{\section}[1]{\renewcommand{\dummy}{#1}}
\renewcommand{\subsection}[1]{\renewcommand{\dummy}{#1}}
```

科研費LaTeXにおいては、`\section`と`\subsection`はエディタで**それぞれの欄にジャンプするためのダミー**となっているようです。この部分を書き換えてしまってもよいのですが、テンプレートそのものを変更するのには少し抵抗があります。また、山中先生自身も掲示板への投稿で「**欄の中での見かけを調節する目的でsectionを使うべきではない**」との見解を述べられています。

- [研究目的欄の中での見出し - 科研費マクロ&LaTeX掲示板](http://www.cml-office.org/kakenhibbs/msg.php?mid=451&form=tree)

### 解決策

そこで、`dc.tex`のプリアンブルに**自分で使う見出しコマンドを新たに定義してしまう**ことにします（[掲示板で紹介されていた方法です](http://www.cml-office.org/kakenhibbs/list.php)）。こうしておけば、見出しの見た目を一箇所で管理できます。
筆者は次のようにしました。

```latex:dc.tex
\usepackage{udline} % \ul command

\newcommand{\mysubsection}[1]{\noindent\subsection*{\textbf{\ul{#1}}}}
\newcommand{\mysubsubsection}[1]{\noindent\textbf{#1}}
```

中身をご覧になれば分かる通り、**インデントを消して太字や下線を付けただけ**ですので、残念ながら節番号が自動でつくことはありません。番号が必要であれば、以下のように自分で振るのが簡単です。

```latex:dc_03-04_preparation_etc.tex
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
*mysubsection,mysubsubsectionの表示*

もう1つの注意点は、このオレオレ見出しコマンドは内部的には**見出しではなくただの段落**ということです。従って、空行を入れなければ下の文章とくっついてしまいます。
ただ、この動作を悪用（？）して、どうしても欄に文章が入らない時に、**あえて見出しで改行せずに文章を始める**こともできます（乱用すると読みづらくなるので適度に行いましょう）。

```latex:dc_03-04_preparation_etc.tex
  \mysubsection{1. mysubsectionだよ}

  ↑mysubsectionだよ

  \mysubsection{2. mysubsectionだよ}  ←スペース節約版mysubsectionだよ
```

![あえて空行を入れずにスペース節約](https://storage.googleapis.com/zenn-user-upload/6pa5hlyfr080dc77a00rz4ha6q9o)
*あえて空行を入れずにスペース節約*

## 見出しに網掛けする

Wordで申請書を作る方に多いのが、**見出しを網掛けで強調する**やり方です。**グレースケールで印刷**され、欄も限られている学振書類では、色や余白を使わずに見出しを目立たせる方法として有効です。

LaTeXではズバリ網掛けをするコマンドはないので、自分で組み立てる必要があります。いくつか方法はあるようですが、ここではcolorパッケージに含まれる[`\colorbox`コマンドを使った方法](https://www.lightstone.co.jp/latex/betteruse/shading.pdf)を用います。先ほど作った`\mysubsection`を書き換えて、網掛けバージョンにしたものがこちらです。

```latex:dc.tex
\usepackage{color}

\newcommand{\mysubsection}[1]{\noindent\subsection*{\colorbox[gray]{0.8}{\textbf{#1}}}}
\newcommand{\mysubsubsection}[1]{\noindent\textbf{#1}}
```

![網掛けした見出し](https://storage.googleapis.com/zenn-user-upload/fss39kqmbsrzjhqpuo00mydtdgo4)
*網掛けした見出し*

見出しが一目で判別できるようになりました。

## 丸数字を使う

番号をつけて何かを列挙したい場合、通常は番号付き箇条書きを用います。ただし、学振申請書では枠に限りがあるため、仕方なく数字（①、②、......）を使って文章中で列挙をせざるを得ないこともあります。

[科研費LaTeX FAQ](http://osksn2.hep.sci.osaka-u.ac.jp/~taku/kakenhiLaTeX/faq.html)では`\textcircled`コマンドが紹介されています。これでも表示できるものの、数字が円の中心からズレてしまいます。

![標準の丸数字](https://storage.googleapis.com/zenn-user-upload/3c32jf1txun7ginhv8tov3fm6g5f)
*標準の丸数字*

従って、FAQの2番目に書かれているようにotfパッケージを読み込み、`\ajMaru`コマンドを使ったほうがよいでしょう。

```latex:dc.tex
\usepackage{otf}
```

![きれいな丸数字](https://storage.googleapis.com/zenn-user-upload/6om8gox24zpl0iukw1bg53jucuws)
*きれいな丸数字*

## 複数箇所の`\thebibliography`環境で文献番号を連続にする

学振申請書で関連研究に言及する場合は、論文と同様に**適切な引用**を書かなければなりません。
学振では欄が分かれているのでどのように引用を書くかの戦略は複数考えられます。
筆者は次のようにしました。

1. 文献を`[1], [2], ......`という番号で管理し、**最初に言及した欄の下部にまとめて記載**する
2. **文献番号はすべての欄で通し番号**にする（もし欄ごとに付けた場合、他の欄の文献に言及したいとき混乱がおきるため）

さて、LaTeXでは`\thebibliography`環境を使うと`\cite`コマンドで引用番号を自動で振ってくれるので、学振申請書でもぜひ使いたいですね。

```latex:dc_03-04_preparation_etc.tex
  そこで、\underline{地球で}最大の動物から、\underline{地上で}最大の動物~\cite{teramura}に研究対象を変更する。

  \begin{thebibliography}{99}
    \bibitem{teramura} 寺村輝夫、「ぼくは王様 - ぞうのたまごのたまごやき」.
  \end{thebibliography}

  これは2番目の文献を引用するための文~\cite{teramura2}。

  \begin{thebibliography}{99}
    \bibitem{teramura2} 寺村輝夫2、「ぼくは王様2 - ぞうのたまごのたまごやき」.
  \end{thebibliography}
%end  現在までの研究状況 ＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝
```

ところが、`\thebibliography`環境を複数記述すると、**毎回番号が1から始まってしまいます**。

![文献番号が毎回1から始まってしまう](https://storage.googleapis.com/zenn-user-upload/qoxt2fh9kqhuq6zmajpi6jyw7xaj)
*文献番号が毎回1から始まってしまう*

この問題を解決するためには、`mycc.sty`が有益です。
これは、「箇条書き環境のナンバリングを通し番号にしたい時に使う」スタイルファイルで、信州大学の沼田先生がご自身のWebサイトで公開されています。

http://math.shinshu-u.ac.jp/~nu/html/texmacros/mycc/

```latex:dc.tex
\usepackage{mycc}
```

あとは、`\thebibliography`環境を開いた後に`\begin{myenumiv}`、閉じる前に`\end{myenumiv}`を挿入すれば文献番号のカウンタを共有できます。

```latex:dc_03-04_preparation_etc.tex
  そこで、\underline{地球で}最大の動物から、\underline{地上で}最大の動物~\cite{teramura}に研究対象を変更する。

  \begin{thebibliography}{99}
    \begin{myenumiv}
      \bibitem{teramura} 寺村輝夫、「ぼくは王様 - ぞうのたまごのたまごやき」.
    \end{myenumiv}
  \end{thebibliography}

  これは2番目の文献を引用するための文~\cite{teramura2}。

  \begin{thebibliography}{99}
    \begin{myenumiv}
      \bibitem{teramura2} 寺村輝夫2、「ぼくは王様2 - ぞうのたまごのたまごやき」.
    \end{myenumiv}
  \end{thebibliography}
%end  現在までの研究状況 ＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝
```

![文献番号が重複しなくなった](https://storage.googleapis.com/zenn-user-upload/tim4oglv1k6nmcve5sl3yy8mhxsw)
*文献番号が重複しなくなった*

欄の数が増えてくると毎回環境を入れ子にするのは面倒ですから、`mythebibliography`のような環境を宣言して簡単に書けるようにするのもよいでしょう。

```latex:dc.tex
\newenvironment{mythebibliography}{%
  \begin{thebibliography}{99}%
  \begin{myenumiv}%
}{%
  \end{myenumiv}%
  \end{thebibliography}%
}
```

## `\thebibliography`環境のスペースを節約する

`\thebibliography`環境の前には、「参考文献」という文字が表示されます。学振申請書では少しの空きスペースも惜しいので、これは消してしまいましょう。方法は簡単で、`\refname`コマンドを空にするだけです。

@[tweet](https://twitter.com/ftake/status/80532576384724992)

```latex:dc.tex
\renewcommand{\refname}{}
```

## 自分の業績を参照するときは別の記号で参照する

申請書では、「**研究遂行能力**」欄で申請者のこれまでの主要な業績を並べることになります。こちらも番号を付けるわけですが、**関連研究と自分の業績とで記号を分けておく**と、万が一の誤読を防げます。

現在の科研費LaTeXには、研究業績を参照するための独自コマンド`\KLcite`が実装されています。素の状態では`\cite`コマンド同様`[番号]`で参照されます。そこで、[こちらのブログ](https://takehikom.hateblo.jp/entry/20131010/1381351482)を参考に`\KLcite`を再定義して**好きな記号で業績を参照できるように**します。

https://takehikom.hateblo.jp/entry/20131010/1381351482

```latex:dc.tex
\renewcommand{\KLcite}[1]{$^{\ref{#1})}$\,}
\newcommand{\KLciteii}[2]{$^{\ref{#1},~\ref{#2})}$\,}
\newcommand{\KLciteiii}[3]{$^{\ref{#1},~\ref{#2},~\ref{#3})}$\,}
```

これで、業績の方は上付きの`番号)`で参照されるようになりました（上付きはただの好みで、なくてもよいです）。

`\KLciteii`、`\KLciteiii`はそれぞれ2つ・3つの業績を並べたい場合には必要です。掲示板で山中先生が[回答](http://www.cml-office.org/kakenhibbs/msg.php?mid=1423&form=tree)されている通り、`\KLcite`は1つの引数を`\ref`に渡して書式を変えるだけで、複数並べる機能を持たないためです。


## 業績一覧の番号つきリストの見た目を整える

業績一覧向けには、`\KLbibitem`というコマンドが用意されています。しかし、このコマンドは`itemize`環境や`enumerate`環境における`\item`コマンドの代わりにはなりません。**サンプルファイルに使用例が含まれていない**ことからも、最近のテンプレートでは`\KLbibitem`を用いることが想定されていないように思われます。

そこで、業績一覧の見た目を整えるために`\KLbibitem`を改造して使いましょう。下の例では、コマンドの実装は`forms/kakenhi6.sty`からコピーし、5行目だけを変更しています。

```latex:dc.tex
\makeatletter
  \renewcommand{\KLbibitem}[1]{%
    \stepcounter{KLBibCounter}%
    \let \@currentlabel \theKLBibCounter
    \item[\arabic{KLBibCounter})]\label{#1}%
  }
\makeatother
```

このコマンドでは、`\item`で文献番号（`KLBibCounter`）を表示させるとともに、引数を`\label`に渡して`\KLcite`からの参照を可能にします。
ラベルの書式は、前述の業績番号書式と合わせるため、`番号)`のようになるよう指定しました。

使う際には、`\item`を`\KLbibitem{ラベル}`に置き換えるだけです。独自のカウンタを使っているので、**mycc.styで工夫しなくとも複数の箇条書きの間で通し番号になる**のも嬉しいところです。

```latex:dc_08_publications.tex
\subsection{学術雑誌等又は商業誌における解説・総説}
\newcommand{\学術雑誌等または商業誌における解説や総説}{%
%begin  学術雑誌等または商業誌における解説や総説＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝
  \begin{enumerate}
    \KLbibitem{yukawa2003} R.~Kipling, \underline{H. Yukawa},
        ``The Elephant's Child (象の鼻はなぜ長い)'',
        Nature, {\bf 999}, 777-779, (2003).
  \end{enumerate}
  他２件
%end  学術雑誌等または商業誌における解説や総説 ＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝
}
```

![サンプルの「研究遂行能力」欄で使った例](https://storage.googleapis.com/zenn-user-upload/4vomzj2tym96g4uetcmcout6c3yh =400x)
*サンプルの「研究遂行能力」欄で使った例*

## 業績一覧の空きスペースをなるべく詰める

ただし、デフォルトの箇条書き環境は少し隙間が多いです。科研費LaTeXについてきたサンプルでも欄からあふれてしまっています。箇条書き環境の見た目を柔軟に設定できる[`enumitem`パッケージを使ってレイアウトを調整](https://konoyonohana.blog.fc2.com/blog-entry-58.html)しましょう。

http://konoyonohana.blog.fc2.com/blog-entry-58.html

```latex:dc.tex
\usepackage{enumitem}
```

業績一覧以外で`enumerate`環境を使わないなら、`\setlist`コマンドで全体の書式を変更できます。筆者は次のように設定しました。**上・左の余白をなるべく詰める**とともに、**アイテム同士の余白を0に**設定しています。

```latex:dc.tex
\setlist[enumerate]{leftmargin=5truemm,noitemsep,topsep=3pt}
```

![enumerate環境の書式を調整した例](https://storage.googleapis.com/zenn-user-upload/209lf1keufxi6z6265g9xmgb1dit =400x)
*`enumerate`環境の書式を調整した例*

たくさん業績が入りそうです。

なお、もし全ての`enumerate`環境の書式が変わると困るという場合は、業績一覧では`description`環境を使うようにすることで回避できます。`description`環境は標準ではラベルが太字になるため、嫌な場合は`font=\normalfont`を追加で指定してください。

```latex:dc.tex
\setlist[description]{font=\normalfont,leftmargin=5truemm,noitemsep,topsep=3pt}
```

## PDFを自動でグレースケールに変換する

学振の申請書は（そして科研費の書類も）**審査時にはグレースケール印刷されます**。従って、図表がグレースケールでも読み取れるように工夫して申請書を仕上げることが大切です。

グレースケールでの見た目を確認するには、PDFビューワでグレースケールに変換したり、実際にグレースケールで印刷したりといった方法も当然あります。しかし、**LaTeXでコンパイルしたPDFをグレースケールに自動で変換**すれば、これらの作業が不要になります。

ここでは、latexmkの**コールバック機能でGhostscriptを呼び出し**てグレースケールに変換する方法をご紹介します。latexmkは、`.latexmkrc`に`$success_cmd = 'コマンド';`などと書くことで、コンパイル成功後に追加のコマンドを実行してくれます（失敗したときに実行される`$failure_cmd`等もあります）。

https://tex.stackexchange.com/questions/291204/latexmk-how-to-use-compiling-cmd-success-cmd-failure-cmd

```perl:.latexmkrc
$success_cmd  = "gs -q -r600 -dNOPAUSE -sDEVICE=pdfwrite -o %A_gray.pdf
                 -dPDFSETTINGS=/prepress -dOverrideICC -sProcessColorModel=DeviceGray
                 -sColorConversionStrategy=Gray -sColorConversionStrategyForImage=Gray
                 -dGrayImageResolution=600 -dMonoImageResolution=600
                 -dColorImageResolution=600 %D";
```

:::message alert
macOSやLinuxなどのGhostscriptのコマンドは`gs`なのに対し、WindowsでTeX Liveを使っている場合は`rungs`です。ご自身の環境に合わせてコマンド名を調整してください。
:::

これで、例えば`dc.tex`をコンパイルして`dc.pdf`が作られた後に、`dc_gray.pdf`にグレースケール版が作られます。提出するときも、このファイルを使えば間違いないですね。

![グレースケールのPDFができた](https://storage.googleapis.com/zenn-user-upload/8o7dboy00ht9aova4jtlyz6u0nnh)
*グレースケールのPDFができた*

-----

明日のTeX ＆ LaTeX Advent Calendar 2020は、CareleSmith9さんのmathabxパッケージの記事です。
