# Computed 與 Watch

> 這章節會使用到的程式碼

```html
<div id="app">

  <h4>Computed</h4>
  <p>使用 Computed 來過濾資料。</p>
  <input type="text" class="form-control" v-model="filterText">
  <ul>
    <li v-for="(item, key) in arrayData" :key="item.age">
      {{ key }} - {{ item.name }} {{ item.age }} 歲 <input type="text">
    </li>
  </ul> 

  <p>使用 Computed 來呈現時間格式。</p>
  <p></p>

  <h4>Watch</h4>
  <p>使用 trigger 來觸發旋轉 box、並在三秒後改變回來</p>
  <div class="box" :class="{'rotate': trigger }"></div>

  <hr>

  <button class="btn btn-outline-primary">旋轉</button>

</div>
```

```js
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
    filterText: '',
    trigger: false,
    newDate: 0
  }
})
```

```css
.box {
  transition: all 0.5s;
}

.box.rotate {
  transform: rotate(45deg);
}
```
<br>

## Computed

> 使用 Computed 來過濾資料。

```html
<!-- 將 arrayData 改為 filterArray -->
<input type="text" class="form-control" v-model="filterText">

<ul>
  <li v-for="(item, key) in filterArray" :key="item.age">
    {{ key }} - {{ item.name }} {{ item.age }} 歲 <input type="text">
  </li>
</ul> 
```

```js
/* 
  使用 computed 就不需要在 @keyup.enter
  但是當資料量大的時候，效能就會開始產生影響！
*/
computed: {
  filterArray() {
    const vm = this
    return vm.arrayData.filter(item => item.name.match(vm.filterText.trim()))
  }
}
```

<br>

## 使用 Computed 來呈現時間格式

```html
<p>{{ formatTime }}</p>
```

```js
computed: {
  formatTime() {
    let dates = new Date(this.newDate)
    let year = dates.getFullYear()
    let month = dates.getMonth() + 1
    let date = dates.getDate()
    let hours = dates.getHours()
    let minutes = dates.getMinutes()
    let seconds = dates.getSeconds()
    return `${year}/${month}/${date} ${hours}:${minutes}:${seconds}`
  }
},
mounted() {
  this.newDate = Math.floor(Date.now())
}
```

<br>

## Watch

> 使用 trigger 來觸發旋轉 box 並在三秒後改變回來

```html
<div class="box" :class="{'rotate': trigger }"></div>

<hr>

<button class="btn btn-outline-primary" @click="trigger = true">旋轉</button>
```

```js
watch: {
  trigger() {
    const vm = this
    setTimeout(() => {
      vm.trigger = false
    }, 3000)
  }
}
```

<br>

# 表單補充介紹

> 這章節會使用到的程式碼

```html
<div id="app">

  <h4>Select</h4>
  <select name="" id="" class="form-control" v-model="selected">
    <option disabled value="">請選擇</option>
    <option value="小美">小美</option>
    <option value="可愛小妞">可愛小妞</option>
    <option value="漂亮阿姨">漂亮阿姨</option>
  </select>
  <p>小明喜歡的女生是 {{ selected }}。</p>

  <hr>

  <select name="" id="" class="form-control" v-model="selected2">
    <option disabled value="">請選擇</option>
  </select>
  <p>小明喜歡的女生是 {{ selected2 }}。</p>

  <hr>

  <h4 class="mt-3">多選</h4>
  <select name="" id="" class="form-control" v-model="multiSelected">
    <option value="小美">小美</option>
    <option value="可愛小妞">可愛小妞</option>
    <option value="漂亮阿姨">漂亮阿姨</option>
  </select>
  <p>小明喜歡的女生是 <span v-for="item in multiSelected">{{ item }} </span>。</p>

  <hr>

  <h4 class="mt-3">複選框</h4>
  <div class="form-check">
    <input type="checkbox" class="form-check-input" id="sex" v-model="sex">
    <label class="form-check-label" for="sex">{{ sex }}</label>
  </div>

  <h4 class="mt-3">修飾符</h4>
  {{ lazyMsg }}
  <input type="text" class="form-control" v-model="lazyMsg">
  <br>
  <pre>{{ age }}</pre>
  <input type="number" class="form-control" v-model="age">
  <br>
  {{ trimMsg }}緊黏的文字
  <input type="text" class="form-control" v-model="trimMsg">

</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    singleRadio: '',
    selected: '',
    selectData: ['小美', '可愛小妞', '漂亮阿姨'],
    selected2: '',
    multiSelected: [],
    sex: '男生',

    // 修飾符
    lazyMsg: '',
    age: '',
    trimMsg: ''
  }
})
```

<br>

## select 搭配 v-for

```html
<!-- 
  因為是動態屬性所以 value 要改為 :value 
  可以在 Vue Tool 觀察其差異
-->
<select name="" id="" class="form-control" v-model="selected2">
  <option disabled value="">請選擇</option>
  <option :value="item" v-for="item in selectData" :key="item">
    {{ item }}
  </option>
</select>

<p>小明喜歡的女生是 {{ selected2 }}。</p>
```

<br>

## select 改為多選

**MAC** 使用 `shift` 或 `cmd`

**WINDOWS** 使用 `shift` 或 `ctrl`

```html
<!-- multiple -->
<select
  name=""
  id=""
  class="form-control"
  v-model="multiSelected"
  multiple
>
  <option value="小美">小美</option>
  <option value="可愛小妞">可愛小妞</option>
  <option value="漂亮阿姨">漂亮阿姨</option>
</select>
<p>
  小明喜歡的女生是
  <span v-for="item in multiSelected">{{ item }} </span>。
</p>
```

<br>

## checkbox

```html
<!-- true-value、false-value -->
<div class="form-check">
<input
  type="checkbox"
  class="form-check-input"
  id="sex"
  v-model="sex"
  true-value="男生"
  false-value="女生"
/>
<label class="form-check-label" for="sex">{{ sex }}</label>
```

<br>

## 修飾符

```html
<!-- lazy：轉變為 change 事件 -->
<input type="text" class="form-control" v-model.lazy="lazyMsg" />
{{ lazyMsg }}

<!--
  即使在 type="number" 時，HTML 元素所返回的還是 string。
  在 v-model 添加 .number，可將值轉為 number。
-->
<input type="number" class="form-control" v-model.number="age" />
<pre>{{ typeof(age) }} {{ age }}</pre>

<!-- trim：自動過濾 '首'、'尾' 空白字符 -->
<input type="text" class="form-control" v-model.trim="trimMsg" />
{{ trimMsg }}緊黏的文字
```
