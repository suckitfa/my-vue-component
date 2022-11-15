<template>
  <div id="app">
    <my-button 
      @on-click="handleAppClick"
    >点击</my-button>
  </div>
</template>

<script>
import MyButton from './components/MyButton.vue'
export default {
  name: 'App',
  provide: {
    personInfo: {
      age:19,
      name:"bob"
    }
  },  
  components: {
    MyButton,
  },
  methods: {
    handleAppClick(e,msg) {
      console.log(e,msg)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
