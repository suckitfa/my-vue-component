<template>
  <form >
    <slot></slot>
  </form>
</template>

<script>
import { resolve } from 'path'

export default {
  name:"iForm",
  props: {
    model: {
      type:Object
    },
    rules: {
      type:Object
    }
  },  
  provide(){
    return {
      form:this
    }
  },
  data() {
    return {
      fields:[]
    }
  },
  methods: {
    resetFields() {
      this.fields.forEach(field => {
        field.resetField()
      })
    },
    validate(callback) {
      let valid = true;
      let count = 0;
      this.fields.forEach(field => {
        field.validate('',errors => {
          if (errors) {
            valid = false
          }

          // 所有校验通过
         if (++count === this.fields.length) {
          resolve(valid)
          if (typeof callback === 'function') {
            callback(valid);
          }
         }
        })
      })
    }
  },
  created() {
    this.$on('on-form-item-add', (field) => {
      if (field) this.fields.push(field)
    })

    this.$on('on-form-item-remove',field => {
      if (field.prop) this.fields.splice(this.fields.indexOf(field),1);
    })
  }
}
</script>