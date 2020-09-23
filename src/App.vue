<template>
<div>
  Hello World
</div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'app',
  data() {
    return {
    }
  },
  mounted() {
    axios
      .get('/static/data.json')
      .then(response => console.log(response))
  }
}
</script>

<style lang="scss" scoped>
body {
    font-family: Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}
</style>
