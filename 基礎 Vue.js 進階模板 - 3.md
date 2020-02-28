# v-if 與其使用細節

> 這章節會使用到的程式碼

```html
<div id="app">

  <h4>v-if, v-else</h4>
  <p>使用 v-if, v-else 切換物件呈現</p>
  <div class="alert alert-success">成功!</div>
  <div class="alert alert-danger">失敗!</div>
  <div class="form-check">
    <input type="checkbox" class="form-check-input" id="isSuccess" v-model="isSuccess">
    <label class="form-check-label" for="isSuccess">啟用元素狀態</label>
  </div>

  <h4>template 標籤</h4>
  <p>使用 template 切換多數 DOM 呈現</p>
  <table class="table">
    <thead>
      <th>編號</th>
      <th>姓名</th>
    </thead>
    <tr>
      <td>1</td>
      <td>安妮</td>
    </tr>
    <tr>
      <td>2</td>
      <td>小明</td>
    </tr>
  </table>
  <div class="form-check">
    <input type="checkbox" class="form-check-input" id="showTemplate" v-model="showTemplate">
    <label class="form-check-label" for="showTemplate">啟用元素狀態</label>
  </div>

  <hr>

  <h4>v-else-if</h4>
  <p>使用 v-else-if 做出分頁頁籤</p>
  <ul class="nav nav-tabs">
    <li class="nav-item">
      <a class="nav-link" href="#">標題一</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="#">標題二</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="#">標題三</a>
    </li>
  </ul>
  <div class="content">
    <div>Ａ</div>
    <div>Ｂ</div>
    <div>Ｃ</div>
  </div>

  <hr>

  <h4>KEY</h4>
  <p>講師說明 Vue 切換狀態可能遇到的問題</p>
  <template v-if="loginType === 'username'">
    <label>Username</label>
    <input class="form-control" placeholder="Enter your username">
  </template>
  <template v-else>
    <label>Email</label>
    <input class="form-control" placeholder="Enter your email address">
  </template>
  <button class="btn btn-outline-primary mt-3" @click="toggleLoginType">切換狀態</button>

  <hr>

  <h4>v-if 與 v-show</h4>
  <p>講師說明 v-if 與 v-show 的差異</p>
  <div class="alert alert-success" v-show="isSuccess">成功!</div>
  <div class="alert alert-danger" v-show="!isSuccess">失敗!</div>
  <div class="form-check">
    <input type="checkbox" class="form-check-input" id="isSuccess2" v-model="isSuccess">
    <label class="form-check-label" for="isSuccess2">啟用元素狀態</label>
  </div>

</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    isSuccess: true,
    showTemplate: true,

    link: 'a',

    loginType: 'username'
  },
  methods: {
    toggleLoginType: function () {
      return this.loginType = this.loginType === 'username' ? 'email' : 'username'
    }
  }
})
```

<br>

## v-if, v-else

> 使用 v-if, v-else 切換物件呈現

```html
<!-- 加上 v-if -->
<div class="alert alert-success" v-if="isSuccess === true">成功!</div>
<div class="alert alert-danger" v-if="isSuccess === false">失敗!</div>

<!-- 簡化 -->
<div class="alert alert-success" v-if="isSuccess">成功!</div>
<div class="alert alert-danger" v-if="!isSuccess">失敗!</div>

<!-- 配合 v-else 更簡化 -->
<div class="alert alert-success" v-if="isSuccess">成功!</div>
<div class="alert alert-danger" v-else>失敗!</div>
```

<br>

## template 標籤

> 使用 template 切換多數 DOM 呈現

```html
<!-- 
  第一種方法：加上 v-if 
  但這樣如果要控制的 tr 變多，重複的 v-if 就會增加。
-->
<table class="table">
  <thead>
    <th>編號</th>
    <th>姓名</th>
  </thead>
  <tr v-if="showTemplate">
    <td>1</td>
    <td>安妮</td>
  </tr>
  <tr v-if="showTemplate">
    <td>2</td>
    <td>小明</td>
  </tr>
</table>

<!-- 
  使用 template 加上 v-if 
  就可以控制多數 DOM
  而且 template 不會被輸出
-->
<table class="table">
  <thead>
    <th>編號</th>
    <th>姓名</th>
  </thead>
  <template v-if="showTemplate">
    <tr>
      <td>1</td>
      <td>安妮</td>
    </tr>
    <tr>
      <td>2</td>
      <td>小明</td>
    </tr>
  </template>
</table>
```

<br>

## v-else-if

> 使用 v-else-if 做出分頁頁籤

```html
<ul class="nav nav-tabs">
  <li class="nav-item">
    <a
      class="nav-link"
      href="#"
      @click.prevent="link = 'a'"
      :class="{ 'active' : link === 'a' }"
      >標題一</a
    >
  </li>
  <li class="nav-item">
    <a
      class="nav-link"
      href="#"
      @click.prevent="link = 'b'"
      :class="{ 'active' : link === 'b' }"
      >標題二</a
    >
  </li>
  <li class="nav-item">
    <a
      class="nav-link"
      href="#"
      @click.prevent="link = 'c'"
      :class="{ 'active' : link === 'c' }"
      >標題三</a
    >
  </li>
</ul>

<!-- v-if 搭配 v-else-if 這樣是有相關聯的去判斷 -->
<div class="content">
  <div v-if="link === 'a'">Ａ</div>
  <div v-else-if="link === 'b'">Ｂ</div>
  <div v-else-if="link === 'c'">Ｃ</div>
</div>
```

<br>

## KEY

> Vue 切換狀態可能遇到的問題

```html
<!-- 
  打開 console 可以看到 input 是有切換
  但是所輸入的內容卻沒有隨著切換而消失
-->
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input class="form-control" placeholder="Enter your username" />
</template>
<template v-else>
  <label>Email</label>
  <input
    class="form-control"
    placeholder="Enter your email address"
  />
</template>

<!-- 
  加上 :key 
  並賦予不同值
  Vue 看到 key 不同就會重新渲染
-->
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input
    class="form-control"
    placeholder="Enter your username"
    :key="1"
  />
</template>
<template v-else>
  <label>Email</label>
  <input
    class="form-control"
    placeholder="Enter your email address"
    :key="2"
  />
</template>
```

<br>

## v-if 與 v-show 的差異

```html
<!-- v-if：會將 DOM 移除，一次只會顯示一個。 -->
<div class="alert alert-success" v-if="isSuccess">成功!</div>
<div class="alert alert-danger" v-if="!isSuccess">失敗!</div>

<!-- v-show： 使用 display: none; 元素還是會在 -->
<div class="alert alert-success" v-show="isSuccess">成功!</div>
<div class="alert alert-danger" v-show="!isSuccess">失敗!</div>
```

<br>

# 提醒

特別要注意的是 `:key`

這會是在寫 Vue 可能會踩到的地雷
