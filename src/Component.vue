<template>
  <component :is="tag" v-bind="binding" v-on="$listeners" class="kvass-card">
    <div v-if="hasHeader" class="kvass-card__header" :style="headerStyleComp">
      <slot name="header" />
    </div>
    <div v-if="hasContent" class="kvass-card__content">
      <div v-if="title" class="kvass-card__title">{{ title }}</div>
      <slot />
    </div>
    <div v-if="hasFooter" class="kvass-card__footer">
      <div class="kvass-card__footer-content">
        <slot name="footer" />
      </div>
    </div>
    <slot name="root" />
  </component>
</template>

<script>
export default {
  props: {
    tag: {
      type: String,
      default: 'div',
    },
    title: String,
    theme: {
      type: String,
      enums: ['default', 'border', 'flat'],
      default: 'default',
    },
    thumbnail: String,
    headerStyle: {
      type: Object,
      default: () => ({}),
    },
  },
  computed: {
    binding() {
      return {
        ...this.$attrs,
        class: ['kvass-card--' + this.theme],
      }
    },
    hasHeader() {
      return this.$slots.header || this.thumbnail !== undefined
    },
    hasFooter() {
      return this.$slots.footer
    },
    hasContent() {
      return this.$slots.default
    },
    headerStyleComp() {
      let style = {
        ...this.headerStyle,
      }
      if (this.thumbnail) style.backgroundImage = `url(${this.thumbnail})`
      return style
    },
  },
}
</script>

<style lang="scss">
.kvass-card {
  --kvass-card-card-spacing: 1.75rem;
  --kvass-card-border-radius: var(--border-radius, 3px);

  $background-grey: darken(white, 4%);
  $border-color: #e3e3e3;
  $box-shadow: 0 15px 15px -5px darken($background-grey, 6%);
  $card-background: mix($border-color, white, 20%);

  @mixin gap($size) {
    margin-left: -($size * 2);
    margin-bottom: -($size * 2);

    & > * {
      margin-left: ($size * 2);
      margin-bottom: ($size * 2);
    }
  }

  display: flex;
  flex-direction: column;
  border-radius: var(--kvass-card-border-radius);
  background-color: white;
  text-decoration: none;
  color: inherit;

  &--default {
    box-shadow: $box-shadow;
  }

  &--border {
    border: 1px solid $border-color;
  }

  &--flat {
  }

  &__header {
    min-height: 200px;
    background-position: center;
    background-size: cover;
    background-repeat: no-repeat;
    background-color: $card-background;
    border-radius: var(--kvass-card-border-radius) var(--kvass-card-border-radius) 0 0;
    border-bottom: 2px solid $card-background;

    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    &:first-child:last-child {
      flex-grow: 1;
    }
  }

  &__title {
    font-weight: bold;
    line-height: 1;
    margin-bottom: 1rem;
  }

  &__content,
  &__footer,
  &__header {
    padding: var(--kvass-card-spacing);
  }

  &__content {
    flex-grow: 1;
  }

  &__footer {
    background-color: $card-background;
    border-top: 2px solid darken($card-background, 2%);

    &-content {
      display: flex;
      flex-wrap: wrap;

      @include gap(0.5rem);
    }
  }
}
</style>
