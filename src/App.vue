<template>
  <h1>{{ title }}</h1>
  <memo-form />
</template>

<script>
import MemoForm from './components/MemoForm.vue'

export default {
  name: 'App',
  data() {
    return {
      title: 'Memo app'
    }
  },
  components: {
    MemoForm
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
h1 {
  border-bottom: 1px solid #ddd;
  display: inline-block;
  padding-bottom: 10px;
}
body {
  margin: 0;
  background: #eee;
}
</style>
