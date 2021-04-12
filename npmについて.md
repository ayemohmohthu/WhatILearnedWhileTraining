---
title: npm入門
tags: npm 入門
author: maitake9116
slide: false
---
## 概要

### npmとは？

Node Package Managerの略で、Node.jsのモジュールを管理するツールです。  

### package.json

npmはフロントエンドで使用するパッケージのバージョン管理を行うのに用いられます。
npmでは、npmでインストールしたパッケージのバージョン情報をpackage.jsonに格納します。
このpackage.jsonからパッケージを一括でインストールすることが出来るため、package.jsonを一元管理することによりフロントエンドで使用するパッケージのバージョン管理を一元化することが出来ます。

### インストールの種類

npmではパッケージをインストールする方法が、グローバルインストールとローカルインストールの2種類存在します。
グローバルインストールではnpmのルート配下にあるnode_modulesにパッケージがインストールされます。
Node.jsインストール時にNode.jsの実行モジュールのインストール先にパスが通されているため、グローバルインストールされたパッケージを全てのプロジェクトで使用することが出来ます。
ローカルインストールではプロジェクトのルート配下にあるnode_modulesにパッケージがインストールされます。 ローカルインストールされたパッケージは対象のプロジェクトのみで使用することが出来ます。

## 導入

Node.jsをインストールすると同時にnpmもインストールされます。  

## 使用方法

### バージョン確認

下記コマンドで、npmのバージョンを確認出来ます。

``` cmd
npm -–version
```

### 初期化

プロジェクトのルートディレクトリで下記コマンドを実行します。

``` cmd
npm init
```

これにより、プロジェクトのルートディレクトリ配下にpackage.jsonが作成されます。

### パッケージの復元

プロジェクトのルートディレクトリにパッケージ情報が記載されたpackage.jsonを置き、下記コマンドを実行します。

``` cmd
npm install
```

これにより、package.jsonに記載されたパッケージが一括でインストールされます。

### パッケージのインストール

グローバルインストールする場合は下記コマンドを実行します。

``` cmd
npm install –g typings
```

ローカルインストールする場合はプロジェクトのルート配下で下記コマンドを実行します。

``` cmd
npm install jquery
```

--saveオプションを付与するとpackage.jsonのdependenciesに依存関係が追記されます。
dependenciesには公開する際に必要なパッケージを記録します。

``` cmd
npm install --save jquery
```

--save-devオプションを付与するとpackage.jsonのdevDependenciesに依存関係が追記されます。
devDependenciesには開発時に必要なパッケージを記録します（テストツールやタスクランナー等）。

``` cmd
npm install --save-dev jquery
```

バージョンを指定してインストールする場合はパッケージ名の後にバージョン名を指定します。

``` cmd
npm install jquery@1.1.0 
```

### パッケージのバージョン確認

リリースされたパッケージのバージョン一覧を下記コマンドで取得出来ます。

``` cmd
npm info jquery versions
```

ローカルインストール済みのパッケージのバージョン一覧は下記コマンドで確認することが出来ます。

``` cmd
npm list --depth=0
```

グローバルインストール済みのパッケージのバージョン一覧は下記コマンドで確認することが出来ます。

``` cmd
npm list --depth=0 -g
```

### パッケージの更新

下記コマンドで未更新のパッケージを確認出来ます。

``` cmd
npm outdated
```

下記コマンドでpackage.jsonに記載されているパッケージのバージョンに更新されます。

``` cmd
npm update
```

ただし、最新のバージョンを使用するには、package.jsonに記載されているパッケージのバージョンを更新する必要があります。
これを手動で一つ一つ最新か確認するのは大変です。
そこで、npm-check-updatesを使用します。
npm-check-updatesをグローバルインストールします。

``` cmd
npm install -g npm-check-updates
```

下記コマンドでpackage.jsonのパッケージのバージョンを一括で最新にすることが出来るようになります。

``` cmd
npm-check-updates -u
```

この後にnpm updateを実行することで、一括で最新にすることが出来ます。


### パッケージのアンインストール

下記コマンドで、グローバルインストールしたパッケージをアンインストールすることが出来ます。

``` cmd
npm uninstall -g jquery
```

ローカルインストールしたパッケージをアンインストールする場合は下記コマンドを実行します。

``` cmd
npm uninstall jquery
```

package.jsonのdependenciesから依存関係を削除する場合は--saveオプションを付与します。

``` cmd
npm uninstall --save jquery
```

package.jsonのdevDependenciesから依存関係を削除する場合は--save-devオプションを付与します。

``` cmd
npm uninstall --save-dev jquery
```

