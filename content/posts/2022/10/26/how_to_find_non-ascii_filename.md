---
title: "macOSで非ASCII文字を検索する"
date: 2022-10-26T09:48:56+09:00
draft: false
---

非ASCII文字の処理ができないプログラムで扱うファイルの中身、あるいはファイル名を検索したい場合がある。

Perl正規表現の[16進数エスケープ](https://perldoc.jp/docs/perl/5.18.1/perlrebackslash.pod#Hexadecimal32escapes)を使う。
ただし、`BSDのgrep`では、`-P`オプションが使えない。

<!--more-->

grepのバージョン

```
❯ grep --version
grep (BSD grep, GNU compatible) 2.6.0-FreeBSD
```

## Perl正規表現をつかう

下記の通り、Perl正規表現の16進数エスケープでASCIIを指定したい。

```
[^\x00-\x7F]
```

pcre2をインストールする。

```
$ brew install pcre2

$ pcre2grep --version
pcre2grep version 10.40 2022-04-14
```

下記で、非ASCII文字を抽出できる。

```
$ pcre2grep --color='auto' -un '[^\x00-\x7F]' test.txt
```

ファイル名なら、[ASCII印字可能文字](https://ja.wikipedia.org/wiki/ASCII#ASCII%E5%8D%B0%E5%AD%97%E5%8F%AF%E8%83%BD%E6%96%87%E5%AD%97)の範囲を指定してやる方法でも実用的にOK。

```
$ find . -type f | grep -E '[^ -~]'
```

## 参考

https://stackoverflow.com/questions/3001177/how-do-i-grep-for-all-non-ascii-characters