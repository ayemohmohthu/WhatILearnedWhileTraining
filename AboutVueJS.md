#### よく見える単語
data
mounted : mountedはVueインスタンスがマウントされたタイミングで実行されるライフサイクルフックです。
created : createdもあって今回の場合、あまり違いはありませんが、[https://jp.vuejs.org/v2/guide/instance.html#%E3%83%A9%E3%82%A4%E3%83%95%E3%82%B5%E3%82%A4%E3%82%AF%E3%83%AB%E3%83%80%E3%82%A4%E3%82%A2%E3%82%B0%E3%83%A9%E3%83%A0]については、ちゃんと理解する必要がありそうです。
Vue-Router : Vue-Routerを使用することで、登録されたパスとコンポーネントで画面内を差し替えることができます。
axios : axiosは、Ajax通信ライブラリです。
  yarn add axios


### 単一ファイルコンポーネント
.vue ファイルに <template> タグを埋め込んでその中にHTMLを書き込む  
##### my-component.vue
```
<!-- 登録する -->
<template>
  <div>A custom component!</div>
</template>
```
##### app.js
components で使いたいコンポーネントのオブジェクトを指定して、 template でコンポーネントと置き換えるタグ（のようなもの）を指定する。
もちろん、 my-component.vue は読み込む必要がありますので、 import で読み込みます。
```
import MyComponent from './my-component.vue';

// root インスタンスを作成する
new Vue({
  el: '#example',
  components: { MyComponent },
  template: '<my-component></my-component>'
})
```
##### index.html.erb

HTMLは、 root要素に id="example" をつける。  
その中のコンポーネントを挿入したいところに、呼び出すコンポーネントの名前と同じタグ（のようなもの） <my-component> を記述する。  
そして app.js を読み込む。
```
<div id="example">
  <my-component></my-component>
</div>

<script src="./app.js"></script>
```
#### Drag&Drop参考
```
http://www.nct-inc.jp/engineer_blog/2599/
```

#### routerを使いたいときは
```
npm install --save vue-router  
```

### vue-routerでテンプレートの差替え
https://github.com/eiKatou/Sample/tree/a897a499404dee6a60af0ddb44d37338396b75f1/Vue.js/todo_vue

##### JavaScript(main.js)でrouter.jsをimportしつつ、親のテンプレートを表示するように設定。
```
import Vue from 'vue/dist/vue.esm.js'
import App from './App'
import router from './router'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  store,
  router,
  template: '<App/>',
  components: { App }
})
```
##### 親のてプレート(app.vue)で
```
<template>
  <div>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
}
```
##### router.jsで
```
import Vue from 'vue/dist/vue.esm.js'
import Router from 'vue-router'
import Top from './components/Top'
import Edit from './components/Edit'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'top',
      component: Top
    },
    {
      path: '/edit/:index',
      name: 'edit',
      component: Edit
    }
  ]
})
```
##### 子テンプレート１：top.vueで
```
<template>
<div>
<h1>TODO一覧</h1>
  <div id="todo_list">
    <table id="todo_table">
    <template v-for="(item, index) in todos" >
      <tr class="todo_row">
        <td class="todo">{{ item }}</td>
        <td><router-link :to="{ name:'edit', params:{index:index} }">edit</router-link></td>
      </tr>
    </template>
    </table>
  </div>
</template>
<script>
export default {
  name: 'top'
</script>
<style scoped>
</style>
```
##### 子テンプレート２：edit.vueで
```
<template>
  <div>
    <h1>TODOの編集</h1>
  </div>
</template>
<script>
export default {
  name: 'edit'
</script>
<style scoped>
</style>
```

#### Styling with Inline CSS Styles in Vue.js
```
<div style="width: 200px; height: 200px;" v-bind:style="{ 'background-color': ‘red’ }"></div>  
or  
<div style="width: 200px; height: 200px;" v-bind:style="{ 'background-color': color }"></div>
<button v-on:click="changeColor">Change Color</button>
<script>
data: {
	color: 'blue'
},
methods: {
	changeColor: function() {
		if (this.color == 'blue') {
			this.color = 'red';
		} else {
			this.color = 'blue';
		}
	}
}
</script>

or

<div v-bind:style="styles"></div>
data: {
  styles: {
	  'background-color': 'blue',
	  width: '200px',
	  height: '200px'
  }
}
```
#### Vue component に<style>を使いたい場合
html viewでjavascriptをimportするとき、stylesheetもimportする必要がある。
```
<%= javascript_pack_tag 'task_vue' %>
#追加分
<%= stylesheet_pack_tag 'task_vue' %>
```
