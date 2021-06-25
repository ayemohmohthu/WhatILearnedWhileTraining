が2019年12月27日に作成
docker-lambda - AWS Lambda の開発効率を爆アゲする
Node.js
Docker
ローカル開発環境
AWSLambda
docker-lambda
この記事は最終更新日から1年以上が経過しています。
要約
AWS Lambda の Function を実装するときは docker-lambda を使用すると開発スピードが上がる。
Docker コンテナが提供されているので、導入に手間がない。
AWS Lambda の Function 実行だけでなく、 Deploy package の作成にも便利。
はじめに
AWS Lambda を使用することが増えてきたのですが、イチイチ AWS アカウントの設定や、 AWS Lambda へのデプロイが手間だと感じていました。そこで、ローカル開発環境上で実行できる方法を探したところ、 docker-lambda なるものの存在を知ったのでお試ししてみました。

概要
本記事の概要は次の通りです。

書くこと
docker-lambda の導入方法
docker-lambda の実行方法（Node.js版）
AWS Lambda へのデプロイ用パッケージを作成する方法
書かないこと
docker 環境の構築方法
Node.js や npm のインストール方法
想定環境
この記事で実現するシステムの構成イメージは下図の通りです。
今回は Node.js を使用して実行することを想定します。

image.png

Web Application を実装する際は、コンテナ内に VS Code Server を配置したいなと考えているのですが、今回は コンテナの特徴上、コンテナが稼働するOS上での実装の方が都合が良さそうなのでこの構成にしました。

docker-lambda の使用によって実現できること
docker-lambda とは
Item	Content
Copyright	Michael Hart and LambCI contributors
GitHub	lambci/docker-lambda
LICENSE	MIT License
What you Can	- Run Code on Docker Container
- Build and Deploy Code to AWS Lambda
Mode	- a single execution (Default)
- an API server that listens for invoke events
実現できること
docker-lambda を使用することによって、特に次のようなことが便利になります。

AWS アカウントがなくても AWS Lambda Function をお試し実行できる。
AWS環境へと デプロイする手間なくお試し実行できる。
ローカル環境でデバッグと実行できるのが助かりますね。

docker-lambda を試す
0. 基盤構成
この記事で実現している構成の基本的な要素は以下の記事で解説しています。

Visual Studio Code - "Remote Development" を使って Docker Container on "Vagrant + VirtualBox" のファイルを編集する #Qiita
1. 実行コードの準備
サンプルコードは、以下の通りです。

index.js
exports.handler = async (event, context) => {
  // process 関連の情報を色々出力
  console.log("process.execPath: ", process.execPath)
  console.log("process.execArgv: ", process.execArgv)
  console.log("process.argv: ", process.argv)
  console.log("process.cwd(): ", process.cwd())
  console.log("process.mainModule.filename: ", process.mainModule.filename)
  console.log("__filename: ", __filename)
  console.log("process.env: ", process.env)
  console.log("process.getuid(): ", process.getuid())
  console.log("process.getgid(): ", process.getgid())
  console.log("process.geteuid(): ", process.geteuid())
  console.log("process.getegid(): ", process.getegid())
  console.log("process.getgroups(): ", process.getgroups())
  console.log("process.umask(): ", process.umask())

  // 引数の値を出力
  console.log("event: ", event)
  console.log("context: ", context)

  context.callbackWaitsForEmptyEventLoop = false

  // 実行時間
  console.log("context.getRemainingTimeInMillis(): ", context.getRemainingTimeInMillis())

  return context.logStreamName
}
Node.js の AWS Lambda 関数ハンドラー - AWS
docker-lambda/examples/nodejs8.10/index.js
2. docker-lambda を使用して実行
実行方法の詳細については 「lambci/docker-lambda」 を参照頂くとして、 Lambda 関数を実行するための基本構文は以下の通りです。

command(bash@GuestOS)
# 
# Run a command in a new container
# 
# --rm          Automatically remove the container when it exits
# --volume , -v     Bind mount a volume
# 
# (unzipped) lambda code at /var/task
# (unzipped) layer code at /opt,
# 
# [<handler>] : if Node.js -> handler = file_name.function_name
#   ex. Filename: index.js, Function nama: handler
#         -> index.handler
#
# 実行完了   : https://github.com/lambci/docker-lambda#docker-tags
# 環境変数   : https://github.com/lambci/docker-lambda#environment-variables
# 
docker run --rm \
  -v <code_dir>:/var/task:ro,delegated \
  [-v <layer_dir>:/opt:ro,delegated] \
  lambci/lambda:<runtime> \
  [<handler>] [<event>]
Command-Line Interfaces (CLIs) - docker run | docker docs
1.) で作成したコードを試す場合のコマンドは次の通りです。

command(bash@GuestOS)
#
# 成功すると、色々と出力されます。
#
docker run --rm \
  -v "$PWD":/var/task \
  lambci/lambda:nodejs12.x \
  index.handler '{"runtime": "nodejs12.x"}'
以上のように、lambci/lambda に、コードの値やオプションを適切に引き渡すことで、 Docker コンテナ上で AWS Lambda の機能を実行し、結果を取得することが出来ます。

手間も少なく、非常に便利ですね。

デプロイ パッケージの作成
docker-lambda を使用することで、 AWS Lambda へデプロイするパッケージファイルを作成することが出来ます。

以降はデプロイ・パッケージの作成方法を記載します。

1. package.json の作成
AWS Lambda でアップロードする package をゼロから作成してゆきます。

command(bash@GuestOS)
# Create Project Directory
$ mkdir docker-lambda-deploy-package && cd $_

# (Global) Install npm-add-dependencies
# これはやっても、やらなくても OK です。
$ sudo npm install -g npm-add-dependencies

# Create New Project (package.json)
$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (docker-lambda-deploy-package)  [Enter]
version: (1.0.0)   [Enter]
description:   [Enter]
entry point: (index.js)   [Enter]
test command:   [Enter]
git repository:   [Enter]
keywords:   [Enter]
author:   [Enter]
license: (ISC)   [Enter]
About to write to /vagrant/workspace/private/docker-lambda-deploy-package/package.json:

{
  "name": "docker-lambda-deploy-package",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this OK? (yes)  [Enter]

# install axios
$ npm-add-dependencies axios
This script adds dependencies (latest or specified versions) to the package.json file without installing them
Adding packages to 'dependencies'...
Processed: axios, latest version: 0.19.0
Done.
docker-lambda を使って AWS Lambda Function を開発する方法 - to-me-mo-rrow
npm でパッケージをインストールせずに package.json の dependencies を更新する方法 - to-me-mo-rrow
2. index.js の作成
メインの関数を作成します。

index.js
const axios = require("axios");

exports.handler = async (event, context) => {
  const res = await axios.get("http://dummy.restapiexample.com/api/v1/employee/1");
  console.log(res.data);
  return res.data;
}
3. デプロイ用 Docker Image の作成と Build 、 package 作成
ビルド及びデプロイ用の Docker Image を作成します。
正常にビルド出来たらコンテナを起動し、パッケージを作成します。

Using a Dockerfile to build > lambci/docker-lambda - GitHub
command(bash@GuestOS)
# Create New Dockerfile for Build
$ cat <<EOF > Dockerfile 
FROM lambci/lambda:build-nodejs12.x
ENV LANG C.UTF-8
ENV AWS_DEFAULT_REGION ap-northeast-1

COPY . .

CMD npm install
CMD zip -r9 deploy_package.zip .
EOF

#
# Build Container
# --tag , -t        Name and optionally a tag in the ‘name:tag’ format
#
$ docker build \
  -t aws-lambda-nodejs12.x-deploy-package \
  .

# Create Deploy Package.
$ docker run --rm \
  -v "$PWD":/var/task \
  aws-lambda-nodejs12.x-deploy-package:latest

# 作成ファイルの確認
$ ls
deploy_package.zip  Dockerfile  index.js  package.json
4. Lambda Function の作成|更新
ここまでくれば、あとは CLI なり AWS Web Console なりで Lambda Function を作成すれば完成です。

おわりに
AWS Lambda の実行とパッケージングをスムーズにローカル環境で実行することが出来るので、開発効率が大きく改善しそうです。

