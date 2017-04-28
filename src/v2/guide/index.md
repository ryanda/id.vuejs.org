---
title: Introduction
type: guide
order: 2
---

## Apa itu Vue.js?

Vue (/vjuː/, seperti **view**) adalah *progressive framework* untuk membuat user interface. Berbeda dengan framework monolitis, Vue didesain dari bawah ke atas, agar lebih mudah diadaptasi secara bertahap. Pustaka inti (*core library*) difokuskan pada layer tampilan, dan sangat mudah untuk diintegrasikan dengan pustaka lain atau proyek yang sedang berjalan. Selain itu, Vue juga sangat mampu untuk dipakai sebagai dasar untuk membuat *Single-Page Applications* (SPA) yang kompleks, apabila digunakan bersamaan dengan [komponen tunggal (*single-file component*)](single-file-components.html) dan [pustaka pendukung (*external libraries*)](https://github.com/vuejs/awesome-vue#components--libraries).

Apabila anda adalah frontend programmer yang berpengalaman, mungkin anda ingin tahu [perbedaan Vue dengan library/framework lainnya](comparison.html).

## Memulai

<p class="tip">Panduan resmi ini mengasumsikan bahwa anda telah memiliki pengetahuan yang cukup mengenai [HTML](https://www.w3schools.com/html/), [CSS](https://www.w3schools.com/css/), dan [Javascript](https://www.w3schools.com/js/). Apabila anda benar-benar baru di bidang *frontend*, langsung menggunakan framework bukanlah ide yang terbaik, pelajari terlebih dahulu [dasar-dasar](https://www.w3schools.com)nya terlebih dahulu. Pengalaman menggunakan framework lain bisa membantu, tetapi tidak diwajibkan dalam mempelajari Vue.</p>

Cara termudah untuk mencoba Vue.js adalah dengan contoh [JSFiddle Hello World](https://jsfiddle.net/chrisvfritz/50wL7mdz/). Silahkan membukanya di tab baru lalu ubah sambil mengikuti contoh-contoh dari tutorial, atau buatlah sebuah file `.html` yang berisi:

``` html
<script src="https://unpkg.com/vue"></script>
```

Halaman [Instalasi](installation.html) berisi cara-cara lain untuk menginstall Vue. Untuk pemula sebaiknya **tidak** langsung menggunakan `vue-cli`, terutama apabila anda belum memiliki pengalaman menggunakan *build tools* [Node.js](https://nodejs.org/en/).

## *Declarative Rendering*

Inti dari Vue.js adalah sistem untuk me-*render* data ke dalam [DOM](https://en.wikipedia.org/wiki/Document_Object_Model) dengan langkah yang mudah, yaitu menggunakan *syntax template*:

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Halo Vue!'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Halo Vue!'
  }
})
</script>
{% endraw %}

Kita telah berhasil membuat aplikasi vue pertama kita! Ini sangat mirip seperti me-*render string template*, karena Vue yang mengerjakan hal-hal kompleks di dalamnya. Data dan DOM sekarang saling menyambung dan **reactive**. Bagaimana cara kita tahu bahwa ini sudah *reactive*?
Buka browser Javascript console pada halaman tersebut, lalu ganti isi variabel `app.message` menjadi nilai lain, maka kita akan melihat perubahan itu juga ter-*render* sesuai template.

Selain *text interpolation* dengan `{{ data }}`, kita juga dapat melakukan penyambungan/pentautan (*binding*) pada atribut elemen dengan `v-bind:`, misal:

``` html
<div id="app-2">
  <span v-bind:title="message">
    Arahkkan kursor mouse anda di sini untuk melihat tooltip yang di-binding secara dinamis
  </span>
</div>
```
``` js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Anda membuka halaman ini pada ' + new Date()
  }
})
```
{% raw %}
<div id="app-2" class="demo">
  <span v-bind:title="message">
    Arahkkan kursor mouse anda di sini untuk melihat tooltip yang di-binding secara dinamis
  </span>
</div>
<script>
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Anda membuka halaman ini pada ' + new Date()
  }
})
</script>
{% endraw %}

Atribut `v-bind` dinamakan **directive**. Tiap *directive* diawali dengan `v-` untuk menandakan bahwa atribut tersebut disediakan oleh vue, dalam hal ini bertugas untuk menambahkan *reactive behavior* yaitu dalam kasus ini bertugas mengganti isi atribut `title` dari elemen agar selalu sinkron dengan `data.message` dari Vue.

Apabila kita membuka JavaScript console lalu mengganti `app2.message = 'sesuatu yang baru'`, lalu mengarahkan ulang kursor ke elemen tersebut, anda kita melihat bahwa `title` juga ikut berubah.

## Percabangan dan Perulangan

Dengan Vue, untuk menyembunyikan atau menampilkan elemen menjadi mudah:

``` html
<div id="app-3">
  <p v-if="seen">Aku hadir dan menyapa</p>
</div>
```

``` js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

{% raw %}
<div id="app-3" class="demo">
  <span v-if="seen">Aku hadir dan menyapa</span>
</div>
<script>
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
</script>
{% endraw %}

Apabila kita ubah `app3.seen = false` di console, maka elemen tersebut akan menghilang.

Contoh ini mendemonstrasikan bahwa kita dapat melakukan binding data tidak hanya pada teks ataupun atribut, tetapi juga pada struktur dari DOM. Vue juga menyediakan [efek transisi](transitions.html) (animasi), yang dieksekusi ketika elemen ditambahkan/diupdate/dihapus oleh Vue.

Terdapat beberapa *directive* lain, yang memiliki fungsi yang berbeda, seperti `v-for` untuk melakukan perulangan untuk tiap elemen dari Array:

``` html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
``` js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Belajar JavaScript' },
      { text: 'Belajar Vue' },
      { text: 'Buat sesuatu yang keren' }
    ]
  }
})
```
{% raw %}
<div id="app-4" class="demo">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Belajar JavaScript' },
      { text: 'Belajar Vue' },
      { text: 'Buat sesuatu yang keren' }
    ]
  }
})
</script>
{% endraw %}

Di console, ketik `app4.todos.push({ text: 'Belajar Weex' })`, maka item yang baru akan muncul.

## Menangani Inputan User

Untuk menangkap event, kita dapat mendambahkan `v-on` *directive*:

``` html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Balik Pesan</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Halo Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app-5" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Balik Pesan</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Halo Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Di dalam *method* reverseMessage tersebut kita tidak memanipulasi DOM secara langsung, tetapi melalui Vue, sehingga kode yang kita tuliskan hanya perlu berfokus pada logika bisnisnya saja.

Vue juga menyediakan `v-model` *directive*, untuk melakukan *binding* 2 arah antara form input dan *state* data aplikasi:

``` html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Halo Vue!'
  }
})
```
{% raw %}
<div id="app-6" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Halo Vue!'
  }
})
</script>
{% endraw %}

## Menyusun dengan Komponen

*Component system* adalah salah satu konsep penting di Vue, karena terdapat abstraksi yang memperbolehkan pembuatan aplikasi berskala besar yang tersusun atas komponen-komponen kecil yang mandiri dan dapat dipergunakan di proyek lain (*reusable*). Apabila If we think about it, almost any type of application interface can be abstracted into a tree of components:

![Component Tree](/images/components.png)

Komponen di Vue sebenarnya adalah sebuah *instance* dengan opsi-opsi yang telah diatur sejak awal (*predefined*). Untuk mendaftarkan sebuah komponen di Vue:

``` js
// Define a new component called todo-item
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})
```

Kita dapat menggunakannya ke dalam komponen lain seperti tag html biasa:

``` html
<ol>
  <!-- Create an instance of the todo-item component -->
  <todo-item></todo-item>
</ol>
```

Namun ini akan me*render* teks yang sama untuk tiap `todo-item`, untuk mem*passing* data dari *parent* ke *child* kita dapat menggunakan atribut, yaitu pada Vue dinamakan [prop](components.html#Props):

``` js
Vue.component('todo-item', {
  // The todo-item component now accepts a
  // "prop", which is like a custom attribute.
  // This prop is called todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

Sekarang kita dapat menambahkan atribut `todo` untuk tiap komponen yang berulang, yaitu dengan `v-bind`:

``` html
<div id="app-7">
  <ol>
    <!-- Now we provide each todo-item with the todo object    -->
    <!-- it's representing, so that its content can be dynamic -->
    <todo-item v-for="item in groceryList" v-bind:todo="item"></todo-item>
  </ol>
</div>
```
``` js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { text: 'Vegetables' },
      { text: 'Cheese' },
      { text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```
{% raw %}
<div id="app-7" class="demo">
  <ol>
    <todo-item v-for="item in groceryList" v-bind:todo="item"></todo-item>
  </ol>
</div>
<script>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { text: 'Vegetables' },
      { text: 'Cheese' },
      { text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
</script>
{% endraw %}

Ini hanya contoh sederhana, dimana kita berhasil memisahkan aplikasi kita ke dalam 2 komponen yang lebih kecil, dan melakukan komuniakasi dari *parent* ke *child* melalui `props`. Sekarang kita dapat memperbaiki komponen `<todo-item>` dengan *template* dan logika yang lebih kompleks tanpa mempengaruhi *parent*.

Pada aplikasi yang lebih besar, adalah penting untuk memisahkan aplikasi ke dalam komponen-komponen kecil agar lebih mudah di*maintain*. Tutorial akan membahas tentang [komponen](components.html) lebih lanjut nanti, ini adalah sedikit gambaran bagaimana *template* aplikasi apabila dibuat dengan komponen:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### Hubungan dengan Custom Element

Komponen di Vue sangat mirip dengan **Custom Elements**, yang merupakan bagian dari [Web Components Spec](http://www.w3.org/wiki/WebComponents/). 
Hal ini karena syntax komponen di vue memang diimplementasikan menyerupai model spesifikasi tersebut, seperti [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) dan atribut `is`, tetapi dengan perbedaan utama:

1. Spesifikasi Web Components masih bersifat draft, dan belum diimplementasikan di semua browser, sedangkan di Vue dapat berjalan di semua browser (IE9+) dan tidak membutuhkan [polyfills](https://polyfill.io). Apabila dibutuhkan, komponen vue dapat dibungkus di dalam custom element yang *native*.

2. Komponen di Vue, menyediakan fitur-fitur yang tidak terdapat di Custom Elements, seperti: *cross-component data flow*, *custom event communication* dan integrasi dengan *build tool*.

## Ingin mempelajari lebih lanjut?

Kita baru saja mempelajari fitur-fitur dasar dari Vue.js, panduan ini akan mencakup fitur-fitur lanjut dan lebih mendetil, pastikan anda membacanya secara menyeluruh.