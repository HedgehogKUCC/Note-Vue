# Vue 應用程式建立

## 1. 建立一個應用程式使資料呈現於畫面上

Vue 起手式

```js
var app = new Vue({
  // 物件內容都是選項的
})
```

打開 `Vue 的開發工具`

這時候因為還沒有綁定 `HTML 元素`

所以不會產生 `Root`

<br>

```html
<div id="app">

</div>
```

```js
var app = new Vue({
  // el 是 element 縮寫
  el: '#app'
})
```

這時候就會產生 `Root`，成功生成一個 Vue 的應用程式

<br>

接下來試著將資料呈現在畫面上

```html
<div id="app">
  {{ text }}
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    text: '這是一段話'
  }
})
```

<br>

### 額外補充

可以使用 `class` 綁定，效果是一樣的。

但是不管是用 `id` 還是 `class`，Vue 還是只能一次綁定一個元素。

所以使用 `id` 是比較好的！

```html
<div class="app">
  {{ text }}
</div>
<div class="app">
  {{ text }}
</div>
```

```js
var app = new Vue({
  el: '.app',
  data: {
    text: '這是一段話'
  }
})
```

<br>

## 2. 建立兩個應用程式

一個網頁上是可以放入 **兩個 Vue 的應用程式**

```html
<div id="app">
  {{ text }}
</div>
<div id="app2">
  {{ text }}
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    text: '這是一段話 1'
  }
});
var app2 = new Vue({
  el: '#app2',
  data: {
    text: '這是一段話 2'
  }
});
```

<br>

## 3. 試試看巢狀應用程式

這樣子的巢狀不會跳錯，但是會發現資料兩筆輸出的內容都是第一個 Vue 應用程式的資料。

```html
<div id="app">
  {{ text }}
  <div id="app2">
    {{ text }}
  </div>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    text: '這是一段話 1'
  }
});
var app2 = new Vue({
  el: '#app2',
  data: {
    text: '這是一段話 2'
  }
});
```

<br>

將 `text` 修改為 `text2`

這時候就會跳出錯誤

    vendor.js:600 [Vue warn]: Property or method "text2" is not defined on the instance but referenced during render.

這個是因為 Vue 的應用程式是不可以用巢狀方式去建立！

外面生成了，裡面就不可以建立其它 Vue 的應用程式。

所以巢狀裡面的 Vue 應用程式等同於無效！

```html
<div id="app">
  {{ text }}
  <div id="app2">
    {{ text2 }}
  </div>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    text: '這是一段話 1'
  }
});
var app2 = new Vue({
  el: '#app2',
  data: {
    text2: '這是一段話 2'
  }
});
```

<br>

# 雙向綁定的資料

Vue 不是完全的 **MVVM** 的架構，只是受到 **MVVM** 的啟發。

[MVVM](https://zh.wikipedia.org/wiki/MVVM) 是一種軟體架構模式。

<br>

## 1. 使用雙花括號與資料串接

```html
<div id="app">
  {{ message }}
  <input type="text" />
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: '哈囉'
  }
})
```

<br>

## 2. 試試看使用 HTML 屬性綁定

呈現資料還可以使用 `v-text`

```html
<div id="app">
  {{ message }}
  <div v-text="message"></div>
  <input type="text" />
</div>
```

<br>

除了 `v-text` 還有 `v-html`

這個可以插入 HTML 標籤

```html
<div id="app">
  {{ message }}
  <div v-html="message"></div>
  <input type="text" />
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: '<span>哈囉</span>'
  }
})
```

<br>

## 3. 試試看雙向綁定

在 `input` 使用 `v-model` 就可以直接調整 `message` 資料內容

```html
<div id="app">
  {{ message }}
  <input type="text" v-model="message" />
</div>
```

<br>

# 補充說明

**VUE JS** 是以 `資料狀態` 操作畫面

jQuery 是直接操作畫面上的 DOM 元素
