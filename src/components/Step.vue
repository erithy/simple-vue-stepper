<template>
  <div :class="['v-step', classes]">
    <input
      class="input"
      type="radio"
      :id="id"
      :name="computedName"
      :checked="active"
      @change="handleChange"
      v-show="debug"
    >
    <label class="label" :for="id">
      <span class="index">
        <slot name="index" v-bind="api">
          {{api.displayIndex}}
        </slot>
      </span>
      <span class="title" v-if="defaultSlot">
        <slot v-bind="api"></slot>
      </span>
      <span class="divider" v-if="withDivider"></span>
    </label>
  </div>
</template>

<script>
export default {
  name: 'VStep',
  props: {
    index: {
      type: Number,
      default: 0
    },
    name: {
      type: String,
      default: ''
    },
    active: {
      type: Boolean,
      default: false
    },
    visited: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    },
    withDivider: {
      type: Boolean,
      default: false
    },
    debug: {
      type: Boolean,
      default: true
    }
  },
  data: () => ({
    namespace: 'v-step'
  }),
  computed: {
    id() {
      return `${this.namespace}-${this._uid}-${this.index}`
    },
    displayIndex() {
      return this.index + 1
    },
    classes() {
      return {
        'is-active': this.active,
        'is-visited': this.visited,
        'is-disabled': this.disabled
      }
    },
    computedName() {
      return this.name || this.id
    },
    defaultSlot() {
      return this.$slots.default || this.$scopedSlots.default
    },
    api() {
      const { displayIndex, defaultSlot } = this
      return { displayIndex, defaultSlot }
    }
  },
  methods: {
    handleChange() {
      this.$emit('change', this.index)
    }
  },
  inheritAttrs: false
}
</script>

<style lang="scss" scoped>
@import '~@/sass/colors';

.v-step {
  flex: 1;
  opacity: 0.55;
  box-sizing: border-box;
  transition: opacity 0.7s;

  &:hover:not(.is-disabled) {
    opacity: 0.85;
  }

  *,
  *::before,
  *::after {
    box-sizing: inherit;
  }

  &.is-active,
  &.is-visited {
    .label {
      cursor: pointer;
    }

    .index {
      color: $app-color-white;
    }
  }

  &.is-active {
    opacity: 1;

    .label {
      .index {
        background-color: $bs-primary;
      }
    }
  }

  &.is-visited {
    .index {
      background-color: $bs-secondary;
    }
  }

  @media (max-width: 575px) {
    // Bootstrap "xs"
    margin-right: 0.5rem;
  }
}

.label {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.index {
  width: 2.5rem;
  height: 2.5rem;
  display: flex;
  flex-shrink: 0;
  font-size: 1rem;
  border-radius: 50%;
  margin-right: 0.5rem;
  color: $app-color-white;
  align-items: center;
  justify-content: center;
  background-color: $bs-secondary;
}

.title {
  color: $app-color-white;
}

.divider {
  width: 100%;
  margin-left: 0.5rem;
  border-bottom: 1px solid $app-color-white;
  box-shadow: 1px 1px 1px rgba(0, 0, 0, 0.2);
}
</style>