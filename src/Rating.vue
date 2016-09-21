<template>
  <fieldset :class="[`starability-${kind}`, 'rating']">
    <legend v-if="legend">{{ legend }}</legend>
    <template v-for="(item, index) in items">
      <input type="radio" :id="uuid(index)" name="rating" :value="item.value" :checked="hasChecked(index)" @change="change($event)">
      <label class="touchable" :for="uuid(index)" :title="item.title || ''">{{ item.label || '' }}</label>
    </template>
  </fieldset>
</template>

<script>
export default {
  props: {
    legend: String,
    items: {
      type: Array,
      default: () => ([])
    },
    value: {
      type: Number,
      default: -1
    },
    kind: {
      type: String,
      default: 'basic',
      validator: (val) => ['basic', 'slot', 'grow', 'growRotate', 'fade', 'checkmark'].indexOf(val) > -1
    }
  },

  data () {
    return {
      selected: this.value
    }
  },

  methods: {
    uuid (index) {
      return `rating-${this._uid}-item-${index}`
    },

    hasChecked (index) {
      return this.count - index === this.selected
    },

    change (e) {
      this.selected = e.target.value >>> 0
      this.$emit('change', this.selected)
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
@import "~starability/starability-scss/variables";

$accessible-highlight: false;

$image-directory-path: '~starability/starability-images';

@mixin starability-base($bg-image-path: 'icons') {
  display: block;
  position: relative;
  width: $star-count * $star-size;
  min-height: 2 * $star-size;
  padding: 0;
  border: none;

  > input {
    position: absolute;
    margin-right: -100%;
    opacity: 0;
  }

  > input:checked ~ label,
  > input:focus ~ label {
    background-position: 0 (-$star-size);
  }

  @if ($hover-enabled) {
    > input:hover ~ label {
      background-position: 0 (-$star-size);
    }
  }

  @if ($accessible-highlight) {
    > input:focus + label {
      outline: 1px dotted #999;
    }
  }

  > label {
    position: relative;
    display: inline-block;
    float: right;
    width: $star-size;
    height: $star-size;
    color: transparent;
    cursor: pointer;
    background-image: url('#{$image-directory-path}/#{$bg-image-path}.png');
    background-repeat: no-repeat;

    @media screen and (min-resolution: 192dpi) {
      background-image: url('#{$image-directory-path}/#{$bg-image-path}@2x.png');
      background-size: $star-size auto;
    }
  }
}

@mixin starability-animation-base($bg-image-path: 'icons') {
  display: none;
  position: absolute;
  content: ' ';
  width: $star-size;
  height:  $star-size;
  background-image: url('#{$image-directory-path}/#{$bg-image-path}.png');
  background-repeat: no-repeat;

  @media screen and (min-resolution: 192dpi) {
    background-image: url('#{$image-directory-path}/#{$bg-image-path}@2x.png');
    background-size: $star-size auto;
  }
}

.starability-basic {
  @include starability-base;
}

.starability-slot {
  @include starability-base;

  > input:checked ~ label,
  > input:hover ~ label,
  > input:focus ~ label {
    transition: background-position .7s;
  }
}

@keyframes grow {

  0% {
    transform: scale(1,1);
  }

  99% {
    transform: scale(4,4);
    opacity: 0;
  }

  100% {
    transform: scale(1,1);
    opacity: 0;
  }
}

.starability-grow {
  @include starability-base;

  > label:before {
    @include starability-animation-base;
    bottom: 0;
  }

  > input:checked + label:before {
    background-position: 0 (-$star-size);
    display: block;
    animation-duration: 1s;
    animation-name: grow;
    animation-fill-mode: forwards;
  }
}

@keyframes grow-rotate {

  0% {
    transform: scale(1,1) rotate(0deg);
  }

  99% {
    transform: scale(4,4) rotate(90deg);
    opacity: 0;
  }

  100% {
    transform: scale(1,1) rotate(0deg);
    opacity: 0;
  }
}

.starability-growRotate {
  @include starability-base;

  > label:before {
    @include starability-animation-base;
    bottom: 0;
  }

  > input:checked + label:before {
    background-position: 0 (-$star-size);
    display: block;
    animation-duration: 1s;
    animation-name: grow-rotate;
    animation-fill-mode: forwards;
  }
}

@keyframes fade {

  0% {
    transform: translateY($star-size);
  }

  80% {
    opacity: 100%;
  }

  100% {
    transform: none;
    opacity: 0;
  }
}

.starability-fade {
  @include starability-base;

  > label:before {
    @include starability-animation-base;
    background-position: 0 (-$star-size);
    bottom: $star-size;
  }

  > input:checked + label:before {
    display: block;
    animation-name: fade;
    animation-duration: 1s;
    animation-fill-mode: forwards;
  }
}

@keyframes checkmark {

  0% {
    transform: translateX(-0.5 * $star-size);
  }

  60% {
    opacity: 1;
  }

  70% {
    transform: none;
  }

  80% {
    opacity: 1;
  }

  100% {
    opacity: 0;
  }
}

.starability-checkmark {
  @include starability-base('icons-checkmark');

  > label {
    position: static;
    z-index: 2;
  }

  > label:before {
    @include starability-animation-base('icons-checkmark');
    background-position: 0 (-2*$star-size);
    right: -$star-size;
  }

  > input:checked + label:before {
    display: block;
    animation-name: checkmark;
    animation-duration: .7s;
    animation-fill-mode: forwards;
  }
}
</style>
