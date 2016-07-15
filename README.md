# Rating

Rating component is based on [starability](http://lunarlogic.github.io/starability/) for Vue Bulma.

## Installation

```
$ npm install vue-bulma-rating
```

## Examples

```vue
<template>
  <rating :items="items" legend="Default star rating:"></rating>
</template>

<script>
import Rating from 'vue-bulma-rating'

export default {
  components: {
    Rating
  },

  data () {
    return {
      items: [
        {
          title: '5 Stars',
          value: 5
        },
        {
          title: '4 Stars',
          value: 4
        },
        {
          title: '3 Stars',
          value: 3
        },
        {
          title: '2 Stars',
          value: 2
        },
        {
          title: '1 Star',
          value: 1
        }
      ]
    }
  }
}
</script>

```


## Badges

![](https://img.shields.io/badge/license-MIT-blue.svg)
![](https://img.shields.io/badge/status-stable-green.svg)

---

> [fundon.me](https://fundun.me) &nbsp;&middot;&nbsp;
> GitHub [@fundon](https://github.com/fundon) &nbsp;&middot;&nbsp;
> Twitter [@_fundon](https://twitter.com/_fundon)

