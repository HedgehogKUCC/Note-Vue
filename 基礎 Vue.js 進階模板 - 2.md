# v-for 與其使用細節

> 這章節會使用到的程式碼

```html
<div id="app">

  <h4>陣列與物件的迴圈</h4>
  <p>請使用 v-for 在陣列與物件上，並且加上索引</p>
  <ul>
    <!-- 陣列的迴圈 -->
    <li v-for="(item, key) in arrayData">
      {{ key }} - {{ item.name }} {{ item.age }} 歲
    </li>
  </ul>
  <ul>
    <!-- 請使用物件的迴圈 -->
  </ul>

  <hr>

  <h4>Key</h4>
  <p>請在範例上補上 key，並觀察其差異</p>
  <ul>
    <li v-for="(item, key) in arrayData">
      {{ key }} - {{ item.name }} {{ item.age }} 歲 <input type="text">
    </li>
  </ul>
  <button class="btn btn-outline-primary">反轉陣列</button>

  <h4>Filter</h4>
  <p>請製作過濾資料</p>
  <input type="text" class="form-control" v-model="filterText">
  <ul>
    <li v-for="(item, key) in arrayData" :key="item.age">
      {{ key }} - {{ item.name }} {{ item.age }} 歲 <input type="text">
    </li>
  </ul>

  <h4>不能運作的狀況</h4>
  <p>講師說明</p>
  <button class="btn btn-outline-primary">無法運行</button>
  <ul>
    <li v-for="(item, key) in arrayData" :key="item.age">
      {{ key }} - {{ item.name }} {{ item.age }} 歲 <input type="text">
    </li>
  </ul>

  <h4>純數字的迴圈</h4>
  <ul>
    <li v-for="item in 10">
      {{ item }}
    </li>
  </ul>

  <h4>Template 的運用</h4>
  <p>請將兩個 tr 一組使用 v-for</p>
  <table class="table">
    
  </table>

  <h4>v-for 與 v-if</h4>
  <p>請加上 v-if 判斷式</p>
  <ul>
    <li v-for="(item, key) in arrayData">
      {{ key }} - {{ item.name }} {{ item.age }} 歲
    </li>
  </ul>

  <h4>v-for 與 元件</h4>
  <p>講師說明</p>
  <ul>
    <list-item :item="item" v-for="(item, key) in arrayData"></list-item>
  </ul>

</div>
```

```js
Vue.component('list-item', {
  template: `
    <li>
      {{ item.name }} {{ item.age }} 歲
    </li>
  `,
  props: ['item']
});

var app = new Vue({
  el: '#app',
  data: {
    arrayData: [
      {
        name: '小明',
        age: 16
      },
      {
        name: '漂亮阿姨',
        age: 24
      },
      {
        name: '杰倫',
        age: 20
      }
    ],
    objectData: {
      ming: {
        name: '小明',
        age: 16
      },
      auntie: {
        name: '漂亮阿姨',
        age: 24
      },
      jay: {
        name: '杰倫',
        age: 20
      }
    },
    filterArray: [],
    filterText: ''
  },
  methods: {
    // 請在此練習 JavaScript
  },
});
```

<br>

## 陣列與物件的迴圈

> 使用 v-for 在陣列與物件上並且加上索引，觀察不同之處。

```html
<!-- 陣列的索引會是 0、1、2 -->
<ul>
  <li v-for="(item, key) in arrayData">
    {{ key }} - {{ item.name }} {{ item.age }} 歲
  </li>
</ul>

<!-- 物件的索引會是 '物件的屬性': ming、auntie、jay -->
<ul>
  <li v-for="(item, key) in objectData">
    {{ key }} - {{ item.name }} {{ item.age }} 歲
  </li>
</ul>
```

<br>

## Key

> 請在範例上補上 key，並觀察其差異。

```html
<!-- :key="唯一值" 這邊會用 item.age 是因為資料中的 age 還沒有重複的資料 -->
<!-- 加上 :key 後，input 所輸入的值也會跟著反轉。 -->
<ul>
  <li v-for="(item, key) in arrayData" :key="item.age">
    {{ key }} - {{ item.name }} {{ item.age }} 歲
    <input type="text" />
  </li>
</ul>

<button class="btn btn-outline-primary" @click="reverseArray">
  反轉陣列
</button>
```

```js
methods: {
  reverseArray () {
    this.arrayData.reverse()
  }
}
```

<br>

## Filter

> 請製作過濾資料

```html
<!--
  1. 先將 arrayData 改成 filterArray
  2. input 綁定 v-model="filterText"、@keyup.enter="filterData"
-->
<input
  type="text"
  class="form-control"
  v-model="filterText"
  @keyup.enter="filterData"
/>
<ul>
  <li v-for="(item, key) in filterArray" :key="item.age">
    {{ key }} - {{ item.name }} {{ item.age }} 歲
    <input type="text" />
  </li>
</ul>
```

```js
/*
  可以先使用 
  console.log(this.filterText, item.name, item.name.match(this.filterText.trim())) 
  來觀察
*/
methods: {
  filterData() {
    this.filterArray = this.arrayData.filter(item => item.name.match(this.filterText.trim()))
  }
}
```

<br>

## 不能運作的狀況

```html
<button class="btn btn-outline-primary" @click="cantWork">
  無法運行
</button>
<ul>
  <li v-for="(item, key) in arrayData" :key="item.age">
    {{ key }} - {{ item.name }} {{ item.age }} 歲
    <input type="text" />
  </li>
</ul>
```

```js
methods: {
  cantWork() {

    /* 
      第一個狀況
      console 和 VueTool 資料都沒有了
      可是畫面卻還是有資料顯示
    */
    this.arrayData.length = 0

    /*
      第二個狀況
      在 console 觀察下可以發現沒有 set、get
    */
    this.arrayData[0] = {
      name: '小強',
      age: 99
    }

    // 正確解法
    Vue.set(this.arrayData, 0, {
      name: '小強',
      age: 99
    })
    console.log(this.arrayData)
  }
}
```

[Vue.set](https://vuejs.org/v2/api/#Vue-set) ( target, propertyName/index, value )

<br>

## 純數字的迴圈

```html
<ul>
  <li v-for="item in 10">
    {{ item }}
  </li>
</ul>
```

<br>

## Template 的運用

> 請將兩個 tr 一組使用 v-for

```html
<!-- template 不會被渲染至畫面 -->
<table class="table">
  <template v-for="item in arrayData">
    <tr>
      <td>{{ item.name }}</td>
    </tr>
    <tr>
      <td>{{ item.age }}</td>
    </tr>
  </template>  
</table>
```

<br>

## v-for 與 v-if

> 請加上 v-if 判斷式

```html
<!-- 會先執行 v-for 在來 v-if -->
<ul>
  <li v-for="(item, key) in arrayData" v-if="item.age <= 19">
    {{ key }} - {{ item.name }} {{ item.age }} 歲
  </li>
</ul>
```

<br>

## v-for 與 元件

```html
<!-- :key -->
<ul>
  <list-item
    :item="item"
    v-for="(item, key) in arrayData"
    :key="item.age"
  ></list-item>
</ul>
```

```js
Vue.component('list-item', {
  template: `
    <li>
      {{ item.name }} {{ item.age }} 歲
    </li>
  `,
  props: ['item']
});
```

注意：現在元件使用 v-for 都必須加上 key。 [參考](https://cn.vuejs.org/v2/guide/list.html#%E4%B8%80%E4%B8%AA%E7%BB%84%E4%BB%B6%E7%9A%84-v-for)
