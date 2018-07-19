<template>
  <div class="container">
    <div class="category" >
       {{categories[0].Caption}}
    </div>
    <div class="small-container">
    <div v-if="result.GroupId == 'g1'" class="card" v-for="result in results">
      <h4>{{result.Caption}}</h4>
      <img v-bind:src="result.Img" alt="">
    </div>
      </div>
      <div class="category" >
         {{categories[1].Caption}}
      </div>
    <div class="small-container">
    <div v-if="result.GroupId == 'g2'" class="card" v-for="result in results" >
      <h4>{{result.Caption}}</h4>
      <img v-bind:src="result.Img" alt="">
    </div>
    </div>
    <div class="category" >
       {{categories[2].Caption}}
    </div>
    <div class="small-container">
    <div v-if="result.GroupId == 'g3'"  class="card" v-for="result in results" >
      <h4>{{result.Caption}}</h4>
      <img v-bind:src="result.Img" alt="">
    </div>
</div>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      results: [],
      cards: [],
      categories: []
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

    .then(response => {
      response.json().then(data => ({
        data: data,
        status: response.status
    })
    ).then(res => {

  this.results =  JSON.parse(res.data.d)
    console.log(this.results )
    })
  }
  )
},
getCategory () {
  fetch('http://m.it.ua/ws/WebService.asmx/ExecuteEx', {
    method: 'POST',
    body: JSON.stringify({

      calcId: '_MOCK.GET_GROUPS',
      args: null,
      ticket: null

    }),
    headers: {
    Accept: 'json',
    'Content-Type': 'application/json',
  }
})    .then(response => {
      response.json().then(data => ({
        data: data,
        status: response.status
    })
    ).then(res => {

  this.categories =  JSON.parse(res.data.d)
    console.log(this.categories )
    })
  }
  )
}

},
created: function() {
  this.getInfo();
  this.getCategory();
}

  }

</script>

Add "scoped" attribute to limit CSS to this component only
<style>
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

.small-container {
  display: flex;
  flex-wrap: wrap;
  /* justify-content: space-around; */
}
.card {
  /* width: 20vw;
  height: 20vh; */
  border: 2px solid #103370;
  margin: 5px 5px;
}

img {
  width: 20vw;
  height: 20vh;
}
h4 {
  width: 20vw;
}

.category{
  width: 80vw;
  height: 10vh;
  background-color:  #103370;
  color: #fff;
  font-size: 130%;
}
</style>
