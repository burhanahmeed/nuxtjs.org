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

As of Nuxt >= v2.13 there is a crawler installed that will now crawl your link tags and generate your dynamic routes based on those links. However if you have pages that are not linked to such as a secret page, then you will need to manually generate those dynamic routes.

</base-alert>

<base-alert type="next">

[Generate dynamic routes](/guides/concepts/static-site-generation) for static sites

</base-alert>

### Locally Accessing Route Params

You can access the current route parameters within your local page or component by referencing `this.$route.params.{parameterName}`. For example, if you had a dynamic users page (`users\_id.vue`) and wanted to access the `id` parameter to load the user or process information, you could access the variable like this: `this.$route.params.id`.

## Nested Routes

Nuxt.js lets you create nested routes by using the children routes of vue-router. To define the parent component of a nested route, you need to create a Vue file with the same name as the directory which contains your children views.

<base-alert>

Don't forget to include the [NuxtChild component](/guides/features/nuxt-components#the-nuxtchild-component) inside the parent component (`.vue` file).

</base-alert>

This file tree:

```
pages/
--| users/
-----| _id.vue
-----| index.vue
--| users.vue
```

will automatically generate:

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

## Dynamic Nested Routes

This is not a common scenario, but it is possible with Nuxt.js to have dynamic children inside dynamic parents.

This file tree:

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

will automatically generate:

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

## Unknown Dynamic Nested Routes

If you do not know the depth of your URL structure, you can use `_.vue` to dynamically match nested paths. This will handle requests that do not match a *more specific* request.

This file tree:

```
pages/
--| people/
-----| _id.vue
-----| index.vue
--| _.vue
--| index.vue
```

Will handle requests like this:

```
/ -> index.vue
/people -> people/index.vue
/people/123 -> people/_id.vue
/about -> _.vue
/about/careers -> _.vue
/about/careers/chicago -> _.vue
```

<base-alert type="info">

Handling 404 pages is now up to the logic of the `_.vue` page.

</base-alert>

## Extending the router

There are multiple ways to extend the routing with Nuxt:

- [router-extras-module](https://github.com/nuxt-community/router-extras-module) to customize the route parameters in the page
- component[@nuxtjs/router](https://github.com/nuxt-community/router-module) to overwrite the Nuxt router and write your own `router.js` file
- Use the [router.extendRoutes](/guides/configuration-glossary/configuration-router#extendroutes) property in your `nuxt.config.js`

## The router Property

The router property lets you customize the Nuxt.js router (vue-router).

```js{}[nuxt.config.js]
export default {
  router: {
    // customize the Nuxt.js router
  }
}
```

### Base:

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
