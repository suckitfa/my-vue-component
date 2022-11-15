<template>
  <div>
    <label v-if="label" :class="{'i-form-item-label-required':isRequired}" >{{label}}</label>
    <div>
      <slot></slot>
      <div v-if="validateState === 'error'" class="i-form-item-message">
      </div>
    </div>
  </div>
</template>
<script>
import AsyncValidator from 'async-validator'
export default {
  name:"iFormItem",
  props: {
    label: {
      type:String,
      default:""
    },
    prop: {
      type:String
    }
  },
  computed:{
    filedValue(){
      return this.form.model[this.prop];
    }
  },
  data() {
    return {
      isRequired:false, // 是否必填
      validateState:'', // 校验状态
      validateMessage:'' // 校验提示信息
    }
  },
  methods: {
    getRules() {
      let formRules = this.form.rules;
      
      formRules = formRules ? formRules[this.prop] : []
      return [].concat(formRules || []);
    },
    getFilteredRule(trigger) {
      const rules = this.getRules();
      return rules.filter(rule => !rule.trigger || rule.trigger.indexOf(trigger) !== -1);
    },
    validate(trigger, callback=function(){}) {
      let rules = this.getFilteredRule(trigger)

      if (!rules || rules.length === 0) {
        return true;
      }

      // 设置校验状态未教
      this.validateState = 'validating';

      // 以下为async-validator库的用途
      let descriptor = {};
      descriptor[this.prop] = rules;

      const validator = new AsyncValidator(descriptor);
      let model = {};
      model[this.prop] = this.filedValue;

      validator.validate(model,{firstFields:true}, errors => {
        this.validateState = !errors ? 'success' : 'error';
        this.validateMessage = errors ? errors[0].message : "";

        callback(this.validateMessage)
      })
    },
    resetField() {
      this.validateMessage = '';
      this.validateState = '';
      
      this.form.model[this.prop] = this.initialValue;
    }
  },
  mounted() {
    if (this.prop) {
      this.dispatch('iForm','on-form-item-add',this);

      // 设置初设值
      this.initialValue = this.filedValue;
      this.setRules();
    }
  },
  beforeDestroy() {
    this.dispatch('iForm','on-form-item-remove',this);
  }
}
</script>

<style>
  .i-form-item-label-required::before {
    content:"*";
    color:red;
  }

  .i-form-item-message {
    color:red;
  }
</style>