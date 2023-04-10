# このノートを見ると何が分かるか
1. [WindowsにWSL2をインストールする方法](#2-wslのインストール)
1. [Linux必須コマンド](#3linuxコマンド)
1. WSLをいい感じにセットアップする（Linuxにほぼ必須のものをいくつかインストールする。随時更新）
1. [Ubuntuの更新。](#6ubuntuの更新)
1. [Python3のインストール。](#7python3)
1. [GCCのインストール。](#8gcc)
1. [Fortranコンパイラのインストール。](#9gfortran)

## 追加予定
- Linux必須ツール（適当）リストを増やす。

# 1, Linuxについて
UNIXライクなオープンソースOS。  
様々なディストリビューションがある。有名どころはUbuntuとかDebianとか。  
シェルスクリプトBashで動きます。Windwsのcmdのシェルと同じ雰囲気だけど結構違う。  
git使うときのgit bashと同じ感じですね。

# 2, WSLのインストール
1. Powershellを管理者モードで起動する。
2. WSL（Windows Subsystem for Linux）をインストール。
	1. デフォルトだと新しめのUbuntuが入る。私がやったときは22.04.2
	2. 参考[WSL のインストール | Microsoft Learn](https://learn.microsoft.com/ja-jp/windows/wsl/install)
```Shell
wsl --install
```
3. 再起動
4. Usernameとpassword入力。
	1. passwordは見えない。
	2. passwordは2回打たされる。
5. 更新したい際はPower Shellで以下を入力。
```Shell
wsl --update
```

# 3,Linuxコマンド
GUIなら、わざわざWindows上でLinuxを入れる意味は薄い。  
コマンドラインで使いたいからこそ入れるのだ。  
LinuxはBash（Bourne again shell）というシェルスクリプトで動かせる。  
ここでは、以下の操作に必須のコマンドを解説する。  
実際に使う上ではさらに沢山必要になるので、Bashコマンド便利帳的なの（そのうちUPる）を参照。  

## 基本
```Shell
コマンド --オプション 作用先
```
と言った形で記述することが多いみたい。  
オプションは、-や--に続けて記述する。  
各要素は半角スペースで区切る。  
そのため、ディレクトリ名などにスペースを含む場合は、""で囲む。  

## cd　移動
場所移動。  
ファイル名にスペースが入っている場合は""で囲む。  
```Shell
cd ./"test file"
```
最も良いのは、スペースなんか入れないこと。

## ls　インデックス
直下のファイル一覧を取得。  
-aで隠しファイルも表示。

## sudo　管理者権限
sudoを冒頭に付けると、root権限を行使できる。  
パスワードの入力を求められることがある。  
まじで強力なので要注意。必要ない時に付けてはいけない。  
rootでいろんなものを実行するのも同様に注意。

## WSLでペースト
右クリックでペースト

# 4,apt（apt-get）
Advanced Package Tool  
色々なものをインストールするときに使う。  
[Linuxメモ: apt (apt-get) コマンドの使い方メモ｜まくろぐ](https://maku.blog/p/rdq2cnx/)  
apt-get等を良くしたのがaptなので、aptで良いと思う。  
様々なサイトにapt-getと書いてあったら、それをaptに置き換えて書けばよい。  
[Ubuntuのupdate、upgrade、do-release-upgradeとは | Snow Tree in June](https://snowtree-injune.com/2020/10/09/ubuntu-upgrade-dj007/)

## aptの更新
インストール出来る更新のインデックス（一覧）を取得する。
```Shell
sudo apt update
```
インデックスに基づいてインストールする。
``` Bash
sudo apt -y upgrade
```
-yは同意を省く。

# Vim
コマンドライン上でよく使うエディタ。  
スマホでTermux使ってた時に重宝した。

## Vimのインストール
```Shell
sudo apt-get install vim
```

## Vimの使い方
[Vim初心者に捧ぐ実践的入門 - Qiita](https://qiita.com/okamos/items/c97970ab34ff55ff3167)
| したいこと | 入力 |
| --- | --- |
| 上書き保存 | :w |
| 名前をつけて保存 | :w ファイル名 |
| 編集終了 | :q |
| 保存して終了 | :wq または :x |
| ファイルを開く | :e ファイル名 |
| 強制的な操作 | !を付す |

# 5,Windows Terminal
WSLから行くより、Windows Terminalを使った方が将来的に便利かもしれない。  
Microsoftストアより検索してインストールできる。  
新規タブの横にあるvみたいなところから、Ubuntuなどを選択する。  

# 6,Ubuntuの更新
私はまだやったことがない。
## lsb_release　バージョン確認
-aで全表示

## do-release-upgrade
[Ubuntu 20.04 LTS を 22.04 LTS にアップグレードする - Uzabase for Engineers](https://tech.uzabase.com/entry/2022/10/05/163458)
```Shell
sudo apt install update-manager-core
```
```Shell
sudo do-release-upgrade
```

# 7,Python3
[Linuxmania:Python、gcc、Fortranの開発環境を準備しよう(Ubuntu)](https://www.linuxmania.jp/aptget-site.html#python)
を参考にするが、少し古い。

## Python3のインストール
既存のPythonのバージョン確認。
```Shell
python3 -V
```
存在しなければインストール。
```Shell
sudo apt install python3
```

## pipのインストール
pipがあると便利らしい？  
Python書かないから知らんけど。
```Shell
sudo apt install python3-pip
```
パッケージのゲットはこうやります。
```Shell
pip3 install パッケージ名
```
これやるといいらしいけど、やってない。詳細不明。
```Shell
sudo apt install -y build-essential libssl-dev libffi-dev python3-dev
```

## Pythonの実行
```Shell
python3 ファイル名
```
例
```Shell
python3 hello.py
```

# 8,GCC
GNU Compiler Collection  
いろんな言語のコンパイラ詰め合わせ。  
[Linuxmania:Python、gcc、Fortranの開発環境を準備しよう(Ubuntu)](https://www.linuxmania.jp/aptget-site.html#gcc)

## GCCのインストール
既存のGCCのバージョン確認
```Shell
gcc -version
```
存在しなければインストール
```Shell
sudo apt install gcc
```

# 9,gfortran
GNU Fortran
GCCの一部

いろんなコンパイラがあるが、無料で良いのはgfortranらしい。
[Linuxmania:Python、gcc、Fortranの開発環境を準備しよう(Ubuntu)](https://www.linuxmania.jp/aptget-site.html#fortran)
Fortranの拡張子は、.f .f90 ,forなど。

## gfortranのインストール
既存のgfortranのバージョン確認
```Shell
gfortran --version
```
存在しなければインストール
```Shell
sudo apt install gfortran
```

## Fortranのコンパイル
```Shell
gfortran -o 出力ファイル名 元ファイル名
```
例
```Shell
gfortran -o hello.exe hello.f90
```
-oは出力したオブジェクトファイルの名前指定をするオプション。

## Fortranの実行
```Shell
./ファイル名
```
例
```Shell
./hello.exe
```

# meta data
Created 2023/04/10