<template>
  <div class="hello">

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  methods: {
    getInfo () {

  fetch('http://m.it.ua/ws/WebService.asmx/ExecuteEx', {
    method: 'POST',
    // contentType: 'application/json; charset=windows-1251',
    // dataType: 'json',
    body: JSON.stringify({

calcId: '_MOCK.GET_PRODUCTS',
args: null,
ticket: null

    }),
    headers: {
    Accept: 'json',
    'Content-Type': 'application/json',
  }
})
    .then(response =>{
       if (response.ok) {  console.log(response.json())}
      // throw new Error("Something bad");
})
    .catch(error => {
      console.error("Error:", error);
})
}
},
created: function() {
  this.getInfo()
}

  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
