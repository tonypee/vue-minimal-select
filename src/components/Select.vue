<style scoped lang="less">
.select-container {
  position: relative;
  div.select-menu {
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
      <div class="select-menu" v-show="show" @click="onClick">
        <span v-if="!multiple && showSearch">
          <input v-model="search" ref="search" class="search" placeholder="Search Items..." />
        </span>
        <div v-for="option of filteredObjects">
          <a @click="select(option.value)">{{ option.label }}</a>
        </div>
      </div>
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
      showSearch: { type: Boolean, default: false },
    },
    directives: { ClickOutside },
    data() {
      return {
        show: false,
        disabled: false,
        search: '',
      }
    },
    computed: {
      filteredObjects() {
        return this.optionObjects.filter(o => {
          return !this.search.length || ~o.label.toLowerCase().indexOf(this.search.toLowerCase())
        })
      },
      optionObjects() {
        return this.options.map(option => {
          return isObject(option) ? {
            label: option[this.labelFrom],
            value: option[this.valueFrom],
          } : {
            label: option,
            value: option,
          }
        })
      },
      labelMap() {
        return this.optionObjects.reduce((o, v) => {
          o[v.value] = v.label
          return o
        }, {})
      }
    },
    methods: {
      getLabel(value) {
        return this.labelMap[value]
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

      onClick(e) {
        if (e.target === this.$refs.search) {
          return
        }
        this.hide()
      },

      hide() {
        this.show = false
      },
    }
  }
</script>
