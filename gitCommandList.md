---
title: Gitコマンド一覧
tags: Git
author: fukumone
slide: false
---
#作業ディレクトリにGitを使う宣言をする

```
>> git init
```

#ステージングエリアにあげる

```
>>  git add (ファイル名) or git add .
```

#コミットする

```
>> git commit "コミット名"
```

#コミットメッセージを１行以内に納める

```
>> git commit -m "コミット名"
```

#一つ前のコミットと統合する

```
>> git commit --amend -m "コミット名"
```

#gitのログを確認する

```
>> git log
```

#Gitのlogを簡潔にまとめて表示する

```
>> git log --oneline
```

#オプション
git log --oneline
git log -p(変更した場所を見たい場合)
git log --stat(より詳しく変更した場所を見たい場合)

#現在の状態

```
>> git status
```

#前の状態に戻る

```
>> git checkout --(file名)
```

#何処を編集したのか知りたい場合

```
>> git diff
```

#ステージングエリアにあげた場合、コミットで変更されるファイルが分かる

```
>> git diff --cached
```

#コミットした後の編集されたファイルが分かる

```
>> git diff -r (ID名)
```

#git addを取り消す

```
>> git reset HEAD または　git reset HEAD　(ファイル名)
```

#直前に戻る

```
>> git reset --hard HEAD
```

#１つ前に戻る

```
git reset --hard HEAD^
```

#指定されたlogに戻る

```
>> git reset --hard　(ID)
```
#マージを始めた頃のブランチに戻る

```
>> git reset --hard ORIG_HEAD
```

#git 修正したファイルの変更を取り消す事はできるけど、特定のバージョンに戻したい場合

```
>> git checkout コミット ファイル
```

#ブランチの一覧表示

```
>> git branch
```

#ブランチを作る

```
>> git branch
```

#ブランチを削除

```
>> git branch -d (ブランチ名)
```

#リポジトリの複製

```
>> git clone git@(アドレス名)
```

#新規ブランチを作りそれをカレントブランチにする

```
>> git checkout -b (ブランチ名)
```

#リモートリポジトリにPushする前に登録するコマンド

```
>> git remote add origin(リモートリポジトリ名) git@~
```

#リモートリポジトリ削除

```
>> git remote rm origin
```

#ローカルリポジトリをリモートリポジトリに同期する

```
>> git fetch origin
```

#リモートブランチと同期したデータ、追跡ブランチをローカルリポジトリに取り込む

```
>> git merge origin / (ブランチ名)
```

#mergeとfetchをまとめて行う

```
>> git pull origin  (ブランチ名)
```

#ローカルブランチのデータをリモートブランチに送る（最新のもの）

```
>> git push origin  (ブランチ名)
```

#他のブランチを現在のカレントブランチに取り込む

```
>> git merge (branch name)
```

#rebaseをする(履歴を綺麗にする、まとめてコミットを取り込む)

```
>> git rebase (branch name)
```

#rebaseインタラクティブモード

```
# intの所にはなにか任意の数字を入れる
>> git rebase -i HEAD~int
```

#cherry-pickをする(直接コミットを取り込みたいとき)

```
>> git cherry-pick (コミットID)
# コミットIDはgit logで確認
```

#git の履歴を削除する

```
>> git filter-branch -f --index-filter 'git rm --ignore-unmatch filename' HEAD
#  -rfを付けるとディレクトリを削除

>> git filter-branch -f --index-filter 'git rm -rf --ignore-unmatch dirname' HEAD

```

#名前の登録

```
>> config --global user.name ”(your name)"
```

#メールの登録

```
>> config --global user.email "(your email)"
```

#メッセージの色分け

```
>> config --global color.ui true
```

#設定一覧

```
>> config -l
```

#ヘルプを見る

```
>> config --help
>> help config
```


#git省略コマンド
参考ページ
http://tobysoft.net/wiki/index.php?git%2F%A5%B3%A5%DE%A5%F3%A5%C9%A4%CE%BE%CA%CE%AC(alias)%C0%DF%C4%EA%A4%F2%A4%B9%A4%EB%CA%FD%CB%A1

git config --global alias.co checkout
git config --global alias.st 'status'
git config --global alias.ci 'commit -a'
git config --global alias.di 'diff'
git config --global alias.br 'branch'

.bashprofile,bashrcなどに記入
alias gco="git checkout"
alias gst="git status"
alias gci="git commit -m"
alias gdi="git diff"
alias gbr="git branch"

