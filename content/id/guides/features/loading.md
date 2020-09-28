---
title: Loading
description: Hal yang di luar kotak, Nuxt.js memberikan Anda proses _loading_ komponen sendiri yang ditampilkan diantara rute. Anda dapat melakukan kustomisasi, menonaktifkan atau bahkan membuat proses _loading_ milik Anda sendiri.
position: 8
category: features
csb_link: https://codesandbox.io/embed/github/nuxt-academy/guides-examples/tree/master/03_features/08_loading?fontsize=14&hidenavigation=1&theme=dark
questions:
  - question: Untuk membuat proses _loading_ Nuxt.js bekerja, apa yang Anda perlu lakukan?
    answers:
      - Tidak ada, itu langsung bekerja
      - Mengatur proses _loading_ menjadi true pada nuxt.config.js
      - Membuat komponen _loading_
    correctAnswer: Tidak ada, itu langsung bekerja
  - question: Dimana Anda dapat melakukan modifikasi gaya untuk proses _loading_ bawaan?
    answers:
      - komponen layout
      - komponen page
      - nuxt.config.js
    correctAnswer: komponen layout
  - question: Pada properti mana anda dapat mengatur gaya untuk proses _loading_ di nuxt.config.js?
    answers:
      - progress
      - loading
      - loadingBar
    correctAnswer: loading
  - question: Apa yang Anda tambahkan pada nuxt.config.js untuk menonaktifkan _loading_?
    answers:
      - 'loadingBar: false'
      - "loading: 'none'"
      - 'loading: false'
    correctAnswer: 'loading: false'
  - question: Anda dapat menonaktifkan _loading_ pada halaman tertentu?
    answers:
      - true
      - false
    correctAnswer: true
  - question: Apa yang Anda gunakan untuk memulai batang _loading_ secara terprogram?
    answers:
      - $nuxt.loading.start()
      - $nuxt.loading()
      - $loading.start()
    correctAnswer: $nuxt.loading.start()
  - question: Bagian properti mana yang Anda gunakan untuk membuat batang proses secara terus-menerus ketika _loading_ mengambil waktu yang lama dari yang diprediksi?
    answers:
      - "duration: 'continuous'"
      - "loading: 'continuous'"
      - 'continuous: true'
    correctAnswer: 'continuous: true'
  - question: Mana dua metode yang dibutuhkan ketika membuat komponen _loading_ secara kustom?
    answers:
      - start() dan fail()
      - start() dan finish()
      - loading() dan finish()
    correctAnswer: start() dan finish()
  - question: Ketika Anda telah membuat komponen loading.vue, bagaimana Anda menggunakan itu?
    answers:
      - diimpor ke halaman layout
      - menambahkan di nuxt.config.js dibawah properti _loading_
      - menambahkan di nuxt.config.js dibawah properti _plugins_
    correctAnswer: menambahkan di nuxt.config.js dibawah properti _loading_
  - question: Untuk menambahkan pemuat lingkaran berputar ketika Nuxt.js di metode SPA, apa yang Anda tambahkan ke properti _loading_?
    answers:
      - 'circle: true'
      - 'spinner: circle'
      - 'name: circle'
    correctAnswer: 'name: circle'
---

Hal yang di luar kotak, Nuxt.js memberikan Anda proses _loading_ komponen sendiri yang ditampilkan diantara rute. Anda dapat melakukan kustomisasi, menonaktifkan atau bahkan membuat proses _loading_ milik Anda sendiri.

## Melakukan kustomisasi batang proses

Diantara properti lainnya, warna, ukuran, durasi, dan arah dari batang proses dapat dikustomisasi untuk menyesuaikan aplikasi Anda. Ini dilakukan dengan melakukan update properti `loading` dari `nuxt.config.js` sesuai dengan properti,

Contohnya, untuk mengatur sebuah batang proses berwarna biru dengan tinggi 5px, kita perbarui `nuxt.config.js` seperti berikut ini:

```js
export default {
  loading: {
    color: 'blue',
    height: '5px'
  }
}
```

Daftar properti untuk melakukan kustomisasi batang proses.

| Key         | Tipe    | Nilai anggapan | Deskripsi                                                                                                                   |     |
| ----------- | ------- | -------------- | --------------------------------------------------------------------------------------------------------------------------- | --- |
| color       | String  | 'black'        | Warna CSS untuk batang proses                                                                                               |     |
| failedColor | String  | 'red'          | Warna CSS dari batang proses ketika terjadi sebuah kegalatan ketika me-_render_ (jika data atau `fetch` mengirimkan error). |     |
| height      | String  | '2px'          | Tinggi dari batang proses (digunakan pada properti gaya dari batang proses).                                                |     |
| throttle    | Number  | 200            | Dalam milidetik, tunggu pada waktu tertentu untuk menampilkan batang proses. Berguna untuk mencegah batang dari _flashing_. |     |
| duration    | Number  | 5000           | Dalam milidetik, durasi maksimal dari batang proses, Nuxt.js mengasumsikan bahwa rute akan di-_render_ sebelum 5 detik.     |     |
| continuous  | Boolean | false          | Tetap menjalankan animasi batang proses ketika memuat lebih lama dari yang diharapkan.                                      |     |
| css         | Boolean | true           | Atur ke `false` untuk menghapus gaya dari batang proses bawaan (dan tambahkan milik Anda sendiri).                          |     |
| rtl         | Boolean | false          | Atur arah dari batang proses dari kanan ke kiri.                                                                            |     |

## Disable the Progress Bar

If you don't want to display the progress bar between the routes add `loading: false` in your `nuxt.config.js` file:

```js{}[nuxt.config.js]
export default {
  loading: false
}
```

The loading property gives you the option to disable the default loading progress bar on a specific page.

```html{}[pages/index.vue]
<template>
  <h1>My page</h1>
</template>

<script>
  export default {
    loading: false
  }
</script>
```

## Programmatically starting the loading bar

The loading bar can also be programmatically started in your components by calling `this.$nuxt.$loading.start()` to start the loading bar and `this.$nuxt.$loading.finish()` to finish it.

During your page component's mounting process, the `$loading` property may not be immediately available to access. To work around this, if you want to start the loader in the `mounted` method, make sure to wrap your `$loading` method calls inside `this.$nextTick` as shown below.

```js
export default {
  mounted() {
    this.$nextTick(() => {
      this.$nuxt.$loading.start()
      setTimeout(() => this.$nuxt.$loading.finish(), 500)
    })
  }
}
```

## Internals of the Progress Bar

Unfortunately, it is not possible for the Loading component to know in advance how long loading a new page will take. Therefore, it is not possible to accurately animate the progress bar to 100% of the loading time.

Nuxt's loading component partially solves this by letting you set the `duration`, this should be set to an estimate of how long the loading process will take. Unless you use a custom loading component, the progress bar will always move from 0% to 100% in `duration` time (regardless of actual progression). When the loading takes longer than `duration` time, the progress bar will stay at 100% until the loading finishes.

You can change the default behaviour by setting `continuous` to true, then after reaching 100% the progress bar will start shrinking back to 0% again in `duration` time. When the loading is still not finished after reaching 0% it will start growing from 0% to 100% again, this repeats until the loading finishes.

```js
export default {
  loading: {
    continuous: true
  }
}
```

_Example of a continuous progress bar:_

![https://nuxtjs.org/api-continuous-loading.gif](https://nuxtjs.org/api-continuous-loading.gif)

## Using a Custom Loading Component

You can also create your own component that Nuxt.js will call instead of the default loading progress bar component. To do so, you need to give a path to your component in the `loading` option. Then, your component will be called directly by Nuxt.js.

Your component has to expose some of these methods:

| Method        | Required | Description                                                                              |
| ------------- | -------- | ---------------------------------------------------------------------------------------- |
| start()       | Required | Called when a route changes, this is where you display your component.                   |
| finish()      | Required | Called when a route is loaded (and data fetched), this is where you hide your component. |
| fail(error)   | Optional | Called when a route couldn't be loaded (failed to fetch data for example).               |
| increase(num) | Optional | Called during loading the route component, num is an Integer < 100.                      |

You can create your custom component in `components/LoadingBar.vue`:

```html{}[components/LoadingBar.vue]
<template>
  <div v-if="loading" class="loading-page">
    <p>Loading...</p>
  </div>
</template>

<script>
  export default {
    data: () => ({
      loading: false
    }),
    methods: {
      start() {
        this.loading = true
      },
      finish() {
        this.loading = false
      }
    }
  }
</script>

<style scoped>
  .loading-page {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(255, 255, 255, 0.8);
    text-align: center;
    padding-top: 200px;
    font-size: 30px;
    font-family: sans-serif;
  }
</style>
```

Then, you update your `nuxt.config.js` to tell Nuxt.js to use your component:

```js{}[nuxt.config.js]
export default {
  loading: '~/components/LoadingBar.vue'
}
```

## The loading indicator Property

When running Nuxt.js in SPA mode, there is no content from the server side on the first page load. So, instead of showing a blank page while the page loads, Nuxt.js gives you a spinner which you can customize to add your own colors or background and even change the the indicator.

```js{}[nuxt.config.js]
export default {
  loadingIndicator: {
    name: 'circle',
    color: '#3B8070',
    background: 'white'
  }
}
```

## Built-in indicators

These indicators are imported from awesome [Spinkit](http://tobiasahlin.com/spinkit) project. You can check out its demo page to preview the spinners. In order to use one of these spinners all you have to do is add it's name to the name property. No need to import or install anything. Here is a list of built in indicators you can use.

- circle
- cube-grid
- fading-circle
- folding-cube
- chasing-dots
- nuxt
- pulse
- rectangle-bounce
- rotating-plane
- three-bounce
- wandering-cubes

Built-in indicators support `color` and `background` options.

## Custom indicators

If you need your own special indicator, a String value or Name key can also be a path to a html template of indicator source code! All of the options are passed to the template, too.

Nuxt's built-in [source code](https://github.com/nuxt/nuxt.js/tree/dev/packages/vue-app/template/views/loading) is also available if you need a base!

<app-modal>
  <code-sandbox  :src="csb_link"></code-sandbox>
</app-modal>

<quiz :questions="questions"></quiz>
