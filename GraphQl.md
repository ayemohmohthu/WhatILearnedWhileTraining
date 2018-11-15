### GraphQLとは
https://qiita.com/syu_chan_1005/items/3350f1d12c17a77e98c7

GraphQLはAPI向けの言語です。データの形式のみの定義のため, 言語やデータを保存する方法は依存しません。(要するにDBでもテキストでもいい)
GraphQLの定義に従ってクエリを書き, サーバーと通信を取ることでJSONになって戻ってきます。
### クエリとミューテーション
GraphQLには様々なデータの取得や編集に関するものがあります。
 クエリ : 取得
 ミューテーション：編集
### クエリ例
 Request
 {
  hero {
    name
    # Queries can have comments!
    friends {
      name
    }
  }
}
Response
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        { "name": "Luke Skywalker" },
        { "name": "Han Solo" },
        { "name": "Leia Organa" }
      ]
    }
  }
}
### Argumentを渡して条件として使うこともできる
### Aliases
### Fragments
### Variables
### Directives
### Mutations

#### graphql Installation
```

```
#### graphiQl IDE is used to run graphql query on browser
```

```

#### graphql Object Types 作成
GraphQLのTypesClassを作成するとき、全部のフィールドにOptions（Null制限など...）をつけなければなりません。
```
rails g graphql:object Task id:ID! name:String! is_done:integer!
```

#### graphQl queryで指定するフィールド名はKermelケースではないといけない。
```
例：model Todo の　column user_id を取得したいのであれば、userIdと指定する必要がある。
```

#### mutation query sample
```
mutation createTask($task:TaskInputType!) {
  createTask(task:$task) {
    id
    name
    status
    storyId
  }
}
```
#### Variables
```
{
  "task": {
    "id": 7,
    "name": "task7",
    "status": "new",
    "storyId": 2
  }
}
```

#### 日付型定義
```
Types::DateTimeType = GraphQL::ScalarType.define do
　　name 'DateTime'
　　coerce_input ->(value, _ctx) { Time.zone.parse(value) }
　　coerce_result ->(value, _ctx) { value.utc.strftime("%Y/%m/%d") }
end
```
