<template>
<div>
  <header>
    <h1 class="main_title">{{!status?titleCatalog:titleOrder}}</h1>
  </header>
  <nav :class="!showMenu?'nav-menu':'nav-menu-active'" v-on:click="showMenu = !showMenu">
    <div>
      <button type="button" name="button" v-on:click="status = !status">{{!showMenu?"К":"КАТАЛОГ"}}</button>
    </div>
    <button type="button" name="button" v-on:click="status = !status">{{!showMenu?"З":"ЗАКАЗЫ"}}</button>
  </nav>
  <div class="container">
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
    <div class="order" :class="status?'order-active': 'order-passive'">
      <table border="1" v-for="order in orders">
        <tr>
          <td v-for="result in results" v-if="order.ProductId==result.Id" rowspan="3" colspan="2"><img v-bind:src="result.Img" alt=""></td>
          <td v-for="result in results" v-if="order.ProductId==result.Id" rowspan="2"> {{result.Caption}}</td>
          <td rowspan="3">дата заказа {{order.Date}}</td>
          <td>количество: {{order.Amount}}</td>
        </tr>
        <tr>
          <td>цена: {{order.Price}}</td>
        </tr>
        <tr>
          <div v-for="category in categories">
            <td v-for="result in results" v-if="order.ProductId==result.Id && category.Id == result.GroupId">категория: {{category.Caption }}
            </td>
          </div>
          <td>сума: {{(order.Amount * order.Price).toFixed(2)}}</td>
        </tr>
        <tr>
        </tr>
        <tr>
          <td v-for="stage in stages" v-if="order.StageId==stage.Id" colspan="5">статус:{{stage.Caption}}</td>
        </tr>
      </table>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      titleCatalog: 'Каталог',
      titleOrder: 'Заказы',
      results: [],
      cards: [],
      categories: [],
      showWire: false,
      showNetwork: false,
      showCable: false,
      orders: [],
      stages: [],
      status: false,
      showWiresContainer: true,
      showNetworkContainer: true,
      showCableContainer: true,
      showMenu: false
    }
  },
  methods: {
    getInfo() {

      fetch('http://m.it.ua/ws/WebService.asmx/ExecuteEx', {
          method: 'POST',
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
          })).then(res => {

            this.results = JSON.parse(res.data.d)
            console.log(this.results)

          })
        })
    },
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

          this.categories = JSON.parse(res.data.d)
          console.log(this.categories)
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
    },
    getOrders() {
      fetch('http://m.it.ua/ws/WebService.asmx/ExecuteEx', {
        method: 'POST',
        body: JSON.stringify({

          calcId: '_MOCK.GET_ORDERS',
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

          this.orders = JSON.parse(res.data.d)
          console.log(this.orders)
        })
      })
    },
    getStages() {
      fetch('http://m.it.ua/ws/WebService.asmx/ExecuteEx', {
        method: 'POST',
        body: JSON.stringify({

          calcId: '_MOCK.GET_STAGES',
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

          this.stages = JSON.parse(res.data.d)
          console.log(this.stages)
        })
      })
    }
  },
  created: function() {
    this.getInfo();
    this.getCategory();
    this.getOrders();
    this.getStages();
  }

}
</script>


<style>
.container {
  margin-left: 10vw;
  position: relative;
}

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

header {
  width: 100%;
  background-color: #6c737f;
  height: 80px;
  position: fixed;
  z-index: 10000;
}

.catalog {
  padding-top: 80px;
}

.small-container {
  display: flex;
  flex-wrap: wrap;
  width: 85vw;
}

.card {
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

.category {
  width: 85vw;
  height: 10vh;
  background-color: #103370;
  color: #fff;
  font-size: 130%;
  cursor: pointer;
  position: relative;
  margin: 50px 0;
}

.nav-menu {
  width: 20vw;
  height: 100vh;
  position: fixed;
  top: 0;
  left: -20px;
  background-color: #6c737f;
  border: 2px dashed #000;
  margin-top: 80px;
  transition: all .3s cubic-bezier(.65, .05, .36, 1);
  transform: translate(-60%);
  z-index: 100000;
  text-align: right;
  cursor: pointer;
}

.nav-menu-active {
  width: 20vw;
  height: 100vh;
  position: fixed;
  top: 0;
  left: -20px;
  background-color: #6c737f;
  border: 2px dashed #000;
  margin-top: 80px;
  transition: all .3s cubic-bezier(.65, .05, .36, 1);
  z-index: 100000;
  transform: translate(0);
  cursor: pointer;
}

.order {
  padding-top: 80px;
  background-color: #fff;
  width: 85vw;
}

table {
  background-color: #fff;
  width: 70vw;
  margin-bottom: 15px;
}

.order-active {
  position: absolute;
  top: 0;
  z-index: 1000;
}

.order-passive {
  display: none;
}

.main_title {
  color: #fff;
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

button {
  font-size: 170%;
  margin: 10px 25px;
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
