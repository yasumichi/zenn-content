---
title: "po4a 化されていない man ページの po4a 化"
emoji: "🚀"
type: "tech"
topics:
  - "linux"
  - "gettext"
  - "man"
  - "jm"
  - "weblate"
published: true
published_at: "2025-05-31 20:50"
---

## 前回の記事

https://zenn.dev/yasumichi/articles/b93ed514a00275

## リストアップ

以下、左側が、manual ディレクトリにあるすべてのディレクトリ、右側は `po4a` 化が終わっているディレクトリです。

```sdiff
GNU_autoconf                                                    GNU_autoconf
GNU_automake                                                    GNU_automake
GNU_bash                                                        GNU_bash
GNU_bc                                                        <
GNU_binutils                                                  <
GNU_bison                                                       GNU_bison
GNU_coreutils                                                 <
GNU_cpio                                                        GNU_cpio
GNU_dejagnu                                                     GNU_dejagnu
GNU_diffutils                                                   GNU_diffutils
GNU_ed                                                          GNU_ed
GNU_fileutils                                                 <
GNU_findutils                                                   GNU_findutils
GNU_gawk                                                        GNU_gawk
GNU_gcc                                                       <
GNU_gdb                                                         GNU_gdb
GNU_gdbm                                                        GNU_gdbm
GNU_gettext                                                     GNU_gettext
GNU_gperf                                                       GNU_gperf
GNU_grep                                                      <
GNU_groff                                                     <
GNU_grub                                                        GNU_grub
GNU_gsl                                                         GNU_gsl
GNU_gzip                                                        GNU_gzip
GNU_indent                                                      GNU_indent
GNU_inetutils                                                   GNU_inetutils
GNU_less                                                      <
GNU_libtool                                                     GNU_libtool
GNU_m4                                                          GNU_m4
GNU_make                                                        GNU_make
GNU_patch                                                     <
GNU_rcs                                                       <
GNU_screen                                                      GNU_screen
GNU_sed                                                         GNU_sed
GNU_sh-utils                                                  <
GNU_sharutils                                                 <
GNU_tar                                                         GNU_tar
GNU_texinfo                                                     GNU_texinfo
GNU_textutils                                                 <
GNU_uucp                                                      <
GNU_which                                                       GNU_which
LDP_man-pages                                                   LDP_man-pages
SysVinit                                                      <
acl                                                             acl
apmd                                                          <
asymptote                                                       asymptote
at                                                              at
autofs                                                        <
bind                                                          <
bs                                                            <
bsd-games                                                     <
bsd-games-non-free                                            <
byacc                                                         <
bzip2                                                           bzip2
cdparanoia                                                    <
cdrecord                                                      <
cron                                                          <
cups                                                            cups
cvsup                                                         <
dblatex                                                         dblatex
dhcp                                                          <
dhcp2                                                         <
dhcpcd                                                        <
e2fsprogs                                                       e2fsprogs
ebtables                                                      <
efax                                                          <
eject                                                         <
expect                                                        <
fetchmail                                                     <
file                                                          <
flex                                                            flex
fontconfig                                                      fontconfig
fort77                                                        <
freetype                                                        freetype
ghostscript                                                     ghostscript
glibc-linuxthreads                                            <
gnumaniak                                                     <
hdparm                                                        <
ipchains                                                      <
ipchains-scripts                                              <
ipfwadm                                                       <
iptables                                                        iptables
itstool                                                         itstool
kmod                                                            kmod
ld.so                                                         <
libgcrypt                                                       libgcrypt
lids                                                          <
lilo                                                          <
logrotate                                                     <
lpr-linux                                                     <
man                                                             man
man-db                                                          man-db
meson                                                           meson
microcode_ctl                                                 <
mirrordir                                                     <
module-init-tools                                               module-init-tools
modutils                                                      <
mpg123                                                        <
ncftp                                                         <
ncurses                                                       <
net-tools                                                     <
netatalk                                                      <
netkit                                                        <
nfs-server                                                    <
nfs-utils                                                     <
nginx                                                           nginx
open-adventure                                                  open-adventure
pciutils                                                        pciutils
pcmcia-cs                                                     <
ppp                                                           <
procinfo                                                      <
procmail                                                        procmail
procps                                                        <
psmisc                                                          psmisc
qpdf                                                            qpdf
quota                                                         <
rdate                                                         <
reiserfsprogs                                                 <
rp-pppoe                                                      <
rpm                                                           <
rssh                                                          <
rsync                                                           rsync
sendmail                                                      <
setserial                                                     <
shadow                                                        <
sqlite                                                          sqlite
subversion                                                      subversion
sudo                                                            sudo
sysklogd                                                      <
sysstat                                                       <
tcp_wrappers                                                  <
tcpdump                                                       <
tcsh                                                          <
ucd-snmp                                                      <
upower                                                          upower
util-linux                                                      util-linux
uudeview                                                      <
vsftpd                                                        <
wu-ftpd                                                       <
xinetd                                                        <
xz                                                              xz
yp-tools                                                        yp-tools
ypbind                                                        <
ypbind-mt                                                       ypbind-mt
ypserv                                                          ypserv
```

## すでに po4a 化されているディレクトリの Makefile 調査

autoconf 用の Makefile を例とする。

[manual/GNU_autoconf/Makefile](https://github.com/linux-jm/manual/blob/master/manual/GNU_autoconf/Makefile)

```Makefile
PACKAGE_NAME = autoconf
man_numbers = 1

THRESH = 100
EXTFLAGS =
PO4AFLAGS += -k $(THRESH) $(EXTFLAGS)

target-mans = $(addprefix man,$(man_numbers))
po_dirs     = $(addprefix po4a/,$(target-mans))
po_files    = $(addsuffix /ja.po,$(po_dirs))

all: translate

translate: $(target-mans)

$(target-mans): man%:
        po4a $(PO4AFLAGS) po4a/man$*/$(PACKAGE_NAME)-man$*.cfg

stat:
        @for po in $(po_files); do \
          msgfmt -v --statistics -o /dev/null $$po; \
        done

page-stat:
        @for n in $(man_numbers); do \
          if test -f po4a/man$$n/$(PACKAGE_NAME)-man$$n.cfg; then \
            echo po4a/man$$n/$(PACKAGE_NAME)-man$$n.cfg: ;\
            po4a --force --no-update -k 0 po4a/man$$n/$(PACKAGE_NAME)-man$$n.cfg | \
              sed "s/^/  /g" ;\
          fi \
        done

.PHONY: all translate stat page-stat
```

## po4a 用設定ファイルの例

[manual/GNU_autoconf/po4a/man1/autoconf-man1.cfg](https://github.com/linux-jm/manual/blob/master/manual/GNU_autoconf/po4a/man1/autoconf-man1.cfg)

```ini
[po4a_langs] ja
[po4a_paths] po4a/man1/autoconf-man1.pot $lang:po4a/man1/ja.po
[po4a_alias: man] man opt:"-v --previous" opt_ja:"-M UTF-8"

[type: man] original/man1/autoconf.1 $lang:draft/man1/autoconf.1 \
        add_$lang:?po4a/add_$lang/copyright/autoconf.1.txt \
        opt:"-o generated"

[type: man] original/man1/autoheader.1 $lang:draft/man1/autoheader.1 \
        add_$lang:?po4a/add_$lang/copyright/autoheader.1.txt \
        opt:"-o generated"

[type: man] original/man1/autom4te.1 $lang:draft/man1/autom4te.1 \
        add_$lang:?po4a/add_$lang/copyright/autom4te.1.txt \
        opt:"-o generated"

[type: man] original/man1/autoreconf.1 $lang:draft/man1/autoreconf.1 \
        add_$lang:?po4a/add_$lang/copyright/autoreconf.1.txt \
        opt:"-o generated"

[type: man] original/man1/autoscan.1 $lang:draft/man1/autoscan.1 \
        add_$lang:?po4a/add_$lang/copyright/autoscan.1.txt \
        opt:"-o generated"

[type: man] original/man1/autoupdate.1 $lang:draft/man1/autoupdate.1 \
        add_$lang:?po4a/add_$lang/copyright/autoupdate.1.txt \
        opt:"-o generated"

[type: man] original/man1/ifnames.1 $lang:draft/man1/ifnames.1 \
        add_$lang:?po4a/add_$lang/copyright/ifnames.1.txt \
        opt:"-o generated"
```

## po4a は homebrew などで最新版をインストールした方が良さげ

`Ubuntu 24.04.2 LTS`(on WSL) で提供されている `po4a` のバージョンは、`0.69` ですが、後で説明する補遺(addendum)をうまく扱えませんでした。(非推奨の `po4a-translate` では、うまいくのに。)

色々、試してみましたが、[Homebrew — The Missing Package Manager for macOS (or Linux)](https://brew.sh/) を使うのが、一番、簡単でした。([plenv](https://github.com/tokuhirom/plenv) は挫折)

デフォルトのインストール方法だと `sudo` 権限を求められるので気になる方は、[Untar anywhere (unsupported)](https://github.com/Homebrew/brew/blob/master/docs/Installation.md#untar-anywhere-unsupported) の方法を試してみると良いかもしれません。

あと、偽サイトに気をつけましょう。

- [sudoが使えない場合にHomebrewをlocal directoryにインストールする](https://zenn.dev/watarungurunnn/articles/6e323d1030afcf)
- [偽のHomebrewのGoogle広告でマルウェアを拡散するキャンペーンが確認|セキュリティニュース](https://rocket-boys.co.jp/security-measures-lab/12217/)

Homebrew がインストール・設定できたら、以下のコマンドで `po4a` をインストールします。

```
$ brew install po4a
```

## 原文と既存の翻訳から PO ファイルを作成する

GNU grep を例に原文と既存の翻訳から、ja.po ファイルを作成していく。

### GNU_grep のディレクトリをカレントディレクトリに変更

```
$ cd manual/GNU_grep
```

### po4a 用のディレクトリを作成

```
$ mkdir -p po4a/man1
$ mkdir po4a/add_ja
```

### po4a-gettextize

[Man page of PO4A-GETTEXTIZE.1P](https://www.po4a.org/man/man1/po4a-gettextize.1.php) を参考に実行してみる。

```
$ po4a-gettextize --format man --master original/man1/grep.1 \
> --localized release/man1/grep.1 --po po4a/man1/ja.po
po4a-gettextize is only useful to convert previously existing translations to a PO based workflow. Once you successfully converted your project to po4a, you should use the po4a(1) program to maintain it and
update your translations.
original/man1/grep.1:2: (po4a::man)
               This page uses conditionals with '.if'. Since po4a is not a real groff parser, this is not supported by default. The option 'groff_code=verbatim' gets these macros copied verbatim in the
               translated file, but it's not very robust. 'groff_code=translate' shows these macros to the translators, but groff macros are not user-friendly for translators.
```

いきなりコケル。po4a は、groff パーサーではないのでデフォルトでは、`.if`をサポートしてないよと。

マクロ節を翻訳対象にするなら、`groff_code=translate` というオプションを指定するようにと。

[Man page of LOCALE::PO4A::MAN.3PM](https://www.po4a.org/man/man3/Locale::Po4a::Man.3pm.php) によると以下のマクロ節が対象になるようです。

| マクロ | 役割            |
| ------ | --------------- |
| .de    | マクロを定義する |
| .ie    | if/else の if   |
| .if    | 条件式          |

- `FreeBSD マニュアル検索` の [On-line Manual of "roff"](http://www.koganemaru.co.jp/cgi-bin/mroff.cgi?sect=7&cmd=&lc=1&subdir=man&dir=jpman-12.4.2%2Fman&subdir=man&man=roff)

### `groff_code=translate` オプションを指定してみる

```
$ po4a-gettextize --format man --master original/man1/grep.1 \
> --localized release/man1/grep.1 --po po4a/man1/ja.po \
> -o groff_code=verbatim
po4a-gettextize is only useful to convert previously existing translations to a PO based workflow. Once you successfully converted your project to po4a, you should use the po4a(1) program to maintain it and
update your translations.
original/man1/grep.1:1158: (po4a::man)
               不明なマクロ '.MTO bug-grep@gnu.org "the bug-reporting address" .' です。ドキュメントから取り除くか、po4aが新しいマクロを扱う方法について、Locale::Po4a::Man の manpage を参照してください。
```

`.MTO` なんてマクロ知らないよ、と。私も知らんがな。

`FreeBSD マニュアル検索` の [On-line Manual of "groff_www"](http://www.koganemaru.co.jp/cgi-bin/mroff.cgi?sect=7&cmd=&lc=1&subdir=man&dir=jpman-12.4.2%2Fman&subdir=man&man=groff_www)によると

> .MTO                HTML email アドレスを作成する

とのこと。~~余計なことを…~~

### 不明なマクロを翻訳対象外にする

[Man page of LOCALE::PO4A::MAN.3PM](https://www.po4a.org/man/man3/Locale::Po4a::Man.3pm.php)の[このモジュールで使用できるオプション](https://www.po4a.org/man/man3/Locale::Po4a::Man.3pm.php#lbAI)から、`unknown_macros` というオプションを見つけた。

> このオプションは、不明なマクロを検出した場合の po4a の挙動を指定します。デフォルトでは、po4a は警告を表示して失敗します。以下の値を取ることができます: failed (デフォルト値), untranslated, noarg, translate_joined, translate_each (値の説明は上記参照)。

```
$ po4a-gettextize --format man --master original/man1/grep.1 \
> --localized release/man1/grep.1 --po po4a/man1/ja.po \
> -o groff_code=verbatim -o unknown_macros=untranslated
po4a-gettextize is only useful to convert previously existing translations to a PO based workflow. Once you successfully converted your project to po4a, you should use the po4a(1) program to maintain it and
update your translations.
po4a gettextize: オリジナルは翻訳よりも文字列が多いです (226>225)。翻訳版にダミーのエントリを追加してください。
po4a gettext 化: オリジナルファイルと翻訳ファイルの構造不一致:
msgid (original/man1/grep.1:1147 にある、タイプ 'SH') と
msgstr (release/man1/grep.1:1202 にある、タイプ 'Plain text') です。
オリジナルテキスト: COPYRIGHT
翻訳テキスト: This is free software; see the source for copying conditions.  There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
(ここまでの結果は gettextization.failed.po に出力しました)
gettext 化に失敗しました (再通知)。あきらめないでください。gettext 化は繊細な手作業ですが、po4a が翻訳者に提供する豪華な拡張のために、一度だけプロジェクトを変換する必要があります。
po4a(7)「既存の訳を po4a にコンバートするには?」セクションにある、ヒントを参照してください
```

原文と翻訳の構造が合っていないと上記エラーとなり、途中までの翻訳結果が、`gettextization.failed.po` という名前でカレントディレクトリに保存される。

どこでずれているかの参考にすると良い。

### 翻訳文書の構造を原文に合わせる

原文と翻訳の構造が合っていないので合わせていく。

```diff
--- a/manual/GNU_grep/release/man1/grep.1
+++ b/manual/GNU_grep/release/man1/grep.1
@@ -1138,6 +1138,7 @@ SGR パラメータの値は十進法の整数であり、セミコロンで結
 これが設定されていると、
 .B grep
 は \s-1POSIX\s0 が要求するとおりの動作をします。
+.B grep
 設定されていない場合の動作は、ほかの \s-1GNU\s0 のプログラムに
 より近いものです。
 \s-1POSIX\s0 の規定では、ファイル名の後にオプションが現れた場合、
@@ -1151,43 +1152,42 @@ SGR パラメータの値は十進法の整数であり、セミコロンで結
 そうしたオプションも法律に違反しているわけではないので、
 .B grep
 のデフォルトでは、\*(lqinvalid\*(rq (無効) という判断を下します。
-.\" さらに、
-.\" .B POSIXLY_CORRECT
-.\" は、下記の \fB_\fP\fIN\fP\fB_GNU_nonoption_argv_flags_\fP を無効にします。
-.\" (訳注: 次項を表示しないことに合わせて、「さらに」以下の文も
-.\" コメントにします)
-.\" .TP
-.\" \fB_\fP\fIN\fP\fB_GNU_nonoption_argv_flags_\fP
-.\" (ここで
-.\" .I N
-.\" は、
-.\" .BR grep
-.\" のプロセス ID 番号です) もし、この環境変数の値の
-.\" .IR i
-.\" 番目の文字が
-.\" .BR 1
-.\" だったら、
-.\" .B grep
-.\" の
-.\" .IR i
-.\" 番目の引き数がオプションのように見えても、
-.\" それをオプションと見なしてはいけない、ということです。
-.\" シェルはコマンドを実行するたびに、そのコマンドの環境にこの変数を
-.\" 挿入して、どの引き数がファイル名をワイルドカード展開した結果であり、
-.\" それ故オプションとして扱ってはいけないかを指定することができます。
-.\" この動作は \s-1GNU\s0 C ライブラリを使用しているときにのみ、それも、
-.\" .B POSIXLY_CORRECT
-.\" が設定されていないときにのみ、利用することができます。
-.\" (訳注: この環境変数は、bash 2.0 で採用されたが、問題を起こすために
-.\" bash 2.01 で削除されたとのことです。それ故、ユーザからは man
-.\" コマンドで見えないようにしておきます。getopt(3) を参照してください)
+さらに、
+.B POSIXLY_CORRECT
+は、下記の \fB_\fP\fIN\fP\fB_GNU_nonoption_argv_flags_\fP を無効にします。
+(訳注: 次項を表示しないことに合わせて、「さらに」以下の文も
+コメントにします)
+.TP
+\fB_\fP\fIN\fP\fB_GNU_nonoption_argv_flags_\fP
+(ここで
+.I N
+は、
+.BR grep
+のプロセス ID 番号です) もし、この環境変数の値の
+.IR i
+番目の文字が
+.BR 1
+だったら、
+.B grep
+の
+.IR i
+番目の引き数がオプションのように見えても、
+それをオプションと見なしてはいけない、ということです。
+シェルはコマンドを実行するたびに、そのコマンドの環境にこの変数を
+挿入して、どの引き数がファイル名をワイルドカード展開した結果であり、
+それ故オプションとして扱ってはいけないかを指定することができます。
+この動作は \s-1GNU\s0 C ライブラリを使用しているときにのみ、それも、
+.B POSIXLY_CORRECT
+が設定されていないときにのみ、利用することができます。
 .
 .SH "終了ステータス"
 通常では、選択される行が見つかったときの終了ステータスは 0 であり、
 見つからなかったときは 1 であり、エラーが起きた場合は 2 です。
 ただし、
-.B \-q ,
-.B \-\^\-quiet ,
+.B \-q
+,
+.B \-\^\-quiet
+,
 .B \-\^\-silent
 といったオプションが使われていて、選択される行が見つかったときは、
 エラーが起きたときでも終了ステータスは 0 です。
@@ -1220,7 +1220,7 @@ not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
 がメモリ不足を起こすことがあります。
 .PP
 後方参照は非常に遅く、とんでもなく時間がかかることがあります。
-.PP
+.
 .SH "関連項目"
 .SS "標準のマニュアルページ"
 awk(1), cmp(1), diff(1), find(1), gzip(1),
@@ -1232,8 +1232,10 @@ glob(7), regex(7).
 .SS "\s-1POSIX\s0 プログラマーズ・マニュアルページ"
 grep(1p).
 .SS "完全版の文書"
+ひとつの
 .URL http://www.gnu.org/software/grep/manual/ "完全版のマニュアル"
 が用意されています。
+もし
 .B info
 と
 .B grep
@@ -1242,13 +1244,9 @@ grep(1p).
 .B info grep
 .PP
 とコマンドを打ち込むことで、完備したマニュアルが読めるはずです。
+.
 .SH "注記"
 このマニュアルは断続的にメンテナンスされるため、
 完全版の文書のほうが最新であることがよくあります。
-.SH "訳者謝辞"
-この翻訳は、FreeBSD jpman Project <http://www.jp.freebsd.org/man-jp/>
-から Linux JM project に寄贈していただいたマニュアルを元にし、
-GNU grep の新しいマニュアルに合わせて、増補・改訂しています。
-この場を借りて、FreeBSD jpman Project の翻訳者の方々にお礼を申し上げます。
 .\" Work around problems with some troff -man implementations.
 .br
```

削除した部分は、補遺として別途追加することにする。敢えてコメントアウトされていた部分は、とりあえず、原文が修正されていることに期待する。

```
$ po4a-gettextize --format man --master original/man1/grep.1 \
> --localized release/man1/grep.1 --po po4a/ja.po \
> -o groff_code=verbatim -o unknown_macros=untranslated
po4a-gettextize is only useful to convert previously existing translations to a PO based workflow. Once you successfully converted your project to po4a, you should use the po4a(1) program to maintain it and
update your translations.
```

これでエラーなく、`po4a-gettextize` が成功した。

## POT ファイルの作成

[Man page of PO4A.1P](https://www.po4a.org/man/man1/po4a.1.php) に以下の記述がある。

> po4aの設定を完了するために最後に必要なものは、新しい翻訳を開始するために使用されるべきテンプレート材料を含むPOTファイルです。指定されたpo_directoryの中にに拡張子.potの空のファイルを作成するだけです（例：man/po4a/foo.pot）。そしてpo4aはその中に期待される内容を詰め込みます。

とりあえず、空の POT ファイルを作成する。

```
$ touch po4a/man1/grep-man1.pot
```

## 補遺の作成

上で削除した`訳者謝辞` を追加するために `po4a/add_ja/thanks.txt` という名前で補遺を作成した。

```
PO4A-HEADER: mode=after; position=注記; beginboundary=\.SH
.SH "訳者謝辞"
この翻訳は、FreeBSD jpman Project <http://www.jp.freebsd.org/man-jp/>
から Linux JM project に寄贈していただいたマニュアルを元にし、
GNU grep の新しいマニュアルに合わせて、増補・改訂しています。
この場を借りて、FreeBSD jpman Project の翻訳者の方々にお礼を申し上げます。
```

また、コメントとして存在した FreeBSD jpman Project からの contribute である旨、`po4a/add_ja.freebsd.txt` という名前で補遺を作成した。

```
PO4A-HEADER: mode=before; position=GREP 1 .*
.\"
.\" About Japanese translation
.\"   The original version was contributed to Linux JM project
.\"     by FreeBSD jpman Project.
.\"     It contained these lines:
.\"       %FreeBSD: src/gnu/usr.bin/grep/grep.1,v 1.16.2.3 2001/11/27 08:25:45 ru Exp %
.\"       .Id %Id: grep.1,v 1.3 2000/06/09 21:58:50 horikawa Exp %
.\"   Updated and Modified (grep-2.6.3) Thu Nov 11 11:44:47 JST 2010
.\"     by Chonan Yoichi <cyoichi@maple.ocn.ne.jp>
.\"   Updated and Modified (grep-2.27) Thu Feb 25 2017
.\"     by Masakazu Takahashi <emasaka@gmail.com>
.\"
```

以上のファイルリストを `po4a/add_ja/grep.ja.add` という名前で作成した。

```
po4a/add_ja/thanks.txt
po4a/add_ja/freebsd.txt
```

## po4a の設定ファイルを作成

po4a の設定ファイルを `po4a/man1/grep-man1.cfg` で作成した。

```
[po_directory] po4a/man1

[type: man] original/man1/grep.1 $lang:draft/man1/grep.1 \
       opt:"-o groff_code=translate" \
       opt:"-o inline=FN" opt:"-o noarg=zY,zZ" \
       opt:"-o unknown_macros=untranslated" \
       add_ja:@po4a/add_ja/grep.ja.add
```

## po4a で実行できることを確認

一旦、翻訳先のファイルを削除した。

```
$ rm draft/man1/grep.1
```

それから、po4a コマンドに上記の設定ファイルを指定して実行した。

```
$ po4a po4a/man1/grep-man1.cfg
```

詳しい情報を得たい場合は、`-v` オプションを指定する。

```
$ po4a -v po4a/man1/grep-man1.cfg
```

ここで生成した manpage を確認した。

```
$ man -l draft/man1/grep.1
```

翻訳先のファイルを元のバージョンに戻すため、以下のコマンドを実行。

```
$ git checkout -f
```

## Makefile の作成

GNU_autoconf の Makefile をコピーしてきて、`PACKAGE_NAME` を修正。

```makefile
PACKAGE_NAME = grep
man_numbers = 1

THRESH = 100
EXTFLAGS =
PO4AFLAGS += -k $(THRESH) $(EXTFLAGS)

target-mans = $(addprefix man,$(man_numbers))
po_dirs     = $(addprefix po4a/,$(target-mans))
po_files    = $(addsuffix /ja.po,$(po_dirs))

all: translate

translate: $(target-mans)

$(target-mans): man%:
        po4a $(PO4AFLAGS) po4a/man$*/$(PACKAGE_NAME)-man$*.cfg

stat:
        @for po in $(po_files); do \
          msgfmt -v --statistics -o /dev/null $$po; \
        done

page-stat:
        @for n in $(man_numbers); do \
          if test -f po4a/man$$n/$(PACKAGE_NAME)-man$$n.cfg; then \
            echo po4a/man$$n/$(PACKAGE_NAME)-man$$n.cfg: ;\
            po4a --force --no-update -k 0 po4a/man$$n/$(PACKAGE_NAME)-man$$n.cfg | \
              sed "s/^/  /g" ;\
          fi \
        done

.PHONY: all translate stat page-stat
```