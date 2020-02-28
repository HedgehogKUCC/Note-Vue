# v-bind 動態屬性指令

## 1. 透過指令 ( v-bind ) 的方式，將圖片加載於畫面之上。

```html
<div>
  <img v-bind:src="imgSrc" alt="" />
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    imgSrc: 'https://images.unsplash.com/photo-1479568933336-ea01829af8de?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=d9926ef56492b20aea8508ed32ec6030&auto=format&fit=crop&w=2250&q=80'
  }
})
```

<br>

試著加入 `className`

```js
var app = new Vue({
  el: '#app',
  data: {
    imgSrc: 'https://images.unsplash.com/photo-1479568933336-ea01829af8de?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=d9926ef56492b20aea8508ed32ec6030&auto=format&fit=crop&w=2250&q=80',
    className: 'img-fluid'
  }
})
```

```html
<div>
  <img v-bind:src="imgSrc" v-bind:class="className" alt="" />
</div>
```

<br>

# v-if 及 v-for

## 使用 v-for 來呈現資料列表

```js
var app = new Vue({
  el: '#app',
  data: {
    list: [
      {
        name: '小明',
        age: 16
      },
      {
        name: '媽媽',
        age: 38
      },
      {
        name: '漂亮阿姨',
        age: 24
      }
    ]
  }
})
```

```html
<div id="app">
  <pre>{{ list }}</pre>
  <ul>
    <li v-for="(item, index) in list">
      {{ index + 1 }} - {{ item.name }} 年齡是 {{ item.age }} 歲
    </li>
  </ul>
</div>
```

<br>

## 使用 v-if 擷取部分資訊

```html
<div id="app">
  <pre>{{ list }}</pre>
  <ul>
    <li v-for="(item, index) in list" v-if="item.age < 38">
      {{ index + 1 }} - {{ item.name }} 年齡是 {{ item.age }} 歲
    </li>
  </ul>
</div>
```

<br>

# 互動式行為 v-on

## 1. 使用 v-on 指令來製作互動行為

```html
<div id="app">
  <input type="text" v-model="text" />
  <button v-on:click="reverseText"></button>
  <div>{{ newText }}</div>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    text: '',
    newText: ''
  },
  methods: {
    reverseText: function() {
      console.log('Click Me')
      this.newText = this.text
    }
  }
})
```

<br>

## 2. 反轉字串

```js
var app = new Vue({
  el: '#app',
  data: {
    text: '',
    newText: ''
  },
  methods: {
    reverseText: function() {
      this.newText = this.text.split('').reverse().join('')
    }
  }
})
```

<br>

### 補充

一定要有預先定義資料的習慣！！！

<br>

# 修飾符 及 縮寫

## 1. 請將 button 改成 a 標籤，並加上 preventDefault()

```html
<div id="app">
  <input type="text" class="form-control" v-model="text" />
  <a href="#" class="btn btn-primary mt-1" v-on:click="reverseText">
    反轉字串
  </a>
  <div class="mt-3">
    {{ newText }}
  </div>
</div>
```

```js
<script>
  var app = new Vue({
    el: '#app',
    data: {
      text: '',
      newText: ''
    },
    methods: {
      reverseText: function(e) {
        e.preventDefault()
        this.newText = this.text
          .split('')
          .reverse()
          .join('')
      }
    }
  })
</script>
```

<br>

## 2. 使用修飾符取代

[Vue 事件修飾符](https://cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6)

```html
<div id="app">
  <input type="text" class="form-control" v-model="text" />
  <a href="#" class="btn btn-primary mt-1" v-on:click.prevent="reverseText">
    反轉字串
  </a>
  <div class="mt-3">
    {{ newText }}
  </div>
</div>
```

```js
<script>
  var app = new Vue({
    el: '#app',
    data: {
      text: '',
      newText: ''
    },
    methods: {
      reverseText: function(e) {
        // e.preventDefault()
        this.newText = this.text
          .split('')
          .reverse()
          .join('')
      }
    }
  })
</script>
```

<br>

## 3. 將 input 加上 Enter 事件

```html
<div id="app">
  <input 
    type="text"
    class="form-control"
    v-model="text"
    v-on:keyup.enter="reverseText"
  />
  <a 
    href="#" 
    class="btn btn-primary mt-1" 
    v-on:click.prevent="reverseText"
    >反轉字串</a
  >
  <div class="mt-3">
    {{ newText }}
  </div>
</div>
```

<br>

## 4. 將範例改成使用縮寫表示

```html
<div id="app">
  <input 
    type="text"
    class="form-control"
    v-model="text"
    @keyup.enter="reverseText"
  />
  <a 
    href="#" 
    class="btn btn-primary mt-1" 
    @click.prevent="reverseText"
    >反轉字串</a
  >
  <div class="mt-3">
    {{ newText }}
  </div>
</div>
```

`v-on` 縮寫為 `@`

`v-bind:` 縮寫為 `:`
