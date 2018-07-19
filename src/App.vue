<template>
  <div id="app">
    <header>
      <h3>{{title}}</h3>
    </header>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      title: 'Catalog'
    }
  }
}
</script>

<style>
body {
  margin: 0;
}
p, h3 {
  margin: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
  max-width: 1440px;
}

header {
  width: 100%;
  background-color: #6c737f;
  height: 80px;
}
</style>
