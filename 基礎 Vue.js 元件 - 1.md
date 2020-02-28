# 使用 x-template 建立元件

- 使用 x-template 建立表格元件
- 使用 is 掛載 template
- 使用 prop 傳遞資料
- 說明局部註冊及全域註冊

<br>

> 這章節會使用到的程式碼

```html
<div id="app">
  <table class="table">
    <thead></thead>
    <tbody>
      <tr v-for="(item, key) in data" :item="item" :key="key">
        <td>{{ item.name }}</td>
        <td>{{ item.cash }}</td>
        <td>{{ item.icash }}</td>
      </tr>
    </tbody>
  </table>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    data: [
      {
        name: '小明',
        cash: 100,
        icash: 500
      },
      {
        name: '杰倫',
        cash: 10000,
        icash: 5000
      },
      {
        name: '漂亮阿姨',
        cash: 500,
        icash: 500
      },
      {
        name: '老媽',
        cash: 10000,
        icash: 100
      }
    ]
  }
})
```

<br>

## 使用 x-template 建立表格元件

先定義出 **component 標籤名稱**

還有要綁定 **template** 使用 `id`

跟創建 Vue 的時候一樣會綁定 `el: '#app'`

```js
// 全域註冊

Vue.component('row-component', {
  template: '#rowComponentTemplate'
})
```

<br>

創建 `x-template` 並給予 `id`

將 `tr` 放入其中

```html
<script type="text/x-template" id="rowComponentTemplate">
  <tr>
    <td>{{ item.name }}</td>
    <td>{{ item.cash }}</td>
    <td>{{ item.icash }}</td>
  </tr>
</script>
```

<br>

將自定義的元件名稱使用在 HTML

```html
<row-component
  v-for="(item, key) in data"
  :key="key"
></row-component>
```

<br>

會發現跳出錯誤訊息無法識別出 `item`

    [Vue warn]: Property or method "item" is not defined on the instance but referenced during render.

<br>

打開 Vue Tool 會發現 `ROOT` 有資料，可是**元件**卻都沒有資料。

這是因為**元件**內資料與外層是不同的

所以我們要把 `ROOT` 的資料傳進**元件**裡面

使用 `props` 這個方法

```js
Vue.component('row-component', {
  template: '#rowComponentTemplate',
  props: ['person']
})
```

<br>

HTML 元件要加上 `:person="item"`

將 item 傳入元件的 person 裡面

```html
<row-component
  v-for="(item, key) in data"
  :key="key"
  :person="item"
></row-component>
```

<br>

所以這時候 `x-template` 的 `item` 要修改為 `person`

```html
<script type="text/x-template" id="rowComponentTemplate">
  <tr>
    <td>{{ person.name }}</td>
    <td>{{ person.cash }}</td>
    <td>{{ person.icash }}</td>
  </tr>
</script>
```

<br>

## 使用 is 掛載 template

資料出現在畫面了，可是卻不是我們要的。

打開**開發人員工具**查看 `Elements` 會發現，我們的 `tr` 不是在 `tbody` 裡面。

這個是 HTML 的特性就是 `tbody` 裡面就是放 `tr` 而 `tr` 裡面就是放 `td`

但是我們卻違反了這個規定，我們在 `tbody` 裡面放的是 `row-component`。

```html
<div id="app">
  <table class="table">
    <thead></thead>
    <tbody>
      <row-component
        v-for="(item, key) in data"
        :key="key"
        :person="item"
      ></row-component>
    </tbody>
  </table>
</div>
```

<br>

為了避免這個錯誤，我們可以使用 `is` 來掛載 `template`！

這個 `is` 是用來動態切換 template 使用的

這樣結構就正確了～

```html
<tr
  is="row-component"
  v-for="(item, key) in data"
  :key="key"
  :person="item"
></tr>
```

<br>

## 局部註冊

這 `Vue.component` 是全域註冊

所有的 Vue 應用程式都可以使用這個 `row-component`

```js
Vue.component('row-component', {
  template: '#rowComponentTemplate',
  props: ['person']
})
```

<br>

現在我們把它改寫為 **局部註冊**

這樣子其它的 Vue 應用程式是**不能**使用的

會出現這樣子的錯誤訊息

    [Vue warn]: Unknown custom element: <row-component> - did you register the component correctly?

```js
var child = {
  template: '#rowComponentTemplate',
  props: ['person']
}

var app = new Vue({
  el: '#app',
  data: {
    ...
  },
  components: {
    'row-component': child
  }
})
```

<br>

# 元件必須使用 function return

> 這章節會使用到的程式碼

```html
<div id="app">
  <counter-component></counter-component>
  <counter-component></counter-component>
  <counter-component></counter-component>
</div>
```

```js
Vue.component('counter-component', {
  template: '#counter-component',
  data: {
    counter: 0
  },
})

var app = new Vue({
  el: '#app'
})
```

```html
<script type="text/x-template" id="counter-component">
  <div>
    你已經點擊 <button class="btn btn-outline-secondary btn-sm" @click="counter += 1">{{ counter }}</button> 下。
  </div>
</script>
```

<br>

錯誤訊息

    [Vue warn]: The "data" option should be a function that returns a per-instance value in component definitions.

<br>

## 元件內的 data 一定要使用 function return

```js
Vue.component('counter-component', {
  data() {
    return {
      counter: 0
    }
  },
  template: '#counter-component'
})
```
