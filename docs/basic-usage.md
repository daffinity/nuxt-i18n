# Basic Usage

The fastest way to get started with **nuxt-i18n** is to define the supported `locales` list and to provide some translations messages to **vue-i18n** via the `vueI18n` option:

```js
{
  modules: [
    'nuxt-i18n'
  ],

  i18n: {
    locales: ['en', 'fr', 'es'],
    defaultLocale: 'en',
    vueI18n: {
      fallbackLocale: 'en',
      messages: {
        en: {
          welcome: 'Welcome'
        },
        fr: {
          welcome: 'Bienvenue'
        },
        es: {
          welcome: 'Bienvenido'
        }
      }
    }
  }
}
```

With this setup, **nuxt-i18n** generates localized URLs for all your pages, using the codes provided in the `locales` option as the prefix, except for the `defaultLocale` (read more on [routing](/routing.md)).

The `vueI18n` option is passed as is to **vue-i18n**, refer to the [doc](https://kazupon.github.io/vue-i18n/) for available options.

## nuxt-link

When rendering internal links in your app using `<nuxt-link>`, you need to get proper URLs for the current locale. To do this, **nuxt-i18n** registers a global mixin that provides some helper functions:

* `localePath` – Returns the localized URL for a given page. The first parameter can be either the name of the route or an object for more complex routes. A locale code can be passed as the second parameter to generate a link for a specific language:

```vue
<nuxt-link :to="localePath('index')">{{ $t('home') }}</nuxt-link>
<nuxt-link :to="localePath('index', 'en')">Homepage in English</nuxt-link>
<nuxt-link
  :to="localePath({ name: 'category-slug', params: { slug: category.slug } })">
  {{ category.title }}
</nuxt-link>
```

Note that `localePath` uses the route's base name to generate the localized URL. The base name corresponds to the names Nuxt generates when parsing your `pages/` directory, more info in [Nuxt's doc](https://nuxtjs.org/guide/routing).

* `switchLocalePath` – Returns a link to the current page in another language:

```vue
<nuxt-link :to="switchLocalePath('en')">English</nuxt-link>
<nuxt-link :to="switchLocalePath('fr')">Français</nuxt-link>
```

For convenience, these methods are also available in the app's context:

```js
// ~/plugins/myplugin.js

export default ({ app }) => {
  // Get localized path for homepage
  const localePath = app.localePath('index')
  // Get path to switch current route to French
  const switchLocalePath = app.switchLocalePath('fr')
}
```
