---
title: yarnを使ってみた
tags: YARN
author: masterkey1009
slide: false
---
最近よく耳にするようになったyarnとやらを使ってみたので、その忘備録として。

## yarnとは
FacebookとExponent、Google、Tildeとの共同チームによって生まれた新しいパッケージマネージャーらしい。(bowerからnpmに変わって次はこれか...)しかーし、npmと互換性があるので、既存のpackage.jsonはそのまま使える。

### Lockfile
ユーザーやデバイス間でライブラリのバージョン違いを無くす、npmの`shrinkwrp`に相当する機能。

### Offline
キャッシュシステムで、パッケージのインストールにかかる時間を大幅に減らして、オフラインでも使用できるとのこと。   

## インストール

```
$ npm install -g yarn
```

Macでhomebrewを使っているなら

```
$ brew update
$ brew install yarn
```

で、

` .base_profile` にPATHを通す。

```
export PATH="$HOME/.yarn/bin:$PATH"
```

## コマンド

| npm | yarn |
|:-----------|:------------|
| npm install | yarn install |
| (N/A) | yarn install --flat |
| (N/A) | yarn install -har |
| (N/A) | yarn install -no-lockfile |
| (N/A) | yarn install --pure-lockfile |
| npm install [package] | (N/A) |
| npm install --save [package] | yarn add [package] |
| npm install --save-dev | yarn add [package] --dev |
| (N/A) | yarn add [package] --peer |
| npm install --save-optional [package] | yarn add [package] --optional |
| npm install --save-exact [package] | yarn add [package] --exact |
| (N/A) | yarn add [package] --exact |
| (N/A) | yarn add [package] --tilde |
| npm install --global [package] | yarn global add [package] |
| npm uninstall [package] | (N/A) |
| npm uninstall --save [package] | yarn remove [package] |
| npm uninstall --save-dev [package] | yarn remove [package] |
| (N/A) | yarn upgrade [package] |


optionについてはまだ調べきれていないけど、基本的には`yarn add` or `yarn add -D`や`yarn remove`とかを使っていく事になりそう。  

