# props 基本觀念

> 這章節會使用到的程式碼

```html
<div id="app">
  <h2>靜態傳遞</h2>
  <photo></photo>
  <h2>動態傳遞</h2>
  <photo></photo>
</div>

<script type="text/x-template" id="photo">
  <div>
    <img :src="imgUrl" class="img-fluid" alt="" />
    <p>風景照</p>
</div>
</script>
```

```js
Vue.component('photo', {
  // 自行填寫 props 的寫法
  template: '#photo'
})

var app = new Vue({
  el: '#app',
  data: {
    url: 'https://images.unsplash.com/photo-1522204538344-922f76ecc041?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=50e38600a12d623a878983fc5524423f&auto=format&fit=crop&w=1351&q=80'
  }
})
```

<br>

## 設定 props 

```js
Vue.component('photo', {
  template: '#photo',
  props: ['imgUrl']
})
```

<br>

## 靜態傳遞

```html
<photo
  img-url="https://images.unsplash.com/photo-1522204538344-922f76ecc041?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=50e38600a12d623a878983fc5524423f&auto=format&fit=crop&w=1351&q=80"
></photo>
```

<br>

## 動態傳遞

```html
<photo :img-url="url"></photo>
```

<br>

## 特別注意

在元件設定 `props` 的值，要使用 **駝峰命名** ( 大小駝峰都可以 ) ！

但建議使用 **小駝峰** ，不可以使用此種命名方式 `img-url` ！！！

```html
<script type="text/x-template" id="photo">
  <div>
    <img :src="imgUrl" class="img-fluid" alt="" />
    <p>風景照</p>
  </div>
</script>
```

```js
Vue.component('photo', {
  template: '#photo',
  props: ['imgUrl']
})
```

<br>

在 `<photo>` 標籤上使用屬性要命名為 `img-url`

```html
<div id="app">

  <h2>靜態傳遞</h2>
  <photo
    img-url="https://images.unsplash.com/photo-1522204538344-922f76ecc041?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=50e38600a12d623a878983fc5524423f&auto=format&fit=crop&w=1351&q=80"
  ></photo>

  <h2>動態傳遞</h2>
  <photo :img-url="url"></photo>

</div>
```

<br>

如果使用 `imgUrl` 會跳出以下訊息

    [Vue tip]: 
    
    Prop "imgurl" is passed to component <Anonymous>, but the declared prop name is "imgUrl". 
    
    Note that HTML attributes are case-insensitive and camelCased props need to use their kebab-case equivalents when using in-DOM templates. 
    
    You should probably use "img-url" instead of "imgUrl".

<br>

# Props 注意事項

> 這章節會使用到的程式碼

```html
<div id="app">
  <h2>單向數據流</h2>
  <photo :img-url="url"></photo>
  <p>修正單向數據流所造成的錯誤</p>

  <h2 class="mt-3">物件傳參考特性 及 尚未宣告的變數</h2>
  <div class="row">
    <div class="col-sm-4">
      <card :user-data="user"></card>
    </div>
  </div>

  <h2 class="mt-3">維持狀態與生命週期</h2>
  <div class="form-check">
    <input type="checkbox" class="form-check-input" id="isShow" v-model="isShow">
    <label class="form-check-label" for="isShow">Check me out</label>
  </div>
  <div class="row">
    <div class="col-sm-4" v-if="isShow">
      <keep-card>
      </keep-card>
    </div>
  </div>
</div>

<script type="text/x-template" id="photo">
<div>
  <img :src="imgUrl" class="img-fluid" alt="" />
  <input type="text" class="form-control" v-model="imgUrl">
</div>
</script>


<script type="text/x-template" id="card">
<div class="card">
  <img class="card-img-top" :src="user.picture.large" alt="Card image cap">
  <div class="card-body">
    <h5 class="card-title">{{ user.name.first }} {{ user.name.last }}</h5>
    <p class="card-text">{{ user.email }}</p>
  </div>
  <div class="card-footer">
    <input type="email" class="form-control" v-model="user.email">
  </div>
</div>
</script>
```

```js
Vue.component('photo', {
  props: ['imgUrl'],
  template: '#photo',
})

Vue.component('card', {
  props: ['userData'],
  template: '#card',
  data: function () {
    return {
      user: this.userData
    }
  }
})

Vue.component('keepCard', {
  template: '#card',
  data: function() {
    return {
      user: {}
    }
  },
  created: function() {
    var vm = this
    $.ajax({
      url: 'https://randomuser.me/api/',
      dataType: 'json',
      success: function(data) {
        vm.user = data.results[0]
      }
    })
  }
})

var app = new Vue({
  el: '#app',
  data: {
    user: {},
    url: 'https://images.unsplash.com/photo-1522204538344-922f76ecc041?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=50e38600a12d623a878983fc5524423f&auto=format&fit=crop&w=1351&q=80',
    isShow: true 
  },
  created: function() {
    var vm = this
    $.ajax({
      url: 'https://randomuser.me/api/',
      dataType: 'json',
      success: function(data) {
        vm.user = data.results[0]
      }
    })
  }
})
```

<br>

## 單向數據流

```html
<photo :img-url="url"></photo>
<p>修正單向數據流所造成的錯誤</p>

<script type="text/x-template" id="photo">
  <div>
    <img :src="imgUrl" class="img-fluid" alt="" />
    <input type="text" class="form-control" v-model="imgUrl">
  </div>
</script>
```

```js
Vue.component('photo', {
  props: ['imgUrl'],
  template: '#photo',
})
```

<br>

在 input 欄位將網址隨意添加修改，會跑出以下錯誤訊息。

    [Vue warn]: Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop's value. Prop being mutated: "imgUrl"

不要直接去修改 `prop` 傳進來的內容，希望我們是 **單向** 而 **不要反向** 寫回去。

<br>

在 `Vue.component` 新增 data

```js
Vue.component('photo', {
  props: ['imgUrl'],
  template: '#photo',
  data() {
    return {
      newUrl: this.imgUrl
    }
  }
})
```

<br>

然後修改 `x-template` 的 `v-model`

```html
<script type="text/x-template" id="photo">
  <div>
    <img :src="imgUrl" class="img-fluid" alt="" />
    <input type="text" class="form-control" v-model="newUrl">
  </div>
</script>
```

<br>

## 尚未宣告的變數

當我們在傳進去的資料有時間差時會跑出以下錯誤訊息

    [Vue warn]: Error in render: "TypeError: Cannot read property 'large' of undefined"

    found in

    ---> <Card>
          <Root>

<br>

這 `user` 是從 Vue 應用程式在 `created` 使用 AJAX 非同步去撈資料

所以會產生時間差，在 render 畫面時，資料還沒進來！

才會產生上面所跳出的錯誤訊息～

```html
<card :user-data="user"></card>
```

```js
Vue.component('card', {
  props: ['userData'],
  template: '#card',
  data: function() {
    return {
      user: this.userData
    }
  }
})

var app = new Vue({
  el: '#app',
  data: {
    user: {},
    url:
      'https://images.unsplash.com/photo-1522204538344-922f76ecc041?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=50e38600a12d623a878983fc5524423f&auto=format&fit=crop&w=1351&q=80',
    isShow: true
  },
  created: function() {
    var vm = this
    $.ajax({
      url: 'https://randomuser.me/api/',
      dataType: 'json',
      success: function(data) {
        vm.user = data.results[0]
      }
    })
  }
})
```

<br>

解決方法：

加上 `v-if` 假設 user 的資料內沒有撈回哪一個屬性就不顯示

就選取一個一定會得到的資料內容屬性

```html
<card :user-data="user" v-if="user.phone"></card>
```

<br>

## 物件傳參考特性

```js
var a = { name: '小明' }

var b = a

b.name = '小強'

console.log(a.name) // 小強
```

<br>

## 維持狀態與生命週期

當我們每次點選 Check me out 都會產生不同圖片

也就是說每次元件在銷毀和生成的時候，都重新執行一次 AJAX。

```html
<div class="form-check">
  <input
    type="checkbox"
    class="form-check-input"
    id="isShow"
    v-model="isShow"
  />
  <label class="form-check-label" for="isShow">Check me out</label>
</div>
<div class="row">
  <div class="col-sm-4" v-if="isShow">
    <keep-card> </keep-card>
  </div>
</div>
```

```js
Vue.component('keepCard', {
  template: '#card',
  data: function() {
    return {
      user: {}
    }
  },
  created: function() {
    var vm = this
    $.ajax({
      url: 'https://randomuser.me/api/',
      dataType: 'json',
      success: function(data) {
        vm.user = data.results[0]
      }
    })
  }
})
```

<br>

如果我們想要看到同一張圖片就要維持元件的生命週期

使用 `keep-alive` 並且要把 `v-if` 改放在 `<keep-card>`

```html
<div class="row">
  <div class="col-sm-4">
    <keep-alive>
      <keep-card v-if="isShow"></keep-card>
    </keep-alive>
  </div>
</div>
```

<br>

打開 Vue Tool 觀察 checkbox 為 false 時

會發現元件旁邊顯示 `inactive`

<br>

在來 Console 還是有些錯誤訊息

    [Vue warn]: Error in render: 
    
    "TypeError: Cannot read property 'large' of undefined"

<br>

這個問題也是 render 畫面時，AJAX 還沒有執行完成。

所以我們一樣加上 `v-if` 去做判斷

```html
<script type="text/x-template" id="card">
  <div class="card">
    <img 
      class="card-img-top" 
      :src="user.picture.large"  
      alt="Card image cap"
      v-if="user.picture"
    >
    <div class="card-body">
      <h5 class="card-title">{{ user.name.first }} {{ user.name.last }}</h5>
      <p class="card-text">{{ user.email }}</p>
    </div>
    <div class="card-footer">
      <input type="email" class="form-control" v-model="user.email">
    </div>
  </div>
</script>
```

<br>

後面還會在跑出一個錯誤訊息

一樣加上 `v-if` 去做判斷

    [Vue warn]: Error in render: 
    
    "TypeError: Cannot read property 'first' of undefined"

<br>

```html
<h5 
  class="card-title" 
  v-if="user.name"
>
  {{ user.name.first }} {{ user.name.last }}
</h5>
```

<br>

這樣子可以確保 **資料載入後**，才把物件內容給讀取出來。
