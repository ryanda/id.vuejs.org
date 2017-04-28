---
title: Template Syntax
type: guide
order: 4
---

Vue.js menggunakan syntax berbasis HTML, yang memperbolehkan kita mendeklarasikan dan melakukan binding antara DOM dan data. Semua template Vue, adalah HTML yang valid, dan dapat di*parsing*  oleh browser pada umumnya dan *HTML parser*.

Vue meng*compile* *template* menjadi fungsi *render Virtual DOM*, dikombinasikan *reactivity system*, Vue memperhitungkan dengan pintar manipulasi DOM apa yang perlu dilakukan dan komponen-komponen apa yang perlu di*render* apabila state berubah.

Apabila anda familiar dengan konsep virtual DOM, dan lebih suka membuat fungsi Javascript daripada template, anda dapat [membuat fungsi render sendiri](render-function.html), dengan dukungan JSX (opsional).

## Interpolations

### Text

*Data binding* paling dasar (satu arah) dapat menggunakan *syntax* "Moustache" (kumis), yaitu kurung kurawal ganda:

``` html
<span>Message: {{ msg }}</span>
```

Tag kumis tersebut akan diganti sesuai dengan property `msg` dari `data`, dan akan di*update* apabila `msg` berubah.

Apabila semua binding di *node* tersebut hanya ingin dilakukan sekali, gunakan [`v-once` *directive*](../api/#v-once).

``` html
<span v-once>This will never change: {{ msg }}</span>
```

### Raw HTML

Syntax kumis tersebut menghasilkan teks, bukan HTML, apabila ingin menggunakan html, gunakan `v-html` *directive*:

``` html
<div v-html="rawHtml"></div>
```

Div tersebut akan berisi HTML tanpa *data binding*. `v-html` tidak dapat digunakan sebagai *template partial*, karena Vue bukan *string-based templating engine*. Gunakan komponen untuk membuat unit partial yang lebih kecil agar dapat di*reuse* dan terkomposisi dengan benar.

<p class="tip">Merender HTML secara langsung dapat menyebabkan [XSS vulnerabilities](https://en.wikipedia.org/wiki/Cross-site_scripting), gunakan `v-html` hanya apabila isinya terpercaya dan **bukan** inputan dari user.</p>

### Atribut 

Syntax kumis tidak dapat digunkaan di dalam atribut HTML, tetapi gunakan [v-bind directive](../api/#v-bind):

``` html
<div v-bind:id="dynamicId"></div>
```

Ini juga berlaku untuk atribut *boolean*, dimana atribut akan dihapus apabila nilainya `falsy`:

``` html
<button v-bind:disabled="someDynamicCondition">Button</button>
```

### Menggunakan Ekspresi Javascript

Selama ini kita hanya membinding nilai ke dalam template, tetapi Vue memperbolehkan ekspresi Javascript di dalam semua data binding:

``` html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

Ekspresi ini akan dievaluasi dengan *scope* *instance*, dengan batasan hanya boleh satu ekspresi Javascript, jadi contoh berikut ini **tidak** akan berjalan:

``` html
<!-- this is a statement, not an expression: -->
{{ var a = 1 }}

<!-- flow control won't work either, use ternary expressions -->
{{ if (ok) { return message } }}
```
-
<p class="tip">*Template expressions* dijalankan dalam *sandbox*, dan hanya dapat mengakses variabel global di dalam *whitelist* seperti `Math` dan `date`. </p>

## Directive

*Directive* adalah atribut spesial dengan awalan `v-`, hanya boleh berisi satu ekspresi Javascript kecuali `v-for`. Tujuan dari *directive* adalah untuk menambahkan efek samping ketika nilai dari ekspresi tersebut berubah. Mari coba kita lihat kembali contoh di bagian intro:

``` html
<p v-if="seen">Now you see me</p>
```

Dalam kasus ini `v-if` akan menghapus atau menampilkan elemen `<p>` berdasarkan nilai `seen`.

### Arguments

*Directive* tertentu dapat menerima *argument* yang ditulis setelah tanda titik dua `:`, sebagai contoh `v-bind`:

``` html
<a v-bind:href="url"></a>
```

Pada contoh di atas `href` adalah *argument*, yang memberitahu `v-bind` bahwa atribut `href` yang akan diisi oleh nilai `url`.

Contoh lain adalah `v-on`, untuk *binding event* DOM:

``` html
<a v-on:click="doSomething">
```

Kita akan membahas mengenai *event binding* lebih lanjut pada bagian berikutnya.

### Modifier

*Modifier* adalah akhiran khusus yang diawali dengan titik (.), contoh: `.prevent` membuat `v-on` untuk memanggil `event.preventDefault()` pada event:

``` html
<form v-on:submit.prevent="onSubmit"></form>
```

Kita akan membahas mengenai ini lebih lanjut pada bagian `v-on` dan `v-model`.

## Filters

Vue.js memperbolehkan filter untuk melakukan formatting pada *syntax* kumis ataupun `v-bind`, dengan menambahkan *pipe* (|):

``` html
<!-- in mustaches -->
{{ message | capitalize }}

<!-- in v-bind -->
<div v-bind:id="rawId | formatId"></div>
```

<p class="tip">Untuk transformasi data yang kompleks, gunakan [Computed properties](computed.html).</p>

Fungsi *filter* selalu menerima nilai ekspresi sebagai parameter pertama

``` js
new Vue({
  // ...
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
```

*Filter* dapat di*chaining*:

``` html
{{ message | filterA | filterB }}
```

*Filter* adalah fungsi javascsript, dapat diberi parameter:

``` html
{{ message | filterA('arg1', arg2) }}
```

Dengan catatan parameter pertama akan digeser menjadi parameter kedua (demi menyisipkan nilai ekspresi), dan seterusnya.

## Shorthand

Awalan `v-` digunakan untuk menandai bahwa *attribute* itu adalah dari Vue, terkesan panjang terutama apabila anda membuat [SPA](https://en.wikipedia.org/wiki/Single-page_application) dengan skala besar, oleh karena itu, terdapat 2 shortcut yaitu untuk *directive* yg sering digunakan yaitu `v-bind` and `v-on`:

### `v-bind` Shorthand

``` html
<!-- full syntax -->
<a v-bind:href="url"></a>

<!-- shorthand -->
<a :href="url"></a>
```


### `v-on` Shorthand

``` html
<!-- full syntax -->
<a v-on:click="doSomething"></a>

<!-- shorthand -->
<a @click="doSomething"></a>
```

Terlihat sedikit berbeda dengan HTML pada umumnya, tetapi `:` dan `@` keduanya adalah karakter yang valid untuk nama atribut HTML sehingga browser dapat mem*parsing* dengan benar.