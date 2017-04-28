---
title: Instalasi
type: guide
order: 1
vue_version: 2.3.0
dev_size: "247.31"
min_size: "76.64"
gz_size: "28.03"
ro_gz_size: "19.54"
---

### Kompatibilitas

Vue **tidak** mendukung IE8 kebawah, karena IE8 tidak mendukung ES5 secara lengkap sekalipun menggunakan [shim](https://en.wikipedia.org/wiki/Shim_(computing)). Vue mendukung semua browser yang mendukung [ECMAScript 5](http://caniuse.com/#feat=es5).

### Keterangan Rilis

Keterangan rilis tiap versi terdapat pada [GitHub](https://github.com/vuejs/vue/releases).

## Dengan `<script>`

Dengan men*download* dan menuliskan tag script, maka `Vue` akan diregistrasikan sebagai variabel global.

<p class="tip">Jangan menggunakan versi *minified*/*production* selama proses pembuatan software, karena *warning* atas kesalahan-kesalahan umum tidak akan tampil.</p>

<div id="downloads">
<a class="button" href="/js/vue.js" download>Development Version</a><span class="light info"> dengan *warning* dan mode *debug*</span>

<a class="button" href="/js/vue.min.js" download>Production Version</a><span class="light info"> tanpa *warning*, {{gz_size}}kb min+gzip</span>
</div>

### CDN

Rekomendasi: [https://unpkg.com/vue](https://unpkg.com/vue), akan menggunakan versi terakhir sesuai npm. Anda juga dapat melihat sumber npm-nya di [https://unpkg.com/vue/](https://unpkg.com/vue/).

Terdapat juga alternatif [jsDelivr](//cdn.jsdelivr.net/vue/latest/vue.js) atau [cdnjs](//cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.js), tetapi keduanya membutuhkan waktu untuk sinkronisasi, sehingga tidak selalu versi terakhir.

## NPM

NPM adalah metode instalasi yang direkomendasikan untuk aplikasi berskala besar, karena dapat dipergunakan dengan *bundler* seperti [Webpack](https://webpack.js.org/) atau [Browserify](http://browserify.org/). Vue juga menyediakan *authoring tool* untuk [Single File Components](single-file-components.html).

``` bash
# latest stable
$ npm install vue
```

## CLI

Vue menyediakan [program CLI](https://github.com/vuejs/vue-cli) resmi untuk memudahkan pembuatan SPA dengan fitur *hot-reload*, *lint-on-save*, dan ()production-ready build*:

``` bash
# install vue-cli
$ npm install --global vue-cli
# create a new project using the "webpack" template
$ vue init webpack my-project
# install dependencies and go!
$ cd my-project
$ npm install
$ npm run dev
```

<p class="tip">Pengguna program CLI harus memiliki pengetahuan mengenai Node.js dan build tools, apabila anda pemula sebaiknya baca terlebih dahulu guide tanpa menggunakan CLI</p>

## Direktori dist/

Di dalam direktori [`dist/`](https://unpkg.com/vue@latest/dist/) pada paket NPM, terdapat berbagai jenis *build* berikut ini perbedaannya:

| | UMD | CommonJS | ES Module |
| --- | --- | --- | --- |
| **Full** | vue.js | vue.common.js | vue.esm.js |
| **Runtime-only** | vue.runtime.js | vue.runtime.common.js | vue.runtime.esm.js |
| **Full (production)** | vue.min.js | - | - |
| **Runtime-only (production)** | vue.runtime.min.js | - | - |

### Istilah

- **Full**: *build* yang berisi *compiler* dan *runtime*

- **Compiler**: kode yang bertugas meng*compile* *template string* menjadi fungsi *render* javascript.

- **Runtime**: kode yang bertugas membuat Vue *instance*, *rendering*. *patching* *virtual DOM*, dll, yang artinya adalah semua hal selain compiler.

- **[UMD](https://github.com/umdjs/umd)**: UMD dapat dipergunakan secara langsung di browser dengan tag `<script>`. File yang sama dengan CDN Unpkg [https://unpkg.com/vue](https://unpkg.com/vue) adalah Runtime + Compiler UMD build (`vue.js`).

- **[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)**: CommonJS untuk *bundler* versi lama seperti [browserify](http://browserify.org/) atau [webpack 1](https://webpack.github.io). Isi (`pkg.main`) hanya Runtime CommonJS (`vue.runtime.common.js`).

- **[ES Module](http://exploringjs.com/es6/ch_modules.html)**: ES untuk *bundler* yang lebih baru [webpack 2](https://webpack.js.org) atau [rollup](http://rollupjs.org/). Isi (`pkg.module`) is hanya Runtime ES Module (`vue.runtime.esm.js`).

### Runtime + Compiler vs. hanya Runtime

Apabila anda membutuhkan untuk meng*compile* *template* ketika *runtime* (misal mempassing *string* ke `template` atau memembuat Vue *instance* menggunakan DOM), anda juga membutuhkan *compiler* tidak hanya *runtime build*:

``` js
// this requires the compiler
new Vue({
  template: `<div>{{ hi }}</div>`
})

// this does not
new Vue({
  render (h) {
    return h('div', this.hi)
  }
})
```

Dengan menggunakan `vue-loader` atau `vueify` template di dalam file `*.vue` di*precompile* ke Javascript ketika *build*, sehingga kita hanya memerlukan hanya *runtime build*.

Tanpa *full build* (*compiler*+*runtime*) menggunakan 30% lebih banyak dibandingkan hanya *runtime*, apabila membutuhkannya, harus mengganti alias di dalam *bundler*:

#### Webpack

``` js
module.exports = {
  // ...
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js' for webpack 1
    }
  }
}
```

#### Rollup

``` js
const alias = require('rollup-plugin-alias')

rollup({
  // ...
  plugins: [
    alias({
      'vue': 'vue/dist/vue.esm.js'
    })
  ]
})
```

#### Browserify

Tambahkan di `package.json`:

``` js
{
  // ...
  "browser": {
    "vue": "vue/dist/vue.common.js"
  }
}
```

### Mode Development vs. Production

Mode *development*/*production* modes ter*hardcode* di UMD builds, yaitu minified untuk production.

CommonJS dan ES Module *build* ditujukan untuk *bundler*, sehingga tidak terdapat minified, anda harus melakukan *minify* secara mandiri.

CommonJS and ES Module juga memeriksa `process.env.NODE_ENV` untuk memastikan mode apa program harus dieksekusi, anda harus mengganti *environment variable* atau mengganti *string* `process.env.NODE_ENV` secara langsung sesuai kebutuhan, atau untuk menghapus *development mode* agar minifier seperti UglifyJS mampu mengoptimalkan ukuran final dari *build*.

#### Webpack

Dengan [DefinePlugin](https://webpack.js.org/plugins/define-plugin/): Webpack 

``` js
var webpack = require('webpack')

module.exports = {
  // ...
  plugins: [
    // ...
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: JSON.stringify('production')
      }
    })
  ]
}
```

#### Rollup

Dengan [rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace):

``` js
const replace = require('rollup-plugin-replace')

rollup({
  // ...
  plugins: [
    replace({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
}).then(...)
```

#### Browserify

Dengan [envify](https://github.com/hughsk/envify):

``` bash
NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
```

Lihat juga saran-saran [Production Deployment](deployment.html).

### CSP environments

Dalam *environment* tertentu seperti Google Chrome Apps, dimana Content Security Policy (CSP) membatasi penggunaan `new Function()` untuk saat mengevaluasi ekspresi, *full build* membutuhkan hal ini untuk mengcompile template, sehingga fitur *compiler* tidak dapat digunakan.

*Runtime* kompatibel dengan lingkungan CSP. Apabila menggunakan hanya *runtime* di [Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple) atau [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple), semua template akan di*precompile* menjadi fungsi `render` sehingga dapat berjalan di lingkungan CSP.

## Dev Build

**Penting**: direktori `/dist` di Github, hanya diupdate ketika rilis, apabila ingin menggunakan source code dari Github, anda harus melakukan proses *build* sendiri.

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

Bower dapat membuat UMD builds.

``` bash
# latest stable
$ bower install vue
```

## AMD Module Loaders

Semua UMD build dapat digunakan secara langsung sebagai modul [AMD](http://requirejs.org/docs/whyamd.html).
