---
sidebarDepth: 2
---

# z-canvas

## Description
This component is a wrapper and provides the "frame" to develop your project. All your zircle's components needs to be inside `z-canvas`.

### View management
`z-canvas` has just one property called `views` that passes your views to the child component `z-view-manager` that controls how views are displayed. To achieve that **zircle-ui** uses a sort of [**navigation stack**](https://developer.apple.com/documentation/uikit/uinavigationcontroller). In this type of interface, only one view is active at a time, but with **zircle-ui** the last two views are also shown.

### Zoom transitions
The transition between views are also performed inside `z-canvas`. When a new view is triggered `z-canvas` calculates the inverted position and the inverted scale of the triggered view and apply them to the `z-canvas`.

**Caveats**
Currently zoom levels are restricted to 6. If you figure how to unlock more zoom levels let me know openning an issue or submiting a PR.

### Full and mixed modes

**zircle-ui** comes with two modes that you can change using `config()`. By default, the app is in [full mode](/api/public-api.html#config-definition) and the `z-canvas` fulfills the browser's viewport. 

In case you set your app as [mixed mode](/api/public-api.html#config-definition) your application coexists with other implementations outside the scope of **zircle-ui**.

<img :src="$withBase('/zircle-app-modes.png')" style="margin-top: 20px; border-radius: 6px" width="100%"/> 

The main difference between `full` and `mixed` mode is that in full mode **zircle-ui** pays attention to the browser's viewport, and in mixed mode the reference is the `z-canvas` itself.

You should use `full` mode to build applications solely based on **zircle-ui** and `mixed` to interact with other elements outside Zircle. In tha later case you need to add some css styles (width, height, border, etc.) into the `z-canvas`'s style or class attribute.  

```html{3}
<div id="app">
  <!-- custom style in mixed mode to fit your needs -->
  <z-canvas style="width: 40%; height: 400px; border: 2px solid red;">
  </z-canvas>
</div>
```

::: tip
You shouldn't use vue-router in `mixed` mode because the routes will interfere with your main application.
::: 

## Usage
Besides the chosen app mode, the main role of `z-canvas` is to control your views. You need to populate the z-canvas's `views` property with an `object` that holds all your defined views.

```html{3}
<div id="app">
  <z-canvas
    :views="{your views}">
  </z-canvas>
</div>
```

### How to pass your views to property `views`?

**Option 1:** Pass `$options.components`

You can include your views into the [components option](https://vuejs.org/v2/guide/components-registration.html#Local-Registration) of the [Vue instance](https://vuejs.org/v2/guide/instance.html). Then you pass the **$options.components** to prop. `views`. 


```vue{4,9-10,14-15}
<template>
  <div id="app">
    <z-canvas
      :views="$options.components">
    </z-canvas>
  </div>
</template>
<script>
  import home from './views/home.vue'
  import search from './views/search.vue'
  export default {
    /* ... */
    components: {
      home,
      search
    }
    /* ... */
  }
</script>
```

**Option 2:** Pass a `Vue data` object

Sometimes is possible you don't want to pass all your componentes (e.g: you may have different components than views). In this case, you can pass a [Vue data](https://vuejs.org/v2/api/#Options-Data) object with your views. 

```vue{4,9,14-16}
<template>
  <div id="app">
    <z-canvas
      :views="myViews">
    </z-canvas>
  </div>
</template>
<script>
  import search from './views/search.vue'
  export default {
    /* ... */
    data () {
      return {
        myViews: {
          search
        }
      }
    }
    /* ... */
  }
</script>
```
**Option 3:** Pass a custom `$options`

You can even create a custom property object in [$options](https://vuejs.org/v2/api/#vm-options) with your views.

```vue{4,9,12-14}
<template>
  <div id="app">
    <z-canvas
      :views="$options.myViews">
    </z-canvas>
  </div>
</template>
<script>
  import search from './views/search.vue'
  export default {
    /* ... */
    myViews: {
      search
    }
    /* ... */
  }
</script>
```

## How to define zircle-ui views?
If you use **zircle-ui** under **browser** or **code sandboxes** you can create views as follows:

```js
const home = "{
  template: `<z-view>Home view</z-view>`
}"
```

However, if you are working with **vue-cli 3** you should create your views [using SFC](https://vuejs.org/v2/guide/single-file-components.html):
```vue
<template>
  <z-view>
    Home view
  </z-view>
<template>
```
Then, you need to import in the **App.vue** file:


```vue
<template>
  <div id="app">
    <z-canvas
      :views="$options.components">
    </z-canvas>
  </div>
</template>
<script>
  import home from './views/home.vue'
  export default {
    /* ... */
    components: {
      home
    }
    /* ... */
  }
</script>
```



## Props
| prop | type |Description
| :--- | :--- | :--- |
| `views` | Object | An object with all your views. Each view must be a [vue component](https://vuejs.org/v2/guide/components.html).
