---
title: Dockerfileを書いてみる
tags: dockerfile vagrant VirtualBox Ubuntu CentOS
author: nl0_blu
slide: false
---
# やること

Docker fileの書き方、仕組みを理解する。
[Dockerを0から勉強する](http://qiita.com/nl0_blu/items/07e5047041ebe009dd06)
ではDockerfileを使わない方法を記載しています。

##流れ(イメージ)
※2017/5/4 図の間違えを修正
![Dockerfile図.png](https://qiita-image-store.s3.amazonaws.com/0/161771/acb79a98-68a8-8594-0d5a-7d6af287841a.png)



##開発環境

* Vitualbox(5.1.0)
* Vagrant(1.9.1)
* Docker(1.13.1)
* Ubuntu(16.04)
* CentOS7

##1. Dockerfileの作成

```
$ vim Dockerfile
```

```Dockerfile
# どのイメージを基にするか
FROM centos
# 作成したユーザの情報
MAINTAINER Admin <admin@admin.com>
# RUN: docker buildするときに実行される
RUN echo "now building..."
# CMD: docker runするときに実行される
CMD echo "now running..."
```

Dockerfileをビルドして"admin/echo"イメージの作成
-t : ビルド成功後メッセージ表示

```
$ sudo docker build -t admin/echo .
```

"admin/echo"イメージからコンテナの作成

```
$ sudo docker run admin/echo
```

##2. webサーバの立ち上げと編集の流れ

###①webサーバの立ち上げ

```
$ vim Dockerfile
```
```Dockerfile
# どのイメージを基にするか
FROM centos
MAINTAINER Admin <admin@admin.com>
# RUN: buildするときに実行される
# RUN echo "now building..."
# CMD: runするときに実行される
CMD echo "now running..."

# httpdのインストール
RUN yum install -y httpd
ホストのindex.htmlをImage内にコピー
ADD ./index.html /var/www/html/
#ポート80を開ける
EXPOSE 80
#runした時にapache起動
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
```

ホスト側のポートの8080番をコンテナ側の80番にリダイレクト

```
sudo docker run -p 8080:80 -d admin/httpd
```

ブラウザ上でホストの「ipアドレス:8080」にアクセス

-->表示されればOK

###②編集して再度イメージを作る

実行しているプロセスを切る

```
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
b34ac75ca77d        admin/httpd         "/usr/sbin/httpd -..."   48 seconds ago      Up 48 seconds       0.0.0.0:8080->80/tcp   loving_booth

$ sudo docker kill b34
```

適当に編集してみる

```
[直接編集]
$ sudo docker run -i -t admin/httpd /bin/bash
$ exit
[index.htmlの編集]
$ vim index.html
[etc.....]
```

編集後、イメージを更新する

```
$ sudo docker build -t admin/httpd .
```


コンテナを再度、作って動作確認

```
$ sudo docker run -p 8080:80 -d admin/httpd
```

##3. イメージをDocker hubにプッシュする

Docker hubにsign upより登録しておく
[https://hub.docker.com](https://hub.docker.com)

loginする(ユーザ名など聞かれる)

```
sudo docker login
```

pushする

```
sudo docker push admin/httpd
```

Docker hubにログインして、Repositoriesのところにadmin/httpdがあればOK

これをpullしたい時は

```
$ sudo docker pull admin/httpd
```

をすれば良い
