<template lang="pug">
  router-link(
    v-if="href"
    tag="button"
    class="c-button"
    :class="classNames"
    :to="href"
  )
    c-icon(v-if="iconName" :name="iconName" valign="middle")
    span(v-if="$slots.default")
      slot
  button(
    v-else
    class="c-button"
    :type="type"
    :class="classNames"
    @click="onClick"
  )
    c-icon(v-if="iconName" :name="iconName" valign="middle")
    span(v-if="$slots.default")
      slot
</template>

<script>
// import css
import './index.css'

import { toVueProps, toClassNames } from '@util'
import Icon from '../icon/index.vue'

const name = 'c-button'
const block = `c-button`
const modifiers = [
  'primary',
  'success',
  'warning',
  'danger',
  'round',
  'outline',
  'flat',
  'loading'
]
const props = Object.assign(
  {
    href: String,
    size: String,
    icon: String,
    type: {
      type: String,
      default: 'button'
    },
    autofocus: Boolean
  },
  toVueProps(modifiers)
)
const classNames = toClassNames(block, modifiers)

export default {
  name,
  props,
  components: {
    'c-icon': Icon
  },
  inject: {
    $form: { default: null }
  },
  computed: {
    iconName () {
      return this.loading ? 'loader' : this.icon
    },
    actualSize () {
      const { size, $form } = this
      return size || ($form && $form.size)
    },
    classNames () {
      const classList = classNames.call(this)
      const size = this.actualSize
      if (size) classList.push(`is-${size}`)
      return classList
    }
  },
  methods: {
    onClick (e) {
      this.$emit('click', e)
    }
  },
  mounted () {
    if (this.autofocus) {
      this.$el.focus()
    }
  }
}
</script>
