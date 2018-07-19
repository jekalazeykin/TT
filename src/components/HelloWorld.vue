<template>
  <div class="hello">
    <div class="card" v-for="card in cards">
    <!-- <div v-if="result.GroupId == 'g1'">
      <h3>{{result.Caption}}</h3>
      <img v-bind:src="result.Img" alt="">
    </div>
    <div v-else-if="result.GroupId == 'g2'">
      <h3>{{result.Caption}}</h3>
      <img v-bind:src="result.Img" alt="">
    </div>
    <div v-else-if="result.GroupId == 'g3'">
      <h3>{{result.Caption}}</h3>
      <img v-bind:src="result.Img" alt="">
    </div> -->

    <h3>{{card.Caption}}</h3>
    <img v-bind:src="card.Img" alt="">

      </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      results: [],
      cards: []
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
      // console.log(response.json())
      response.json().then(data => ({
        data: data,
        status: response.status
    })
    ).then(res => {
    // console.log( res.data)
    // let str = res.data.d
    // let arr = str.spit('')
    // console.log(arr)
  this.results =  JSON.parse(res.data.d)
    console.log(this.results )
    })
  }
  )

},
sortOnCategory(result1, result2) {
    console.log(parseInt(result1.GroupId) - parseInt(result2.GroupId))
    return (parseInt(result1.GroupId) - parseInt(result2.GroupId))

},
async showTest () {
  this.cards = await this.results.sort(this.sortOnCategory)
  console.log(`это карты ${this.cards}`)
}

},
created: function() {
  this.getInfo()
  this.showTest()
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
