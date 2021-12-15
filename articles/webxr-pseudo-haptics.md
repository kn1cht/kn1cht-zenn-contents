---
title: "触覚錯覚体験デモ WebXR Pseudo-hapticsを作ってみてのTips"
emoji: "🔊"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - vr
  - webxr
  - haptics
published: false
---

:::message
本記事は、[WexXR Advent Calendar 2021](https://adventar.org/calendars/6556) 16日目の記事です。
昨日の記事は、Hironoriさんでした。
明日はきれいなツチノコさんの「初心者がWebXR+Emscriptenでドはまりした話」です。
:::

# WebXR Pseudo-haptics
視覚刺激によって起こる触覚の錯覚Pseudo-hapticsのオンラインデモ「WebXR Pseudo-haptics」を公開しました。
**https://git.io/WEBXRPH** から体験できます。

https://www.youtube.com/watch?v=PnGpjr6gyj0

Pseudo-haptics（疑似触覚）は**、ユーザの入力と視覚的な表示のずれ**によって生起する触覚の錯覚です。 例えば、マウスカーソルの動きが普段より遅いと、抵抗力が大きくなったかのように感じる現象はpseudo-hapticsの一種です。
物理的な触覚提示デバイスがなくても、ユーザに重さや摩擦、凹凸などを知覚させることができます。

![](https://storage.googleapis.com/zenn-user-upload/9be32e07a048-20211215.gif)

WebXR Pseudo-hapticsは、**ブラウザで動作するpseudo-hapticsのデモンストレーション**です。 2Dモニタでの体験に加えて、ヘッドマウントディスプレイ（HMD）を用いてバーチャル空間でもpseudo-hapticsを体験できます。

制作のモチベーションについてさらに詳しいことは[触覚のオンラインデモを作る試み](https://scrapbox.io/kn1cht/%E8%A7%A6%E8%A6%9A%E3%81%AE%E3%82%AA%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%83%87%E3%83%A2%E3%82%92%E4%BD%9C%E3%82%8B%E8%A9%A6%E3%81%BF)という記事に別途まとめたので、本記事ではWebXR Pseudo-hapticsの実装や公開のTipsについてご紹介します。
