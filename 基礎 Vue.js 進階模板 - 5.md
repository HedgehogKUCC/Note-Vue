# v-on 事件綁定

> 這章節會使用到的程式碼

```html
<div id="app">

  <p>請切換下方 box 的 className</p>
  <div class="box" :class="{'rotate': isRotate }"></div>

  <hr>

  <button class="btn btn-outline-primary">切換 box 樣式</button>

  <hr>

  <h4>帶入參數</h4>
  <ul>
    <li v-for="item in arrayData" class="my-2">
      {{ item.name }} 有 {{ item.cash }} 元 
      <button class="btn btn-sm btn-outline-primary">儲值</button>
    </li>
  </ul>

  <h4>修飾符</h4>
  <h5>事件修飾符</h5>
  <ul>
    <li>.stop - 調用 event.stopPropagation()。</li>
    <li>.prevent - 調用 event.preventDefault()。</li>
    <li>.capture - 添加事件偵聽器時使用 capture 模式。</li>
    <li>.self - 只當事件是從偵聽器綁定的元素本身觸發時才觸發回調。</li>
    <li>.once - 只觸發一次回調。</li>
  </ul>

  <h6>將此範例加上 stopPropagation</h6>
  <div class="p-3 bg-primary" @click="trigger('div')">
    <span class="box" @click="trigger('box')"></span>
  </div>

  <h6 class="mt-3">事件偵聽器時使用 capture 模式</h6>
  <div class="p-3 bg-primary" @click="trigger('div')">
    <span class="box d-flex align-items-center justify-content-center" @click="trigger('box')">
      <button class="btn btn-outline-secondary" @click="trigger('button')">按我</button>
    </span>
  </div>

  <h6 class="mt-3">事件偵聽器時使用 self 模式</h6>
  <div class="p-3 bg-primary" @click="trigger('div')">
    <span class="box d-flex align-items-center justify-content-center" @click="trigger('box')">
      <button class="btn btn-outline-secondary" @click="trigger('button')">按我</button>
    </span>
  </div>

  <h6 class="mt-3">事件偵聽器只觸發一次</h6>
  <div class="p-3 bg-primary" @click="trigger('div')">
    <span class="box d-flex align-items-center justify-content-center" @click="trigger('box')">
      <button class="btn btn-outline-secondary" @click="trigger('button')">按我</button>
    </span>
  </div>

  <h5>按鍵修飾符</h5>
  <ul>
    <li>.{keyCode | keyAlias} - 只當事件是從特定鍵觸發時才觸發回調。</li>
    <li>別名修飾 - .enter, .tab, .delete, .esc, .space, .up, .down, .left, .right</li>
    <li>修飾符來實現僅在按下相應按鍵時才觸發鼠標或鍵盤事件的監聽器 - .ctrl, .alt, .shift, .meta</li>
  </ul>

  <h6 class="mt-3">keyCode</h6>
  <input type="text" class="form-control" v-model="text" @keyup="trigger(13)">

  <h6 class="mt-3">別名修飾</h6>
  <input type="text" class="form-control" v-model="text" @keyup="trigger('space')">

  <h6 class="mt-3">相應按鍵時才觸發的監聽器</h6>
  <input type="text" class="form-control" v-model="text" @keyup="trigger('shift + Enter')">

  <h5>滑鼠修飾符</h5>
  <ul>
    <li>.left - (2.2.0) 只當點擊鼠標左鍵時觸發。</li>
    <li>.right - (2.2.0) 只當點擊鼠標右鍵時觸發。</li>
    <li>.middle - (2.2.0) 只當點擊鼠標中鍵時觸發。</li>
  </ul>
  <h6 class="mt-3">滑鼠修飾符</h6>
  <div class="p-3 bg-primary">
    <span class="box" @click="trigger('Right button')">
    </span>
  </div>

</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    arrayData: [
      {
        name: '小明',
        age: 16,
        cash: 500
      },
      {
        name: '漂亮阿姨',
        age: 24,
        cash: 1000
      },
      {
        name: '杰倫',
        age: 20,
        cash: 5000
      }
    ],
    isRotate: false,
    text: ''
  },
  methods: {
    changeRotate: function() {
      this.isRotate = !this.isRotate;
    },
    storeMoney: function(item) {
      item.cash = item.cash + 500;
    },
    trigger: function(name) {
      console.log(name, '此事件被觸發了')
    }
  }
})
```

```css
.box {
  display: block;
  transition: all .5s;
}

.box.rotate {
  transform: rotate(45deg)
}
```

<br>

## 切換下方 box 的 className

```html
<!-- 
  v-on:click
  @click 
-->
<div class="box" :class="{'rotate': isRotate }"></div>

<button class="btn btn-outline-primary" @click="changeRotate">
  切換 box 樣式
</button>
```

<br>

## 帶入參數

```html
<!-- 
  @click="methods(parameter)" 
  v-for 要搭配 :key
-->
<ul>
  <li v-for="item in arrayData" class="my-2" :key="item.age">
    {{ item.name }} 有 {{ item.cash }} 元
    <button
      class="btn btn-sm btn-outline-primary"
      @click="storeMoney(item)"
    >
      儲值
    </button>
  </li>
</ul>
```

<br>

## 事件修飾符

- `.stop` - 調用 event.stopPropagation()
- `.prevent` - 調用 event.preventDefault()
- `.capture` - 添加事件偵聽器時使用 capture 模式
- `.self` - 只當事件是從偵聽器綁定的元素本身觸發時才觸發回調
- `.once` - 只觸發一次回調

<br>

### 使用 stop 模式

```html
<!-- 
  點擊時會由內向外觸發
  加上 .stop 就不會了
-->
<div class="p-3 bg-primary" @click.stop="trigger('div')">
  <span class="box" @click.stop="trigger('box')"></span>
</div>
```

<br>

### 使用 capture 模式

```html
<!-- capture 會變成由外向內 -->
<div class="p-3 bg-primary" @click.capture="trigger('div')">
  <span
    class="box d-flex align-items-center justify-content-center"
    @click.capture="trigger('box')"
  >
    <button
      class="btn btn-outline-secondary"
      @click.capture="trigger('button')"
    >
      按我
    </button>
  </span>
</div>
```

<br>

### 使用 self 模式

```html
<!-- 只會觸發自己的元素 -->
<div class="p-3 bg-primary" @click.self="trigger('div')">
  <span
    class="box d-flex align-items-center justify-content-center"
    @click.self="trigger('box')"
  >
    <button
      class="btn btn-outline-secondary"
      @click.self="trigger('button')"
    >
      按我
    </button>
  </span>
</div>
```

<br>

### 使用 once 模式

```html
<!-- 只會觸發一次 -->
<div class="p-3 bg-primary" @click.once="trigger('div')">
  <span
    class="box d-flex align-items-center justify-content-center"
    @click.once="trigger('box')"
  >
    <button
      class="btn btn-outline-secondary"
      @click.once="trigger('button')"
    >
      按我
    </button>
  </span>
</div>
```

<br>

## 按鍵修飾符

- `.{keyCode | keyAlias}` - 只當事件是從特定鍵觸發時才觸發回調
- 別名修飾 - `.enter` `.tab` `.delete` `.esc` `.space` `.up` `.down` `.left` `.right`
- 修飾符來實現僅在按下相應按鍵時才觸發鼠標或鍵盤事件的監聽器 - `.ctrl` `.alt` `.shift` `.meta`

<br>

### keyCode

```html
<input
  type="text"
  class="form-control"
  v-model="text"
  @keyup.13="trigger(13)"
/>
```

<br>

### 別名修飾

```html
<input
  type="text"
  class="form-control"
  v-model="text"
  @keyup.space="trigger('space')"
/>
```

<br>

### 相應按鍵時才觸發的監聽器

```html
<input
  type="text"
  class="form-control"
  v-model="text"
  @keyup.shift.enter="trigger('shift + Enter')"
/>
```

<br>

## 滑鼠修飾符

- `.left` - (2.2.0) 只當點擊鼠標**左鍵**時觸發
- `.right` - (2.2.0) 只當點擊鼠標**右鍵**時觸發
- `.middle` - (2.2.0) 只當點擊鼠標**中鍵**時觸發

<br>

```html
<div class="p-3 bg-primary">
  <span class="box" @click.right="trigger('Right button')"> </span>
</div>
```

<br>

# 作業練習：表格排序

```html
<h1 class="mt-0 text-muted">作業練習：表格排序</h1>

<h3>模板練習作業：透過點擊 th 方式，反轉表格的排序</h3>
<p>僅需要排序價格、到期日。</p>
<div class="alert alert-secondary">
  <p>提示：</p>
  <ol class="mb-0">
    <li>將資料排序改為使用 computed 輸出</li>
    <li>
      重新依據點擊排序內容，並透過 computed 輸出。
    </li>
    <li>反轉時，th 指標需給與正確方向。</li>
    <li>加分題：第二次點擊時再次反轉資料</li>
  </ol>
</div>
```

```html
<div id="app">
  <table class="table">
    <thead>
      <tr>
        <th>品名</th>
        <th class="click">
          價格
          <span
            class="icon"
          >
            <i class=" fas fa-angle-up text-success"></i>
          </span>
        </th>
        <th class="click">
          到期日
          <span
            class="icon"
          >
            <i class=" fas fa-angle-up text-success"></i>
          </span>
        </th>
      </tr>
      <tr v-for="item in data">
        <td>{{ item.name }}</td>
        <td>{{ item.price }}</td>
        <td>{{ item.expiryDate }}</td>
      </tr>
    </thead>
  </table>
</div>
```

```js
// 參考語法
// // 使用 Sort 排序資料
// data = data.sort(function (a, b) {
//   // 抓出排序資料的值
//   a = a[欄位]
//   b = b[欄位]

//   // 回傳 1, 0, -1 來進行排列
//   // 詳細規則可參考 https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
//   return (a === b ? 0 : a > b ? 1 : -1) * 正反排序數值
// })

var app = new Vue({
  el: '#app',
  data: {
    data: [
      {
        name: '巧呼呼蘇打水',
        price: 30,
        expiryDate: 90
      },
      {
        name: '心驚膽跳羊肉飯',
        price: 65,
        expiryDate: 2
      },
      {
        name: '郭師傅武功麵包',
        price: 32,
        expiryDate: 1
      },
      {
        name: '不太會過期的新鮮牛奶',
        price: 75,
        expiryDate: 600
      },
      {
        name: '金殺 巧粒粒',
        price: 120,
        expiryDate: 200
      }
    ]
  },
  // 請在此撰寫 JavaScript
  computed: {
  
  },
  methods: {

  }
})
```

```css
.table th.click {
  cursor: pointer;
}

.table th.click {
  cursor: pointer;
}

.icon {
  display: inline-block;
}

.icon.inverse {
  transform: rotate(180deg);
}
```

<br>

## 完成作業 - 表格排序

[GitHub](https://github.com/HedgehogKUCC/sorttable)

[gh-pages](https://hedgehogkucc.github.io/sorttable/)
