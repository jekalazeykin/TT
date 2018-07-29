<template>
    <div class="catalog">
      <div class="category " v-on:click=" makeMarkup(categories[0].Id),  showWire = !showWire, showWiresContainer = !showWiresContainer">
        {{categories[0].Caption}}
        <div :class="!showWire?'arrow-down':'arrow-up'">
        </div>
      </div>
      <div class="wire" v-show="showWire">
      </div>
      <div class="small-container" v-show="showWiresContainer">
        <div v-if="result.GroupId == 'g1'" class="card" v-for="result in results">
          <h4>{{result.Caption}}</h4>
          <img v-bind:src="result.Img" alt="">
        </div>
      </div>
      <div class="category" v-on:click="makeMarkup(categories[1].Id), showNetwork = !showNetwork, showNetworkContainer = !showNetworkContainer">
        {{categories[1].Caption}}
        <div :class="!showNetwork?'arrow-down':'arrow-up'">
        </div>
      </div>
      <div class="network" v-show="showNetwork">
      </div>
      <div class="small-container" v-show="showNetworkContainer">
        <div v-if="result.GroupId == 'g2'" class="card" v-for="result in results">
          <h4>{{result.Caption}}</h4>
          <img v-bind:src="result.Img" alt="">
        </div>
      </div>
      <div class="category" v-on:click="makeMarkup(categories[2].Id), showCable = !showCable, showCableContainer = !showCableContainer">
        {{categories[2].Caption}}
        <div :class="!showCable?'arrow-down':'arrow-up'">
        </div>
      </div>
      <div class="cable" v-show="showCable">
      </div>
      <div class="small-container" v-show="showCableContainer">
        <div v-if="result.GroupId == 'g3'" class="card" v-for="result in results">
          <h4>{{result.Caption}}</h4>
          <img v-bind:src="result.Img" alt="">
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: 'Catalog',
  props: ['results'],
  data() {
    return {
      categories: [],
      showWire: false,
      showNetwork: false,
      showCable: false,
      orders: [],
      stages: [],
      showWiresContainer: true,
      showNetworkContainer: true,
      showCableContainer: true
    }
  },
  methods: {

    getCategory() {
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
      }).then(response => {
        response.json().then(data => ({
          data: data,
          status: response.status
        })).then(res => {

          this.categories = JSON.parse(res.data.d);
          console.log(this.categories);
          this.$emit('categoryGet', this.categories);
        })
      })
    },

    async makeMarkup(num) {

      let strHtml = await this.categoryDescription(num)
      let wire = document.querySelector('.wire');
      let network = document.querySelector('.network');
      let cable = document.querySelector('.cable');
      console.log(num)
      if (num == 'g1') {
        if (!document.querySelector('#left')) {
          wire.insertAdjacentHTML('afterbegin', strHtml);
        }
      } else if (num == 'g2') {
        if (!document.querySelector('.b-user-content')) {
          network.insertAdjacentHTML('afterbegin', strHtml);
        }
      } else {
        if (!document.querySelector('.col1')) {
          cable.insertAdjacentHTML('afterbegin', strHtml);
        }
      }
    },

    categoryDescription(num) {

      console.log(`это категория ${num}`)
      return fetch('http://m.it.ua/ws/WebService.asmx/ExecuteEx', {
        method: 'POST',
        body: JSON.stringify({

          calcId: '_MOCK.GET_GROUP_DESCRIPTION',
          args: `{"groupId":"${num}"}`,
          ticket: null

        }),
        headers: {
          Accept: 'json',
          'Content-Type': 'application/json',
        }
      }).then(function(response) {
        return response.json().then(function(data) {
          return {
            data: data,
            status: response.status
          }
        }).then(function(res) {
          return JSON.parse(res.data.d)
        })
      })
    }
  },
  created: function() {
    this.getCategory();
  }

}
</script>


<style>

  h1,
  h2 {
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

  .catalog {
    padding-top: 80px;
    padding: 80px 2px;
  }

  .small-container {
    display: flex;
    flex-wrap: wrap;
    width: 85vw;
  }

  .card {
    border: 2px solid #103370;
    margin: 5px 5px;
    border-radius: 8px;
    box-shadow: 4px 5px 4px rgb(126, 147, 181);
    background-color: #fff;
  }

  img {
    width: 20vw;
    height: 20vh;
  }

  h4 {
    width: 20vw;
  }

  .category {
    width: 85vw;
    height: 10vh;
    background: linear-gradient(to bottom,  #103370, #95b9ed);
    color: #fff;
    font-size: 130%;
    cursor: pointer;
    position: relative;
    margin: 50px 0;
    box-shadow: 0 5px 4px rgb(126, 147, 181);
    border-radius: 10px;
  }
  .category:hover {
    opacity: 0.7;
  }
  .wire {
    background-color: #fff;
    width: 84.7vw;
    border: 2px solid #103370;
  }

  .network {
    background-color: #fff;
    width: 84.7vw;
    border: 2px solid #103370;
  }

  .cable {
    background-color: #fff;
    width: 84.7vw;
    border: 2px solid #103370;
  }

  .arrow-up {
    position: absolute;
    bottom: 5px;
    left: 50%;
    background-image: url(../assets/up-arrow.svg);
    width: 25px;
    height: 15px;
  }

  .arrow-down {
    position: absolute;
    bottom: 5px;
    left: 50%;
    background-image: url(../assets/angle-arrow-down.svg);
    width: 25px;
    height: 15px;
  }
</style>
