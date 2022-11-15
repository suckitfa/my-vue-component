<template>
  <button 
    :class="'i-button-size'+size" 
    :disabled="disabled"
    @click="handleClick"
  >
    <slot name="icon"></slot>
    <slot></slot>
  </button>
</template>
<script>
export default {
  name:'MyButton',
  inject:['personInfo'],
  props: {
    size: {
      validator(val) {
        return ['large','small','default'].includes(val)
      },
      default:'default'
    },
    disabled: {
      type:Boolean,
      default:false,
    }
  },
  methods: {
    handleClick(e) {
      this.$emit('on-click',e,'子组件被点击了')
      console.log('personInfo = ',this.personInfo)
    }
  }
}
</script>