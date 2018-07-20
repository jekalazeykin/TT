<template>
  <div class="container">
    <div class="category" v-on:click=" makeMarkup(categories[0].Id)">
       {{categories[0].Caption}}
    </div>
    <!-- <div class="category-card">

    </div> -->
    <div class="small-container">
    <div v-if="result.GroupId == 'g1'" class="card" v-for="result in results">
      <h4>{{result.Caption}}</h4>
      <img v-bind:src="result.Img" alt="">
    </div>
      </div>
      <div class="category"  v-on:click="categoryDescription(categories[1].Id)">
         {{categories[1].Caption}}
      </div>
    <div class="small-container">
    <div v-if="result.GroupId == 'g2'" class="card" v-for="result in results" >
      <h4>{{result.Caption}}</h4>
      <img v-bind:src="result.Img" alt="">
    </div>
    </div>
    <div class="category"  v-on:click="categoryDescription(categories[2].Id)">
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
      categories: [],
      description: ''
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
},

async makeMarkup(num) {
  let a = await this.categoryDescription(num)
  console.log(a)
  let markup = document.querySelector('.category');
  markup.insertAdjacentHTML('afterend', a);
},

categoryDescription (num) {
  // for (let i = 0; i < this.categories.length; i++) {

  console.log(`это категория ${num}`)
   return fetch('http://m.it.ua/ws/WebService.asmx/ExecuteEx', {
    method: 'POST',
    body: JSON.stringify({

      calcId: '_MOCK.GET_GROUP_DESCRIPTION',
      // args: '{"groupId":"g1"}',
      args: `{"groupId":"${num}"}`,
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

  this.description =  JSON.parse(res.data.d)
    console.log(this.description )
    })
  }
)
// }
  // setTimeout(this.makeMarkup, 3000)

},
  test () {

    fetch('http://m.it.ua/ws/WebService.asmx/ExecuteEx', {
    method: 'POST',
    body: JSON.stringify({

      calcId: '_MOCK.GET_GROUP_DESCRIPTION',
       args: '{"groupId":"g1"}',
      ticket: null

    }),
    headers: {
    Accept: 'json',
    'Content-Type': 'application/json',
  }
}).then(response => {
       if (response.ok)
  console.log(response.json())

  }
)
  }
},
created: function() {
  this.getInfo();
  this.getCategory();
 // this.categoryDescription();
 this.test();

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
