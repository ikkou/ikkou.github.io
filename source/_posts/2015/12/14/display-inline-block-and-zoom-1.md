title: 'IE6 でも display: inline-block; を使いたかった僕たちは'
description: 'IE6 でも display: inline-block; を使いたかった僕たちは zoom: 1; を頼るしかなかった'
date: 2015-12-14 17:16:17
tags: [CSS, AdventCalendar]
---

これは「[CSS 昔話 Advent Calendar 2015](http://www.adventar.org/calendars/723)」14日目の記事です。
13日目は(´°ム°`)さんの「[昔はマウスオーバーで突如おかしくなることがあった君（IE6）](http://h2ham.net/css-ie6-bug)」です。

CSS 昔話というキーワードに脊椎反射で飛びついて登録しましたが、ブラウザと戯れることになったので IE6 の時代からなので、昔話と言えるのかどうか怪しいです。

**世界観は IE6 の全盛期、まだ IE7 が生まれていない頃です。**

さて、表題の件、IE6 でも display: inline-block; を使いたかった僕たちはどうしたか……。

<!-- more -->

## display: inline-block; が使えなかった IE6

「flexboxしかしらない若者」というのが実際に存在するのか定かではありませんが、仮に「爆発して欲しいブラウザ」というランキングがあれば間違いなく No.1 を取るであろう IE6 こと Internet Explorer 6 は、display: inline-block; が使えませんでした。

横並びにさせたいなら `float` を使えば良いのでは? という声もありますが…

- IE6 で `margin` が倍加するバグとの戦いを強いられる
- `float` の解除が必要になる
- `vertical-align: middle;` で垂直方向を中央に揃えたい

…といった諸事情から display: inline-block; を使う方策を練らなければなりませんでした。

## 僕らは hasLayout property を true にした

どうにかして IE6 で display: inline-block; を使いたかった僕たちは hasLayout property を true にすることで、その夢を叶えました。

そして hasLayout property を true する為に多用されたのが、今や使うことの無くなった zoom: 1; という指定でした。

## hasLayout property

hasLayout property とは、教科書通りの回答をするなら IE 独自のプロパティであり、要素がレイアウト情報を持っているかを示すものです。

true か false の真偽値を持っていて、初期値は false であり、false の場合はレイアウト情報を持たないということになります。

この hasLayout property は特定のプロパティを指定することで true になり、その中で最も影響の少ないプロパティが zoom: 1; でした。

## zoom property

zoom プロパティは hasLayout property 同様に IE 独自の拡張であり、要素を拡大表示する際に指定します。

つまり、zoom: 1; は等倍で表示するという当たり前の指定であり、その故に影響を出しにくいので hasLayout property を true にする為に多用されたのでした。

IE6 の仕様上、display:inline; 且つ hasLayout property が true の場合、それは置換要素となる為、display: inline-block; 「同等」の横並びを実現することが出来ました。

### 置換要素

HTML の要素は、ざっくり以下の3種に大別することが出来ます。

- 幅と高さを持ち、前後で改行が発生する「ブロック要素」
- 幅と高さを持たず、前後で改行が発生しない「インライン要素」
- 幅と高さを持ちながら、前後で改行が発生しない「置換要素」

この置換要素こそが display: inline-block; と同等の振る舞いである為、前述の指定に至ったわけです。

## アンダースコアハック

hasLayout property を true にすれば display: inline; になるんや！
そう気付いた僕らはまた別のことに気付きます。

display: inline-block; が使えるブラウザでは display: inline-block; を指定したい。

そうです、このままだと IE6 をどうにかする為だけの指定が他のブラウザにも影響してしまいます。
そんな状況を打開するため、僕らは「アンダースコアハック」を使いました。

「アンダースコアハック」とは、プロパティの前に `_` を記述する CSS ハックで、このハックが仕込まれたプロパティは、IE6 でのみ動作します。

これで display: inline-block; が使えるブラウザでは display: inline-block; を使い、そうではない IE6 だけ display: inline; と zoom: 1; を効かせることが出来るようになりました。

## まとめ

IE6 でも display: inline-block; を使いたかった僕たちは、「アンダースコアハック」を使って IE6 だけに `zoom: 1;` を指定することで hasLayout property を true にして、`display: inline;` の指定で `display: inline-block;` と同様の横並びを実現しました。

それが下記のコードです。

```
.style {
  display: inline-block;
  _display: inline;
  _zoom: 1;
}
```

[gist ikkou/1d68aa31e45473a5c6e6](https://gist.github.com/ikkou/1d68aa31e45473a5c6e6)

こうして僕たちは IE6 で display: inline-block; を使うという悲願を達成したのでした。

ちなみに、これはまだ IE7 が存在しなかった頃のお話で、IE6 同様に爆発を願う人が多いであろう IE7 も display: inline-block; は使えず、それを嘆いた人々は「アスタリスクハック」という方法を見出しましたが、それはまた別のお話。

## 昔話はまだまだ続く

明日の語り手は Kazuhito Kidachi さんです。
Web Accessibility の人が語る昔話、期待です。
