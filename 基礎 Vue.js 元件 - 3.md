# Props 的型別

> 這章節會使用到的程式碼

```html
<div id="app">

  <h2>Props 的型別</h2>
  <prop-type :cash="cash"></prop-type>

  <h2 class="mt-3">靜態與動態傳入數值差異</h2>
  <prop-type cash="300"></prop-type>

</div>

<script type="text/x-template" id="propType">
  <div>
    <input type="number" class="form-control" v-model="newCash">
    {{ typeof(cash)}}
  </div>
</script>
```

```js
Vue.component('prop-type', {
  template: '#propType',
  props: ['cash'],
  data() {
    return {
      newCash: this.cash
    }
  }
})

var app = new Vue({
  el: '#app',
  data: {
    cash: '300'
  }
})
```

<br>

## 怎麼在 Props 定義型別？

> 避免傳入錯誤的資料內容，幫助我們在開發時就提前知道！！！

```js
Vue.component('prop-type', {
  template: '#propType',
  data() {
    return {
      newCash: this.cash
    }
  },
  // 將 cash 定義為 Number
  props: {
    cash: {
      type: Number
    }
  },
})
```

<br>

Props 傳入的資料定義為 Number 後，會跳出錯誤訊息：

    [Vue warn]: 
    
    Invalid prop: type check failed for prop "cash". 
    
    Expected Number, got String.

<br>

## Props 帶有預設值

```html
<!-- 
  靜態與動態傳入數值差異 
  將 cash="300" 移除
-->
<prop-type></prop-type>
```

```js
props: {
  cash: {
    type: Number,
    default: 300
  }
}
```

<br>

## 靜態屬性

> 傳入的是 string

```html
<prop-type cash="300"></prop-type>
```

<br>

## 動態屬性

> 傳入的是 number

```html
<prop-type :cash="300"></prop-type>
```

<br>

# 向外層傳送事件 emit

> 這章節會使用到的程式碼

```html
<div id="app">

  我透過元件儲值了 {{ cash }} 元
  <button 
    class="btn btn-outline-primary" 
    @click="incrementTotal"
  >
    按我
  </button>

  <button-counter></button-counter>

</div>

<script type="text/x-template" id="buttonCounter">
  <div>
    <button @click="incrementCounter" class="btn btn-outline-primary">增加 {{ counter }} 元</button>
    <input type="number" class="form-control mt-2" v-model="counter">
  </div>
</script>
```

```js
Vue.component('button-counter', {
  template: '#buttonCounter',
  data() {
    return {
      counter: 1
    }
  },
  methods: {}
})

var app = new Vue({
  el: '#app',
  data: {
    cash: 300
  },
  methods: {
    incrementTotal() {
      this.cash ++
    }
  }
})
```

<br>

## 元件觸發外層 methods

自定義 `v-on:` 事件名稱

```html
<button-counter @increment="incrementTotal"></button-counter>
```

<br>

元件內觸發的方法 `incrementCounter` 將它添加在 `methods`

```html
<script type="text/x-template" id="buttonCounter">
  <div>
    <button @click="incrementCounter" class="btn btn-outline-primary">增加 {{ counter }} 元</button>
    <input type="number" class="form-control mt-2" v-model="counter">
  </div>
</script>
```

<br>

使用 `this.$emit('自定義 v-on: 事件名稱')`

```js
Vue.component('buttonCounter', {
  ...,
  methods: {
    incrementCounter() {
      this.$emit('increment')
    }
  }
})
```

<br>

## emit 傳遞參數

加上 `Number` 可以確保是數值，不會是 string ！

```js
Vue.component('buttonCounter', {
  ...,
  methods: {
    incrementCounter() {
      this.$emit('increment', Number(this.counter))
    }
  }
})
```

<br>

這樣子就可以拿到元件內 `counter` 值來增加外層的 `cash`

```js
var app = new Vue({
  el: '#app',
  data: {
    cash: 300
  },
  methods: {
    incrementTotal(newNumber) {
      this.cash = this.cash + newNumber
    }
  }
})
```
