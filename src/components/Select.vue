<style scoped lang="less">
.select-container {
  position: relative;
  ul.select-menu {
    z-index: 999;
    list-style-type: none;
    margin: 0;
    padding: 0;
    box-shadow: 0px 3px 5px rgba(0, 0, 0, .175);
    position: absolute;
    border: 1px solid #ccc;
    border-radius: 5px;
    background: white;
    max-height: 250px;
    overflow: scroll;

    a {
      display: inline-block;
      padding: 7px 15px;
      &:hover {
        cursor: pointer;
      }
    }
    li {
      margin: 0;
    }
  }

  .caret {
    position: relative;
    top: 5px;
  }
  .caret:before {
    position: relative;
    right: 0;
    top: 65%;
    color: #999;
    margin-top: 4px;
    border-style: solid;
    border-width: 5px 5px 0 5px;
    border-color: #999999 transparent transparent transparent;
    content: "";
  }
}
</style>
<template>
  <div class="select-container" v-click-outside="blur">
    <slot name="before"></slot>
    <slot name="button">
      <button type="button" @click="toggle" @keyup.esc="show = false" :disabled="disabled">

        <span v-if="!multiple">{{ getLabel(value) || this.placeholder }}</span>

        <span v-if="multiple" >
          <span v-for="(val, index) of value">
            {{ getLabel(val) }}
            {{ index < (value.length -1) ? ',' : '' }}
          </span>
          {{ value.length == 0 ? this.placeholder : '' }}
        </span>

        <span class="caret"></span>
      </button>
    </slot>
    <slot name="select-menu">
      <ul class="select-menu" v-show="show" @click="onClick">
        <div v-for="option of optionObjects">
          <a @click="select(option.value)">{{ option.label }}</a>
        </div>
      </ul>
    </slot>
  </div>
</template>
<script>
  import ClickOutside from '../directives/ClickOutside.js'

  function isObject(object) {
    return object !== null && typeof object === 'object'
  }

  export default {
    props: {
      value: {},
      options: { type: Array, default: [] },
      text: { type: String },
      multiple: { type: Boolean, default: false },
      placeholder: { type: String, default: '- Select -' },
      valueFrom: { type: String, default: 'value' },
      labelFrom: { type: String, default: 'label' },
    },
    directives: { ClickOutside },
    data() {
      return {
        show: false,
        disabled: false,
      }
    },
    computed: {
      optionObjects() {
        return this.options.map(option => {
          return isObject({option}) ? {
            label: option[this.labelFrom],
            value: option[this.valueFrom],
          } : {
            label: option,
            value: option,
          }
        })
      }
    },
    methods: {
      findOption(value) {
        return this.optionObjects.find(o => o.value === value)
      },

      getLabel(value) {
        const obj = this.findOption(value)
        return obj && obj.label
      },

      select(value) {
        if (this.multiple) {
          const existing = this.value.concat()
          if (!~existing.indexOf(value)) {
            existing.push(value)
          } else {
            existing.splice(existing.indexOf(value), 1)
          }
          this.$emit('input', existing)
        } else {
          this.$emit('input', value)
        }
      },

      toggle() {
        this.show = !this.show
      },

      blur() {
        this.hide()
      },

      onClick() {
        this.hide()
      },

      hide() {
        this.show = false
      },
    }
  }
</script>
