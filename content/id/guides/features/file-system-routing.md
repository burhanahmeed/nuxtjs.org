---
title: File System Routing
description: Nuxt.js akan secara otomatis membuat konfigurasi vue-router berdasarkan turunan berkas dari berkas Vue di dalam direktori _pages_. Ketika Anda membuat berkas .vue di direktori _pages_, Anda akan memiliki _routing_ paling dasar terpasang tanpa tambahan konfigurasi lainnya.
position: 3
category: features
questions:
  - question: Apa nama komponen yang digunakan untuk melakukan navigasi diantara halaman?
    answers:
      - '<a>'
      - '<NuxtLink>'
      - '<Nuxt>'
    correctAnswer: '<NuxtLink>'
  - question: Apa yang Anda lakukan untuk menghasilkan konfigurasi _router_ secara otomatis?
    answers:
      - menambahkan berkas .vue pada direktori _pages_
      - membuat berkas router.config
      - menambahkan <NuxtLink> pada halaman
    correctAnswer: menambahkan berkas .vue pada direktori _pages_
  - question: Yang mana dari berikut ini yang tidak akan membuat rute dinamis?
    answers:
      - dynamic.vue
      - _slug.vue
      - _slug/index.vue
    correctAnswer: dynamic.vue
  - question: rute dinamis diabaikan oleh perintah _nuxt generate_?
    answers:
      - True
      - False
    correctAnswer: False
  - question: How do you access the route params for a dynamic page such as users/_id.vue?
    answers:
      - $route.params.id
      - $route.id
      - $route.params.users.id
    correctAnswer: $route.params.id
  - question: Bagaimana Anda mendefinisikan komponen induk dari rute yang bersarang?
    answers:
      - membuat berkas Vue yang dipanggil oleh induk di dalam direktori yang terdapat _views_
      - membuat berkas Vue dengan nama yang berbeda dengan direktori yang terdapat _views_
      - membuat berkas Vue dengan nama yang sama dengan direktori yang terdapat _views_
    correctAnswer: membuat berkas Vue dengan nama yang sama dengan direktori yang terdapat _views_
  - question: Jika Anda tidak mengetahui seberapa dalam struktur _URL_, Anda dapat menggunakan berkas mana yang secara dinamis cocok dengan alur yang bersarang?
    answers:
      - _.vue
      - _index.vue
      - _id.vue
    correctAnswer: _.vue
  - question: Mana komponen yang dapat digunakan untuk me-_render_ _views_?
    answers:
      - '<Nuxt> dan <Child>'
      - '<Nuxt> dan <NuxtChild>'
      - '<NuxtChild> dan <NuxtLink>'
    correctAnswer: '<Nuxt> dan <NuxtChild>'
  - question: Di Nuxt.js, berkas mana yang dapat Anda gunakan untuk membuat pemaksaan posisi gulir (_scroll_) ke atas pada setiap rute?
    answers:
      - app/router.scrollBehavior.js
      - app/scrollBehavior.js
      - app/router.js
    correctAnswer: app/router.scrollBehavior.js
  - question: Di Nuxt.js Anda dapat menambahkan pemotong (_slash_) yang akan dimasukkan ke setiap rute?
    answers:
      - true
      - false
    correctAnswer: true
---

Nuxt.js akan secara otomatis membuat konfigurasi vue-router berdasarkan turunan berkas dari berkas Vue di dalam direktori _pages_. Ketika Anda membuat berkas .vue di direktori _pages_, Anda akan memiliki _routing_ paling dasar terpasang tanpa tambahan konfigurasi lainnya.

Terkadang Anda mungin perlu membuat rute dinamis atau rute bersarang atau Anda perlu konfigurasi rute lebih lanjut. Pada bagian ini akan memberi tahu Anda segalanya apa yang Anda perlu tahu untuk mengoptimalkan konfigurasi _router_.

<base-alert type="info">

Nuxt.js memberikan Anda pemisahan kode secara otomatis untuk rute. tidak ada konfigurasi yang dibutuhkan.

</base-alert>

<base-alert type="info">

Gunakan [komponen NuxtLink](/guides/features/nuxt-components#the-nuxtlink-component) untuk melakukan navigasi antara halaman.

</base-alert>

```html
<template>
  <nuxt-link to="/">Halaman Beranda</nuxt-link>
</template>
```

## Rute Dasar

Turunan berkas ini:

```
pages/
--| user/
-----| index.vue
-----| one.vue
--| index.vue
```

akan secara otomatis menghasilkan:

```js
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'user',
      path: '/user',
      component: 'pages/user/index.vue'
    },
    {
      name: 'user-one',
      path: '/user/one',
      component: 'pages/user/one.vue'
    }
  ]
}
```

## Rute Dinamis

Terkadang ini tidak memungkinkan untuk mengetahui nama dari rute tersebut ketika kita memanggil sebuah program aplikasi antar muka (_api_) untuk mendapatkan daftar pengguna atau postingan _blog_. Kita memanggil rute dinamis tersebut. Untuk membuat rute dinamis Anda perlu membuat berkas yang diawali dengan nama berkas (\_) sebelum .vue atau sebelum nama dari direktori. Anda dapat memberikan nama berkas atau direktori apapun yang Anda mau namun Anda harus memberikan awalan dengan garis bawah.

Turunan berkas ini:

```
pages/
--| _slug/
-----| comments.vue
-----| index.vue
--| users/
-----| _id.vue
--| index.vue
```

akan secara otomatis menghasilkan:

```js
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'users-id',
      path: '/users/:id?',
      component: 'pages/users/_id.vue'
    },
    {
      name: 'slug',
      path: '/:slug',
      component: 'pages/_slug/index.vue'
    },
    {
      name: 'slug-comments',
      path: '/:slug/comments',
      component: 'pages/_slug/comments.vue'
    }
  ]
}
```

<base-alert type="info">

Seperti yang dapat Anda lihat, nama rute `users-id` memiliki jalur `:id?` yang membuatnya opsional, jika Anda ingin membuatnya wajib maka buatlah sebuah `index.vue` di direktori `users/_id`.

</base-alert>

<base-alert type="info">

Pada Nuxt >= v2.13, terdapat _crawler_ yang terpasang dan akan melakukan _crawl_ penanda tautan Anda dan menghasilkan rute dinamis bedasarkan tautan tersebut. Bagaimanapun itu jika Anda memiliki halaman yang tidak terhubung ke halaman yang rahasia, maka Anda akan memerlukan membuat rute dinamis tersebut secara manual.

</base-alert>

<base-alert type="next">

[Pembuat situs statis](/guides/concepts/static-site-generation) untuk situs statis.

</base-alert>

### Mengakses parameter _Route_ secara Lokal

Anda dapat mengakses parameter rute saat ini didalam halaman atau komponen dengan mengambil referensi `this.$route.params.{parameterName}`. Contohnya, jika Anda memiliki halaman pengguna dinamis (`users\_id.vue`) dan ingin mengakses parameter `id` untuk memuat informasi pengguna, Anda bisa melakukan akses dengan kode seperti berikut ini `this.$route.params.id`.

## Rute Bersarang (_Nested_)

Nuxt.js mengizinkan Anda untuk membuat rute bersarang dengan menggunakan rute anak dari vue-router. Untuk mendefinisikan komponen induk dari rute bersarang, Anda perlu membuat berkas Vue dengan nama yang sama dengan direktori yang mengandung _views_.

<base-alert>

Jangan lupa untuk memasukkan [komponen NuxtChild](/guides/features/nuxt-components#the-nuxtchild-component) di dalam komponen induk (`.vue` file).

</base-alert>

Turunan berkas ini:

```
pages/
--| users/
-----| _id.vue
-----| index.vue
--| users.vue
```

akan secara otomatis menghasilkan:

```js
router: {
  routes: [
    {
      path: '/users',
      component: 'pages/users.vue',
      children: [
        {
          path: '',
          component: 'pages/users/index.vue',
          name: 'users'
        },
        {
          path: ':id',
          component: 'pages/users/_id.vue',
          name: 'users-id'
        }
      ]
    }
  ]
}
```

## Rute Bersarang Dinamis

Ini bukanlah skenario yang umum, namun ini memungkinkan dengan Nuxt.js untuk memiliki rute anak yang dinamis di dalam rute induk yang dinamis.

Turunan berkas ini:

```
pages/
--| _category/
-----| _subCategory/
--------| _id.vue
--------| index.vue
-----| _subCategory.vue
-----| index.vue
--| _category.vue
--| index.vue
```

akan secara otomatis menghasilkan:

```js
router: {
  routes: [
    {
      path: '/',
      component: 'pages/index.vue',
      name: 'index'
    },
    {
      path: '/:category',
      component: 'pages/_category.vue',
      children: [
        {
          path: '',
          component: 'pages/_category/index.vue',
          name: 'category'
        },
        {
          path: ':subCategory',
          component: 'pages/_category/_subCategory.vue',
          children: [
            {
              path: '',
              component: 'pages/_category/_subCategory/index.vue',
              name: 'category-subCategory'
            },
            {
              path: ':id',
              component: 'pages/_category/_subCategory/_id.vue',
              name: 'category-subCategory-id'
            }
          ]
        }
      ]
    }
  ]
}
```

## Rute Bersarang Dinamis yang Tidak Diketahui

Jika Anda tidak mengatahui seberapa dalam struktur _URL_ Anda, Anda dapat menggunakan `_.vue` untuk membuat rute bersarang secara dinamis. Hal ini akan mengarahkan permintaan yang tidak sesuai dengan permintaan rute yang spesifik.

Turunan berkas ini:

```
pages/
--| people/
-----| _id.vue
-----| index.vue
--| _.vue
--| index.vue
```

Ini akan menangani permintaan seperti ini:

```
/ -> index.vue
/people -> people/index.vue
/people/123 -> people/_id.vue
/about -> _.vue
/about/careers -> _.vue
/about/careers/chicago -> _.vue
```

<base-alert type="info">

Menangani halaman 404 sekarang bisa dilakukan di dalam logika kode dari halaman `_.vue`.

</base-alert>

## Memperpanjang _router_

Ada beberapa cara untuk memperpanjang rute pada Nuxt:

- [router-extras-module](https://github.com/nuxt-community/router-extras-module) untuk melakukan kustomisasi parameter rute di dalam halaman
- Komponen [@nuxtjs/router](https://github.com/nuxt-community/router-module) untuk menimpa _router_ Nuxt dan menuliskan berkas `router.js` milik anda sendiri
- Gunakan properti [router.extendRoutes](/guides/configuration-glossary/configuration-router#extendroutes) di `nuxt.config.js`

## Properti _router_

Properti _router_ mengizinkan Anda melakukan kustomisasi _router_ Nuxt.js (vue-router).

```js{}[nuxt.config.js]
export default {
  router: {
    // mengkustom router Nuxt.js
  }
}
```

### Dasar:

The base URL of the app. For example, if the entire single page application is served under `/app/`, then base should use the value `'/app/'`.

<base-alert type="next">

[Router Base Property](/guides/configuration-glossary/configuration-router#base)

</base-alert>

### extendRoutes

You may want to extend the routes created by Nuxt.js. You can do so via the `extendRoutes` option.

Example of adding a custom route:

```js{}[nuxt.config.js]
export default {
  router: {
    extendRoutes(routes, resolve) {
      routes.push({
        name: 'custom',
        path: '*',
        component: resolve(__dirname, 'pages/404.vue')
      })
    }
  }
}
```

If you want to sort your routes, you can use the  `sortRoutes(routes)`  function from `@nuxt/utils`:

```js{}[nuxt.config.js]
import { sortRoutes } from '@nuxt/utils'
export default {
  router: {
    extendRoutes(routes, resolve) {
      // Add some routes here ...

      // and then sort them
      sortRoutes(routes)
    }
  }
}
```

<base-alert>

The schema of the route should respect the [vue-router](https://router.vuejs.org/en/) schema.

</base-alert>

<base-alert>

When adding routes that use [Named Views](https://nuxtjs.org/guide/routing#named-views), don't forget to add the corresponding `chunkNames` of named `components`.

</base-alert>

```js{}[nuxt.config.js]
export default {
  router: {
    extendRoutes(routes, resolve) {
      routes.push({
        path: '/users/:id',
        components: {
          default: resolve(__dirname, 'pages/users'), // or routes[index].component
          modal: resolve(__dirname, 'components/modal.vue')
        },
        chunkNames: {
          modal: 'components/modal'
        }
      })
    }
  }
}
```

<base-alert type="next">

[extendRoutes Property](/guides/configuration-glossary/configuration-router#extendroutes)

</base-alert>

### fallback

Controls whether the router should fallback to hash mode when the browser does not support history.pushState but mode is set to history.

<base-alert type="next">

[fallback Property](/guides/configuration-glossary/configuration-router#fallback)

</base-alert>

### mode

Configure the router mode, it is not recommended to change it due to server-side rendering.

<base-alert type="next">

[mode Property](/guides/configuration-glossary/configuration-router#mode)

</base-alert>

### parseQuery / stringifyQuery

Provide custom query string parse / stringify functions.

<base-alert type="next">

[parseQuery / stringifyQuery Property](/guides/configuration-glossary/configuration-router#parsequery--stringifyquery)

</base-alert>

### routeNameSplitter

You may want to change the separator between route names that Nuxt.js uses. You can do so via the `routeNameSplitter` option in your configuration file. Imagine we have the page file `pages/posts/_id.vue`. Nuxt.js will generate the route name programmatically, in this case `posts-id`. Changing the `routeNameSplitter` config to `/` the name will therefore change to `posts/id`.

```js{}[nuxt.config.js]
export default {
  router: {
    routeNameSplitter: '/'
  }
}
```

### scrollBehavior

The `scrollBehavior` option lets you define a custom behavior for the scroll position between the routes. This method is called every time a page is rendered.

<base-alert type="next">

To learn more about it, see [vue-router scrollBehavior documentation](https://router.vuejs.org/guide/advanced/scroll-behavior.html).

</base-alert>

Available since:v2.9.0:

In Nuxt.js you can use a file to overwrite the router scrollBehavior. This file should be placed in a folder called app.

`~/app/router.scrollBehavior.js`.

Example of forcing the scroll position to the top for every route:

```js{}[app/router.scrollBehavior.js]
export default function (to, from, savedPosition) {
  return { x: 0, y: 0 }
}
```

<base-alert type="next">

[Nuxt.js default `router.scrollBehavior.js` file.](https://github.com/nuxt/nuxt.js/blob/dev/packages/vue-app/template/router.scrollBehavior.js)

</base-alert>

<base-alert type="next">

[scrollBehavior Property](/guides/configuration-glossary/configuration-router#scrollbehavior)

</base-alert>

### trailingSlash

Available since: v2.10

If this option is set to true, trailing slashes will be appended to every route. If set to false, they'll be removed.

```js{}[nuxt.config.js]
export default {
  router: {
    trailingSlash: true
  }
}
```

<base-alert>

This option should not be set without preparation and has to be tested thoroughly. When setting `router.trailingSlash` to something else other than `undefined`(which is the default value), the opposite route will stop working. Thus 301 redirects should be in place and your *internal linking* has to be adapted correctly. If you set `trailingSlash` to `true`, then only `example.com/abc/` will work but not `example.com/abc`. On false, it's vice-versa.

</base-alert>

<base-alert type="next">

[trailingSlash Property](/guides/configuration-glossary/configuration-router#trailingslash)

</base-alert>

<quiz :questions="questions"></quiz>
