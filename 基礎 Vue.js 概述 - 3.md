# v-class 動態切換 className

## 為 .box 動態加上 className "rotate"

```html
<div id="app">
  <!-- <div class="box" :class="{ '要加入的 ClassName' : 判斷式 }"></div> -->
  <div class="box" :class="{ 'rotate' : isTransform }"></div>
  <hr>
  <button @click="isTransform = !isTransform">旋轉物件</button>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    isTransform: false
  }
})
```

```css
<style>
.box {
  transition: transform .5s;
}
.box.rotate {
  transform: rotate(45deg);
}
</style>
```

<br>

# 計算屬性

## 使用 computed 取代原有的表達式

`data` 內的資料更動時才會觸發 `computed`

```html
<div id="app">
  <input type="text" v-model="text" />
  <div>{{ text.split('').reverse().join('') }}</div>
  {{ reverseText }}
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    text: '',
    newText: ''
  },
  computed: {
    reverseText: function() {
      return this.text.split('').reverse().join('')
    }
  }
})
```

<br>

### 補充

`computed` 

是在監控資料更動後，重新運算結果呈現於畫面上，

一般來說**不會修改資料**，只會回傳用於**畫面呈現的資料**。

<br>

`methods`

就是互動的函式，需要觸發才會運作，會用來修改資料內容。

<br>

> 效能

如果資料量大，`computed` 自然會比較慢，只要資料變動就會觸發，無形之中執行次數也會增加。

因此在大量資料時，會建議透過 `methods` 減少不必要的運算！！！

<br>

# Vue 表單雙向綁定

```html
<div id="app">
  <h4>字串</h4>
  {{ text }}
  <input type="text" class="form-control" v-model="text" />
  <hr />
  <pre>{{ textarea }}</pre>
  <textarea
    cols="30"
    rows="3"
    class="form-control"
    v-model="textarea"
  ></textarea>
  <hr />
  <h4>Checkbox 與 Radio</h4>
  <div class="form-check">
    <input
      type="checkbox"
      class="form-check-input"
      id="check1"
      v-model="checkbox1"
    />
    <label class="form-check-label" for="check1">
      你要不要出去玩
    </label>
  </div>
  <hr />
  <div class="form-check">
    <input
      type="checkbox"
      class="form-check-input"
      id="check2"
      value="雞"
      v-model="checkboxArray"
    />
    <label class="form-check-label" for="check2">雞</label>
  </div>
  <div class="form-check">
    <input
      type="checkbox"
      class="form-check-input"
      id="check3"
      value="豬"
      v-model="checkboxArray"
    />
    <label class="form-check-label" for="check3">豬</label>
  </div>
  <div class="form-check">
    <input
      type="checkbox"
      class="form-check-input"
      id="check4"
      value="牛"
      v-model="checkboxArray"
    />
    <label class="form-check-label" for="check4">牛</label>
  </div>
  <p>
    晚餐火鍋裡有
    <span v-for="item in checkboxArray"> {{ item }}</span>。
  </p>
  <hr />
  <div class="form-check">
    <input
      type="radio"
      class="form-check-input"
      id="radio2"
      value="雞"
      v-model="singleRadio"
    />
    <label class="form-check-label" for="radio2">雞</label>
  </div>
  <div class="form-check">
    <input
      type="radio"
      class="form-check-input"
      id="radio3"
      value="豬"
      v-model="singleRadio"
    />
    <label class="form-check-label" for="radio3">豬</label>
  </div>
  <div class="form-check">
    <input
      type="radio"
      class="form-check-input"
      id="radio4"
      value="牛"
      v-model="singleRadio"
    />
    <label class="form-check-label" for="radio4">牛</label>
  </div>
  <p>晚餐火鍋裡有 {{ singleRadio }}。</p>
  <hr />
  <h4>Select</h4>
  <select name="" id="" class="form-control" v-model="selected">
    <option value="" disabled>-- 請選擇</option>
    <option value="小明">聰明的小明</option>
    <option value="小美">漂亮的小美</option>
  </select>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    text: '',
    textarea: '',
    checkbox1: false,
    checkboxArray: [],
    singleRadio: '',
    selected: ''
  }
})
```

<br>

# 元件化

## 將以下元素轉為元件

```html
<div id="#app">
  <div>
    你已經點擊 <button @click="counter += 1">{{ counter }}</button> 下。
    你已經點擊 <button @click="counter += 1">{{ counter }}</button> 下。
  </div>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    counter: 0
  }
})
```

<br>

轉換為元件後 `counter` 都是單獨的資料！

```html
<div id="#app">
  <div>
    你已經點擊 <button @click="counter += 1">{{ counter }}</button> 下。
    你已經點擊 <button @click="counter += 1">{{ counter }}</button> 下。
  </div>
  <counter-component></counter-component>
  <counter-component></counter-component>
  <counter-component></counter-component>
</div>
```

```js
Vue.component('counter-component', {
  data: function() {
    return {
      counter: 0
    }
  },
  template: `
    <div>
      你已經點擊 <button @click="counter += 1">{{ counter }}</button> 下。
    </div>
  `
})

var app = new Vue({
  el: '#app',
  data: {
    counter: 0
  }
})
```

<br>

# Vue.js Todo List

[Todo List](https://hedgehogkucc.github.io/vuetodolist/)

[筆記](https://github.com/HedgehogKUCC/vuetodolist/tree/master)
