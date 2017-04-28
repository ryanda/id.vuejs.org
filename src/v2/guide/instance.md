---
title: The Vue Instance
type: guide
order: 3
---

## Constructor

Tiap *instance* Vue dibuat dengan membuat **root Vue instance** dengan fungsi constructor `Vue`:

``` js
var vm = new Vue({
  // options
})
```

Sekalipun tidak terasosiasi dengan pattern [MVVM](https://en.wikipedia.org/wiki/Model_View_ViewModel), Vue didesain dengan mengambil inspirasi dari model tersebut, sebagai konvensi, kita gunakan variabel `vm` (singkatan dari ViewModel) untuk merefer kepada sebuah *instance* Vue.

Tiap kali membuat sebuah *instance* Vue, kita perlu menambahkan **options object** yang dapat berisi `data`, `template`, `element` tempat instance bernaung, `methods`, `lifecycle callbacks`, dll. Daftar lengkap semua opsi yang ada terdapat pada [API reference](../api).

*Constructor* `Vue` dapat di*extend* untuk membuat *component constructor* baru, yaitu dengan cara:

``` js
var MyComponent = Vue.extend({
  // extension options
})

// all instances of `MyComponent` are created with
// the pre-defined extension options
var myComponentInstance = new MyComponent()
```

Sekalipun dapat juga kita melakukan extend secara imperatif, tetapi direkomenasikan untuk membuatnya secara deklaratif sebagai template sebagai [*custom element*](components.html) yang akan kita bahas lebih lanjut nanti. Untuk saat ini kita hanya perlu tahu bahwa tiap komponen di Vue adalah sebuah Vue *instance* yang telah di*extend*.

## Property dan Method

Tiap *instance* Vue terhubung dengan tiap *property* yang ada di dalam *data*:

``` js
var data = { a: 1 }
var vm = new Vue({
  data: data
})

vm.a === data.a // -> true

// setting the property also affects original data
vm.a = 2
data.a // -> 2

// ... and vice-versa
data.a = 3
vm.a // -> 3
```

*Property* yang terhubung otomatis **hanya** untuk *property* yang sudah ada sebelum *instance* Vue dibuat.

Selain `data`, *method-method* dan *property* milik instance juga terekspos keluar dengan tambahan `$` untuk membedakan dengan yang berasal dari `data`, contoh:

``` js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // -> true
vm.$el === document.getElementById('example') // -> true

// $watch is an instance method
vm.$watch('a', function (newVal, oldVal) {
  // this callback will be called when `vm.a` changes
})
```

<p class="tip">Jangan menggunakan [arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) di dalam *instance property* atau *callback* (contoh: `vm.$watch('a', newVal => this.myMethod())`). Karena *arrow functions* terhubung ke (parent context*, `this` tidak akan berisi Vue *instance*, dan `this.myMethod` akan berisi undefined.</p>

Lihat [API reference](../api) untuk daftar *property* dan *method*.

## Instance Lifecycle Hooks

Tiap *instance* Vue melalui sejumlah fungsi selama masa hidupnya, seperti: observasi perubahan data, *compile*, *mount* ke DOM, *update* DOM ketika data berubah. Terdapat **lifecycle hooks**, yaitu *callback* yang dipanggil ketika event terjadi suatu pada instance tersebut, misal [`created`](../api/#created) yaitu *hook* yang dipanggil ketika dia dibuat:

``` js
var vm = new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` points to the vm instance
    console.log('a is: ' + this.a)
  }
})
// -> "a is: 1"
```
Terdapat berbagai *hook* yang lain, seperti [`mounted`](../api/#mounted), [`updated`](../api/#updated), dan [`destroyed`](../api/#destroyed). Konteks `this` tersambung dengan *instance*. Anda mungkin menanyakan dimana konsep *controller* di Vue? Jawabannya adalah tidak ada *controller*. Logika khusus apabila ada, harus diletakkan didalam *hook-hook* tersebut. 
## Lifecycle Diagram

Berikut ini adalah diagram kehidupan sebuah *instance*.

![Lifecycle](/images/lifecycle.png)
