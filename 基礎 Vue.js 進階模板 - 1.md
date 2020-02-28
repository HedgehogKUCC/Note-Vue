# 基礎模板語法

> 這章節會使用到的程式碼

```html
<div id="app">

  <h4>字串</h4>
  {{ text }}
  <input type="text" class="form-control">

  <h4 class="mt-3">原始 HTML</h4>
  {{ rawHtml }}
  <p>請在此加入原始 HTML 結構</p>

  <h4 class="mt-3">單次綁定</h4>
  <div v-text="text">請將此欄位改為單次綁定</div>

  <h4 class="mt-3">表達式</h4>
  <p>練習不同的表達式</p>

  <hr>

  <h4 class="mt-3">HTML 屬性</h4>
  <p>請綁定上 ID</p>
  
  <input 
    type="text" 
    class="form-control" 
    placeholder="請在此加上動態 disabled"
  >

</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    text: '這是一段文字',
    rawHtml: `<span class="text-danger">紅色文字</span>`,
    number1: 100,
    number2: 300,
    htmlId: 'HTMLID',
    isDisabled: true
  },
});
```

<br>

## 依照上述修改完的 HTML

```html
<div id="app">

  <!-- v-model -->
  <h4>字串</h4>
  {{ text }}
  <input type="text" class="form-control" v-model="text">

  <!-- v-html -->
  <h4 class="mt-3">原始 HTML</h4>
  {{ rawHtml }}
  <p v-html="rawHtml">請在此加入原始 HTML 結構</p>

  <!-- v-once -->
  <h4 class="mt-3">單次綁定</h4>
  <div v-text="text" v-once>請將此欄位改為單次綁定</div>

  <!-- {{ expression }} -->
  <h4 class="mt-3">表達式</h4>
  <p>練習不同的表達式</p>
  {{ text + rawHtml }}
  <br />
  {{ text.split('').reverse().join('') }}
  <br />
  {{ number1 + number2 }}
  <br />
  {{ number1 * number2 }}

  <hr>

  <!-- 各種 HTML 屬性都能綁定 :id :class :src :href ...等等-->
  <h4 class="mt-3">HTML 屬性</h4>
  <p :id="htmlId">請綁定上 ID</p>
  
  <!-- :disabled -->
  <input 
    type="text" 
    class="form-control" 
    placeholder="請在此加上動態 disabled"
    :disabled="isDisabled"
  >

</div>
```

<br>

[v-html 額外注意事項](https://cn.vuejs.org/v2/api/#v-html)：

在網站上動態渲染任意 HTML 是非常危险的，因為容易導致 XSS 攻擊。

只在可信任内容上使用 v-html，絕不能在用戶提交的内容上。

<br>

`XSS 攻擊`：

跨網站指令碼 ( 英語：Cross-site scripting，通常簡稱為：XSS )

是一種網站應用程式的安全漏洞攻擊，是代碼注入的一種。

它允許惡意使用者將程式碼注入到網頁上，其它使用者在觀看網頁時就會受到影響！

<br>

# 動態切換 className 及 style 多種方法

```html
<div id="app">

  <h4>物件寫法 1</h4>
  <div class="box"></div>
  <p>請為此元素加上動態 className</p>

  <hr>

  <button class="btn btn-outline-primary">選轉物件</button>
  <div class="form-check">
    <input type="checkbox" class="form-check-input" id="classToggle1">
    <label class="form-check-label" for="classToggle1">切換色彩</label>
  </div>

  <hr>

  <h5>物件寫法 2</h5>
  <div class="box"></div>
  <p>請將此範例改為 "物件" 寫法</p>

  <hr>

  <button class="btn btn-outline-primary">選轉物件</button>
  <div class="form-check">
    <input type="checkbox" class="form-check-input" id="classToggle2">
    <label class="form-check-label" for="classToggle2">切換色彩</label>
  </div>

  <hr>

  <h4>陣列寫法</h4>
  <button class="btn">請操作本元件</button>
  <p>請用陣列呈現此元件 className</p>
  <div class="form-check">
    <input type="checkbox" class="form-check-input" id="classToggle3">
    <label class="form-check-label" for="classToggle3">切換樣式</label>
  </div>
  <div class="form-check">
    <input type="checkbox" class="form-check-input" id="classToggle4">
    <label class="form-check-label" for="classToggle4">啟用元素狀態</label>
  </div>

  <hr>

  <h4>綁定行內樣式</h4>
  <p>請用不同方式綁定以下行內樣式</p>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>

  <hr>

  <h5>自動加上 Prefix (每個版本結果不同)</h5>
  <div class="box"></div>

</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    isTransform: false,
    boxColor: false,
    objectClass: {
      'rotate': false,
      'bg-danger': false,
    },

    // Array 操作
    arrayClass: [],

    // 行內樣式
    // 使用駝峰式命名
    styleObject: {
      backgroundColor: 'red',
      borderWidth: '5px'
    },
    styleObject2: {
      boxShadow: '3px 3px 5px rgba(0, 0, 0, 0.16)'
    },
    styleObject3: {
      userSelect: 'none'
    }
  },
});
```

```css
.box {
  transition: all .5s;
}
.box.rotate {
  transform: rotate(45deg)
}
```

<br>

## 加上動態 ClassName

```html
<!-- :class="{ '樣式' : 判斷式, '樣式' : 判斷式 }" -->
<div
  class="box"
  :class="{'rotate' : isTransform, 'bg-danger' : boxColor}"
></div>

<p>請為此元素加上動態 ClassName</p>

<hr />

<!-- v-on:click="" or @click="" -->
<button
  class="btn btn-outline-primary"
  v-on:click="isTransform = !isTransform"
>
  選轉物件
</button>

<!-- v-model -->
<div class="form-check">
  <input
    type="checkbox"
    class="form-check-input"
    id="classToggle1"
    v-model="boxColor"
  />
  <label class="form-check-label" for="classToggle1"
    >切換色彩</label
  >
</div>
```

<br>

## 將上面 ClassName 改為另一種 "物件" 寫法

> 這種寫法比較少見

```html
<!-- objectClass: { 'rotate': false, 'bg-danger': false } -->
<div class="box" :class="objectClass"></div>

<p>請將此範例改為 "物件" 寫法</p>

<hr />

<button
  class="btn btn-outline-primary"
  @click="objectClass.rotate = !objectClass.rotate"
>
  選轉物件
</button>

<div class="form-check">
  <input
    type="checkbox"
    class="form-check-input"
    id="classToggle2"
    v-model="objectClass['bg-danger']"
  />
  <label class="form-check-label" for="classToggle2"
    >切換色彩</label
  >

</div>
```

<br>

這裡要特別注意的是 `v-model="objectClass.bg-danger"` 會跳出錯誤

    [Vue warn]: Property or method "danger" is not defined on the instance but referenced during render

請改寫為 `objectClass['bg-danger']`

<br>

## 陣列寫法呈現此元件 ClassName

```html
<!-- arrayClass: [] -->
<button class="btn" :class="arrayClass">
  請操作本元件
</button>

<p>請用陣列呈現此元件 className</p>

<!-- v-model、value -->
<div class="form-check">
  <input
    type="checkbox"
    class="form-check-input"
    id="classToggle3"
    v-model="arrayClass"
    value="btn-outline-primary"
  />
  <label class="form-check-label" for="classToggle3"
    >切換樣式</label
  >
</div>

<!-- v-model、value -->
<div class="form-check">
  <input
    type="checkbox"
    class="form-check-input"
    id="classToggle4"
    v-model="arrayClass"
    value="active"
  />
  <label class="form-check-label" for="classToggle4"
    >啟用元素狀態</label
  >

</div>
```

<br>

## 綁定行內樣式

```html
<p>請用不同方式綁定以下行內樣式</p>

<!-- :style=" { '樣式屬性' : '樣式的值' } " -->
<div class="box" :style="{ 'backgroundColor' : 'red' }"></div>

<!-- 請使用駝峰式命名，不可寫成 background-color、border-width -->
<!-- 
  styleObject: { 
    backgroundColor : 'red', 
    borderWidth : '5px' 
  } 
-->
<div class="box" :style="styleObject"></div>

<!-- 陣列裡面是放物件 -->
<!-- 
  :style=" [ 
    { 
      'backgroundColor' : 'red' 
    }, 
    { 
      'borderWidth' : '5px' 
    } 
  ] " 
-->
<div class="box" :style="[styleObject, styleObject2]"></div>
```

<br>

## 自動加上 Prefix (每個版本結果不同)

```html
<!-- styleObject3: { userSelect : 'none' } -->
<h5 :style="styleObject3">自動加上 Prefix (每個版本結果不同)</h5>
```

[user-select](https://www.w3schools.com/cssref/css3_pr_user-select.asp)：文本是否可以被選擇
