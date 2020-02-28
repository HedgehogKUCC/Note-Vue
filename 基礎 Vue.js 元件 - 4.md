# Slot 插槽替換

> 這章節會使用到的程式碼

```html
<div id="app">

  <h2>沒有插槽可替換的狀態</h2>
  <no-slot-component></no-slot-component>

  <h2>Slot 基礎範例</h2>
  <single-slot-component>
    <p>使用這段取代原本的 Slot。</p>
  </single-slot-component>

  <h2>具名插槽</h2>
  <named-slot-component>
  </named-slot-component>

  <named-slot-component>
    <header>替換的 Header</header>
    <template>替換的 Footer</template>
    <template>按鈕內容</template>
    <p>其餘的內容</p>
  </named-slot-component>

</div>
```

```html
<!-- no-slot-component -->
<script type="text/x-template" id="noSlotComponent">
<div class="alert alert-warning">
  <h6>我是一個元件</h6>
  <p>
    這沒有插槽。
  </p>
</div>
</script>

<script>
Vue.component('no-slot-component', {
  template: '#noSlotComponent',
})
</script>
```

```html
<!-- single-slot-component -->
<script type="text/x-template" id="singleSlotComponent">
<div class="alert alert-warning">
  <h6>我是一個元件</h6>
  <div>
    如果沒有內容，則會顯示此段落。
  </div>
</div>
</script>

<script>
Vue.component('single-slot-component', {
  template: '#singleSlotComponent',
})
</script>
```

```html
<!-- named-slot-component -->
<script type="text/x-template" id="namedSlotComponent">
<div class="card my-3">
  <div class="card-header">
    <div>這段是預設的文字</div>
  </div>
  <div class="card-body">
    <div>
      <h5 class="card-title">Special title treatment</h5>
      <p class="card-text">With supporting text below as a natural lead-in to additional content.</p>
    </div>
    <a href="#" class="btn btn-primary">
      <div>spanGo somewhere</div>
    </a>
  </div>
  <div class="card-footer">
    <div>這是預設的 Footer</div>
  </div>
</div>
</script>

<script>
Vue.component('named-slot-component', {
  template: '#namedSlotComponent',
})
</script>
```

```js
var app = new Vue({
  el: '#app'
})
```

<br>

## 沒有插槽可替換的狀態

不會顯示出來 `<p>...</p>` ，會被模板給替換掉。

```html
<no-slot-component>
  <p>這是一段另外插入的內容</p>
</no-slot-component>
```

<br>

## slot 替換

> 如果就是有需求替換掉模板內的文字

```html
<single-slot-component>
  <p>使用這段取代原本的 Slot。</p>
</single-slot-component>
```

<br>

這樣就會將 `<p>...</p>` 顯示在 `slot` 裡面

```html
<script type="text/x-template" id="singleSlotComponent">
  <div class="alert alert-warning">
    <h6>我是一個元件</h6>
    <div>
      如果沒有內容，則會顯示此段落。
    </div>
    <slot>
      <!-- <p>使用這段取代原本的 Slot。</p> -->
    </slot>
  </div>
</script>
```

<br>

另外一種方式，這樣模板的 `slot` 就是預設文字。

```html
<script type="text/x-template" id="singleSlotComponent">
  <div class="alert alert-warning">
    <h6>我是一個元件</h6>
    <slot>
      如果沒有內容，則會顯示此段落。
    </slot>
  </div>
</script>
```

```html
<single-slot-component>
  <!-- <p>使用這段取代原本的 Slot。</p> -->
</single-slot-component>
```

<br>

## 具名插槽

```html
<named-slot-component>

  <!-- 
    <slot name="header">這段是預設的文字</slot> 
    這裡標籤使用 <header> 插入後也會是
  -->
  <header slot="header">替換的 Header</header>

  <!-- 
    <slot 
      name="content" 
      class="card-title"
    >
      Special title treatment
    </slot> 
  -->
  <p slot="content">其餘的內容</p>

  <!-- 
    <slot name="btn">spanGo somewhere</slot>
    使用標籤 <template> 就只會是純文字
    而且標籤 <template> 不會輸出
  -->
  <template slot="btn">按鈕內容</template>

  <!-- <slot name="footer">這是預設的 Footer</slot> -->
  <template slot="footer">替換的 Footer</template>

</named-slot-component>
```

```html
<script type="text/x-template" id="namedSlotComponent">
  <div class="card my-3">
    <div class="card-header">
      <slot name="header">這段是預設的文字</slot>
    </div>
    <div class="card-body">
      <div>
        <slot name="content" class="card-title">Special title treatment</slot>
        <p class="card-text">With supporting text below as a natural lead-in to additional content.</p>
      </div>
      <a href="#" class="btn btn-primary">
        <slot name="btn">spanGo somewhere</slot>
      </a>
    </div>
    <div class="card-footer">
      <slot name="footer">這是預設的 Footer</slot>
    </div>
  </div>
</script>
```

<br>

# 使用 is 動態切換組件

> 這章節會使用到的程式碼

```html
<div id="app">

  <h2>使用 is 顯示單一組件</h2>
  <primary-component :data="item"></primary-component>

  <h2 class="mt-3">使用 is 動態切換組件</h2>
  <ul class="nav nav-pills">
    <li class="nav-item">
      <a class="nav-link" :class="{'active': current == 'primary-component'}" href="#" @click.prevent="current = 'primary-component'">藍綠色元件</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" :class="{'active': current == 'danger-component'}" href="#" @click.prevent="current = 'danger-component'">紅色元件</a>
    </li>
  </ul>

  <div class="mt-3">
    <primary-component :data="item" v-if="current === 'primary-component'"></primary-component>
    <danger-component :data="item" v-if="current === 'danger-component'"></danger-component>
  </div>

</div>
```

```html
<script type="text/x-template" id="primaryComponent">
  <div class="card text-white bg-primary mb-3" style="max-width: 18rem;">
    <div class="card-header">{{ data.header }}</div>
    <div class="card-body">
      <h5 class="card-title">{{ data.title }}</h5>
      <p class="card-text">{{ data.text }}</p>
    </div>
  </div>
</script>

<script type="text/x-template" id="dangerComponent">
  <div class="card text-white bg-danger mb-3" style="max-width: 18rem;">
    <div class="card-header">{{ data.header }}</div>
    <div class="card-body">
      <h5 class="card-title">{{ data.title }}</h5>
      <p class="card-text">{{ data.text }}</p>
    </div>
  </div>
</script>
```

```js
Vue.component('primary-component', {
  props: ['data'],
  template: '#primaryComponent',
})
Vue.component('danger-component', {
  props: ['data'],
  template: '#dangerComponent',
})

var app = new Vue({
  el: '#app',
  data: {
    item: {
      header: '這裡是 header',
      title: '這裡是 title',
      text: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Enim perferendis illo reprehenderit ex natus earum explicabo modi voluptas cupiditate aperiam, quasi quisquam mollitia velit ut odio vitae atque incidunt minus?'
    },
    current: 'primary-component'
  }
})
```

<br>

## 使用 is 顯示單一組件

```html
<!-- <primary-component :data="item"></primary-component> -->
<div is="primary-component" :data="item"></div>
```

<br>

## 使用 is 動態切換組件

```html
<ul class="nav nav-pills">
  <li class="nav-item">
    <a
      class="nav-link"
      :class="{'active': current == 'primary-component'}"
      href="#"
      @click.prevent="current = 'primary-component'"
      >藍綠色元件</a
    >
  </li>
  <li class="nav-item">
    <a
      class="nav-link"
      :class="{'active': current == 'danger-component'}"
      href="#"
      @click.prevent="current = 'danger-component'"
      >紅色元件</a
    >
  </li>
</ul>

<div class="mt-3">

  <!-- 
    現在是數量不多用 v-if 來做切換
    當數量變多時 v-if 就沒有這麼好用
  -->
  <primary-component
    :data="item"
    v-if="current === 'primary-component'"
  ></primary-component>
  <danger-component
    :data="item"
    v-if="current === 'danger-component'"
  ></danger-component>

  <!-- 可以使用 :is 來做切換 -->
  <div :is="current" :data="item"></div>

</div>
```

# 完成作業 - 城市空污

[GitHub](https://github.com/HedgehogKUCC/citycomponent)

[gh-pages](https://hedgehogkucc.github.io/citycomponent/)

<br>

[JS 取出陣列重複 / 不重複值的方法](https://guahsu.io/2017/06/JavaScript-Duplicates-Array/)

[indexOf](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)