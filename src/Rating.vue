<template>
  <fieldset class="starability-basic rating">
    <legend v-if="legend">{{ legend }}</legend>
  <template v-for="item in items">
    <input type="radio" :id="uuid($index)" name="rating" :value="item.value" />
    <label :for="uuid($index)" :title="item.title || ''">{{ item.label || '' }}</label>
  </template>
  </fieldset>
</template>

<script>
export default {
  props: {
    legend: String,
    items: {
      type: Array,
      default: () => []
    }
  },

  methods: {
    uuid (index) {
      return `rating-${this._uid}-item-${index}`
    }
  },

  computed: {
    count () {
      return this.items.length
    }
  }
}
</script>

<style lang="scss">
@import "~starability/starability-scss/starability/all";
</style>
