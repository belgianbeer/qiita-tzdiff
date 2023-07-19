# はじめに

海外の「現地時間は今何時」を知りたいことはよくある話である。単純に現在の時刻を調べるのであれば、世界時計を眺めれば済む話で、Webサービスも多数ある。ところが「現地時間の午前9時は日本時間の何時」とか知りたい場合は意外に調べやすくない。スマホにある世界時計の機能も、現在時刻を知るのは簡単だが、今の時間とは違う時間については容易ではない。

自分は普段シェルのコマンドラインを利用していることが多いので、時差をコマンドラインで表示するツールが欲しくなったということもあって作ってみた tzdiff コマンドを紹介する。

> この記事の公開の段階では、コマンド名は **timediff** だったが、別のコマンドとコマンド名が被ることが判明したため、**tzdiff** という名前に変更した。(2018-09-04 更新)

[tzdiff のダウンロード](https://github.com/belgianbeer/tzdiff)

# tzdiff の使い方

tzdiff を引数無しで実行すると、/usr/share/zoneinfo の 一覧が得られる

```
$ tzdiff
Africa/         EET             GMT0            MST             Singapore
America/        EST             Greenwich       MST7MDT         Turkey
Antarctica/     EST5EDT         HST             Mexico/         UCT
Arctic/         Egypt           Hongkong        NZ              US/
Asia/           Eire            Iceland         NZ-CHAT         UTC
...
```

この中から Australia を指定する。

```
$ tzdiff Australia
Australia/ACT           Australia/Hobart        Australia/Queensland
Australia/Adelaide      Australia/LHI           Australia/South
Australia/Brisbane      Australia/Lindeman      Australia/Sydney
Australia/Broken_Hill   Australia/Lord_Howe     Australia/Tasmania
Australia/Canberra      Australia/Melbourne     Australia/Victoria
Australia/Currie        Australia/NSW           Australia/West
Australia/Darwin        Australia/North         Australia/Yancowinna
Australia/Eucla         Australia/Perth
```

ここでAustralia/Sydneyを指定すると、時差が10時間分表示される。

```
$ tzdiff Australia/Sydney
Sydney
2023-07-19 23:56 AEST   2023-07-19 22:56 JST
2023-07-20 00:56 AEST   2023-07-19 23:56 JST
2023-07-20 01:56 AEST   2023-07-20 00:56 JST
2023-07-20 02:56 AEST   2023-07-20 01:56 JST
2023-07-20 03:56 AEST   2023-07-20 02:56 JST
2023-07-20 04:56 AEST   2023-07-20 03:56 JST
2023-07-20 05:56 AEST   2023-07-20 04:56 JST
2023-07-20 06:56 AEST   2023-07-20 05:56 JST
2023-07-20 07:56 AEST   2023-07-20 06:56 JST
2023-07-20 08:56 AEST   2023-07-20 07:56 JST
```

このように引数に知りたい地域の Time Zone を指定するだけである。デフォルトでは今の時間から10時間分表示するが、24時間分表示したいのであれば、最後に数字の引数を指定する。

```
$ tzdiff America/New_York 24
New_York
2023-07-19 10:00 EDT   2023-07-19 23:00 JST
2023-07-19 11:00 EDT   2023-07-20 00:00 JST
2023-07-19 12:00 EDT   2023-07-20 01:00 JST
2023-07-19 13:00 EDT   2023-07-20 02:00 JST
2023-07-19 14:00 EDT   2023-07-20 03:00 JST
2023-07-19 15:00 EDT   2023-07-20 04:00 JST
2023-07-19 16:00 EDT   2023-07-20 05:00 JST
2023-07-19 17:00 EDT   2023-07-20 06:00 JST
2023-07-19 18:00 EDT   2023-07-20 07:00 JST
2023-07-19 19:00 EDT   2023-07-20 08:00 JST
2023-07-19 20:00 EDT   2023-07-20 09:00 JST
2023-07-19 21:00 EDT   2023-07-20 10:00 JST
2023-07-19 22:00 EDT   2023-07-20 11:00 JST
2023-07-19 23:00 EDT   2023-07-20 12:00 JST
2023-07-20 00:00 EDT   2023-07-20 13:00 JST
2023-07-20 01:00 EDT   2023-07-20 14:00 JST
2023-07-20 02:00 EDT   2023-07-20 15:00 JST
2023-07-20 03:00 EDT   2023-07-20 16:00 JST
2023-07-20 04:00 EDT   2023-07-20 17:00 JST
2023-07-20 05:00 EDT   2023-07-20 18:00 JST
2023-07-20 06:00 EDT   2023-07-20 19:00 JST
2023-07-20 07:00 EDT   2023-07-20 20:00 JST
2023-07-20 08:00 EDT   2023-07-20 21:00 JST
2023-07-20 09:00 EDT   2023-07-20 22:00 JST
```

毎時 0 時を指定したければ引数に 0 を追加する

```
$ tzdiff America/New_York 0
New_York
2023-07-19 10:00 EDT   2023-07-19 23:00 JST
2023-07-19 11:00 EDT   2023-07-20 00:00 JST
2023-07-19 12:00 EDT   2023-07-20 01:00 JST
2023-07-19 13:00 EDT   2023-07-20 02:00 JST
2023-07-19 14:00 EDT   2023-07-20 03:00 JST
2023-07-19 15:00 EDT   2023-07-20 04:00 JST
2023-07-19 16:00 EDT   2023-07-20 05:00 JST
2023-07-19 17:00 EDT   2023-07-20 06:00 JST
2023-07-19 18:00 EDT   2023-07-20 07:00 JST
2023-07-19 19:00 EDT   2023-07-20 08:00 JST
```

これなら一目瞭然で現地時間の 7:00 は日本時間の何時？というのがすぐにわかる。
また `tzdiff America/New_York 24 0` のように毎時 0分で 24時間分という指定もできる。

都市の指定は、UNIX系OSで /usr/share/zoneinfo にある Time Zone Database をそのまま利用している。そのため "UTC", "Etc/GMT+9" なども指定できる。

さらに tzdiff ではちょっとしたコンプリーションもどきを実装して、使い勝手をよくしてある。

```
$ tzdiff Lon  ( ここで最後は Tab ではなく Return つまり引数に "Lon" を指定する。)
Arctic/Longyearbyen     Europe/London
```

都市名が 「Lon」で始まる都市の一覧が表示されるので、「Lond」まで指定すれば都市が確定する。

```
$ tzdiff Lond
London
2023-07-19 15:01 BST   2023-07-19 23:01 JST
2023-07-19 16:01 BST   2023-07-20 00:01 JST
2023-07-19 17:01 BST   2023-07-20 01:01 JST
2023-07-19 18:01 BST   2023-07-20 02:01 JST
2023-07-19 19:01 BST   2023-07-20 03:01 JST
2023-07-19 20:01 BST   2023-07-20 04:01 JST
2023-07-19 21:01 BST   2023-07-20 05:01 JST
2023-07-19 22:01 BST   2023-07-20 06:01 JST
2023-07-19 23:01 BST   2023-07-20 07:01 JST
2023-07-20 00:01 BST   2023-07-20 08:01 JST
```

今時のシェルはコマンドライン編集機能が充実しているので、このように操作するのは簡単なはずである。

また、指定した時刻からも調べられる。次の例は、アメリカ東海岸で2019年の夏時間から標準時間に切り替わる時間帯を表示している。

```
$ tzdiff -t 2023-11-05T10:00 New_
New_York
2023-11-04 21:00 EDT   2023-11-05 10:00 JST
2023-11-04 22:00 EDT   2023-11-05 11:00 JST
2023-11-04 23:00 EDT   2023-11-05 12:00 JST
2023-11-05 00:00 EDT   2023-11-05 13:00 JST
2023-11-05 01:00 EDT   2023-11-05 14:00 JST
2023-11-05 01:00 EST   2023-11-05 15:00 JST
2023-11-05 02:00 EST   2023-11-05 16:00 JST
2023-11-05 03:00 EST   2023-11-05 17:00 JST
2023-11-05 04:00 EST   2023-11-05 18:00 JST
2023-11-05 05:00 EST   2023-11-05 19:00 JST
```

同時に複数の都市も指定できる。ワールドワイドなテレカンの時間調整にはもってこいである。

```
$ tzdiff  Los New_ Brus 0
Los_Angeles            New_York               Brussels
2023-07-19 07:00 PDT   2023-07-19 10:00 EDT   2023-07-19 16:00 CEST   2023-07-19 23:00 JST
2023-07-19 08:00 PDT   2023-07-19 11:00 EDT   2023-07-19 17:00 CEST   2023-07-20 00:00 JST
2023-07-19 09:00 PDT   2023-07-19 12:00 EDT   2023-07-19 18:00 CEST   2023-07-20 01:00 JST
2023-07-19 10:00 PDT   2023-07-19 13:00 EDT   2023-07-19 19:00 CEST   2023-07-20 02:00 JST
2023-07-19 11:00 PDT   2023-07-19 14:00 EDT   2023-07-19 20:00 CEST   2023-07-20 03:00 JST
2023-07-19 12:00 PDT   2023-07-19 15:00 EDT   2023-07-19 21:00 CEST   2023-07-20 04:00 JST
2023-07-19 13:00 PDT   2023-07-19 16:00 EDT   2023-07-19 22:00 CEST   2023-07-20 05:00 JST
2023-07-19 14:00 PDT   2023-07-19 17:00 EDT   2023-07-19 23:00 CEST   2023-07-20 06:00 JST
2023-07-19 15:00 PDT   2023-07-19 18:00 EDT   2023-07-20 00:00 CEST   2023-07-20 07:00 JST
2023-07-19 16:00 PDT   2023-07-19 19:00 EDT   2023-07-20 01:00 CEST   2023-07-20 08:00 JST
```

一応簡単な ヘルプもある。

```
$ tzdiff -h
Usage: tzdiff [-0lvHN] [-n count] [-f format] [-t start] timezone [timezone ...] [count] [0]
    -0:  round down to hour
    -n:  max hours (default: 10)
    -l:  display full timezone name, only city in default
    -f:  output format (using '+output_fmt' of 'date' command)
    -t:  specify the start time (YYYY-mm-ddTHH:MM[Z], etc...)
    -v:  show version.
    -H:  scripting mode (with timezone name)
    -HH: scripting mode (without timezone name)
    -N:  without localtime
```

# 動作環境

tzdiffそのものはシェルスクリプトであり、FreeBSD / NetBSD / OpenBSD / macOS / Debian / Ubuntu / CentOS での動作を確認している。UNIX系のOSであれば大部分のもので問題無く動作すると思われるが、date コマンドのオプションの違いで動かないものがあるかもしれない。その場合は GNU の date コマンドを用意すれば動くはずである。Windows の場合は、Windows Subsystem for Linux で利用できる。なおFreeBSDではpkg、NetBSDではpkgsrc、DebianやUbuntuではaptでインストールできる。macOSではMacPortsやHomeBrewでもインストールできる。

# おわりに

tzdiff の最初の版である timediff を作ったのは 2016年7月頃なので、すでに2年ほど経過し、機能追加や細かい修正を行っている。自分でも驚いたのだが Qiita に投降しようと下書きを書いたのが 2017年7月頃だったようである。すでに内容が古くなっていたため、現在の仕様に合わせて記事を修正した。

2023-07-19追記：tzdiffのバージョン1.2に合わせて実行例を更新。