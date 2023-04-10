# Dockerについて
Docker EngineとDocker Desktopがある。
Docker DesktopはGUIで重い。
CUIでやるならDocker Engineで十分。

[初心者による初心者のためのDocker入門 その１ dockerコマンド編 - Qiita](https://qiita.com/k5n/items/2212b87feac5ebc33ecb)

# apt-getでインストールする
apt searchで出てくるDockerやDocker.ioは旧バージョンの模様。  

公式文書を参考に正式にインストールを進める。  
[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

方法は4つあるらしいが、今回はaptレポジトリからインストールする方法を取る。更新が容易らしい。

## 0,旧バージョンを削除する。
もし旧バージョンがあった場合、いらないなら削除してしまう。  
その際、アンインストールするだけでは削除されないものがあるので、rmで削除する。  
※要るなら削除しないこと！
```Bash
sudo apt remove docker docker-engine docker.io containerd runc
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```
## 1,Dockerレポジトリをセットアップする。
新しいコンピュータにDockerを入れる前に、まずDockerレポジトリをセットアップしなければならないらしい。
```Shell
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
```
公式文書は改行した上で/（バックスラッシュ）で繋いでいるが、見にくいので改行を削除しました。
## 2,Dockerの公式GPGキーを追加。
- GPGキーが何なのかはよく分からん。GNU Privacy Guardらしい。鍵交換関係？
  - [GPGについて学んだことを整理してみる - Qiita](https://qiita.com/y518gaku/items/435838097c700bbe6d1b)
  - よく分かんないけどとりあえずやりました。sshと同じような感覚なのかな？
```Shell
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
## 3,下のコマンドをぶち込んでレポジトリをセットアップしろ！だそうで。
- なんか解説をちゃんとしてくれよのお気持ちを感じます。
```Shell
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
- echo打ってるのにターミナルに何も表示されないのはなぜ？
  - echoしたのをteeでdocker.listに書き込み。
  - teeの標準出力を/dev/nullに捨てているので画面表示がない。
## 4,apt-get updateをまたやる。
```Shell
 sudo apt-get update
```
- この時GPGエラーが出たら、公式文書にtipsがあるので読む。
## 5,Docker Engine，containerd，Docker Composeをインストールする。
私は最新版だけでいいのでめんどいことはしません。
```Shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
396MBとりますよと警告されました。Y。
## 6,インストールがうまくいったか試す。
```Shell
 sudo docker run hello-world
```
うまくいかなかった。
```
docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.
See 'docker run --help'.
```
こんなのが返ってきた。

### デーモン起動
調べるとデーモンが動いてないっぽい。  
[docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running? への対処法 – // もちぶろ](https://slash-mochi.net/blog/2022/07/18/docker-cannot-connect-to-the-docker-daemon-at-unix-var-run-docker-sock-is-the-docker-daemon-running-%E3%81%B8%E3%81%AE%E5%AF%BE%E5%87%A6%E6%B3%95/)

```Shell
sudo service docker stop
```
- 試しにデーモンをストップさせてみると、既に止まってるとの回答。やっぱり。
```
* Docker already stopped - file /var/run/docker-ssd.pid not found.
```

- と言うわけで動かします。
```
sudo service docker start
```
動きました。
```
Starting Docker: docker
```

- もう一度6を実行。
```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
「hello-world:latest」なんてローカルにないよ、とのお叱りの後、こんなメッセージが返ってきました。  
上手くいったっぽい。  
一件落着。

<br/><br/><br/>

- ちなみに、このような情報を見つけました。

[Docker Desktopを使わずにWindowsでDocker | IIJ Engineers Blog](https://eng-blog.iij.ad.jp/archives/14205)  
IPアドレス後で変えときます。

# Docker Desktop for Windows
Docker Desktop for Windowsをインストールして、WSLから使う。  
[Windows＋WSL2でDocker環境を用意しよう - カゴヤのサーバー研究室](https://www.kagoya.jp/howto/cloud/container/wsl2_docker/)  

私は試してない。

# meta data
created 2023/04/11