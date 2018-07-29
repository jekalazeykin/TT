<template>
<div class="order" :class="status?'order-active': 'order-passive'">
  <table border="2" v-for="order,index in orders" class="table">
    <tr>
      <td v-for="result in results" v-if="order.ProductId==result.Id" rowspan="3" colspan="2"><img v-bind:src="result.Img" alt=""></td>
      <td v-for="result in results" v-if="order.ProductId==result.Id" rowspan="2"> {{result.Caption}}</td>
        <td rowspan="3" >дата заказа: {{dates[index]}}</td>
      <td>количество: {{order.Amount}}</td>
    </tr>
    <tr>
      <td>цена: {{order.Price.toFixed(2)}}</td>
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
</template>

<script>
export default {
  name: 'Order',
  props: ['results', 'categories','status'],
  data() {
    return {
      orders: [],
      stages: [],
      dates: []
    }
  },
  methods: {

    getOrders() {
    return  fetch('http://m.it.ua/ws/WebService.asmx/ExecuteEx', {
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
    },
    async niceDate () {
      this.orders = await this.getOrders();
      console.log(this.orders);
      let arr = this.orders;
      let arr3 =[];
      arr.forEach(function(item, i, arr) {
      let arr1 = item.Date.split("").splice(0, 10);
      let arr2 = [];
      arr2[0] = `${arr1[8]}${arr1[9]}`
      arr2[1] = `${arr1[5]}${arr1[6]}`
      arr2[2] = `${arr1[0]}${arr1[1]}${arr1[2]}${arr1[3]}`
      if(  arr2[1] == '01') {
        arr2.splice(1, 1, 'января')
      }
      else if (arr2[1] == '02') {
        arr2.splice(1, 1, 'февраля')
      }
      else if (arr2[1] == '03') {
        arr2.splice(1, 1, 'марта')
      }
      else if (arr2[1] == '04') {
        arr2.splice(1, 1, 'апреля')
      }
      else if (arr2[1] == '05') {
        arr2.splice(1, 1, 'мая')
      }
      else if (arr2[1] == '06') {
        arr2.splice(1, 1, 'июня')
      }
      else if (arr2[1] == '07') {
        arr2.splice(1, 1, 'июля')
      }
      else if (arr2[1] == '08') {
        arr2.splice(1, 1, 'августа')
      }
      else if (arr2[1] == '09') {
        arr2.splice(1, 1, 'сентября')
      }
      else if (arr2[1] == '10') {
        arr2.splice(1, 1, 'октября')
      }
      else if (arr2[1] == '11') {
        arr2.splice(1, 1, 'ноября')
      }
      else if (arr2[1] == '12') {
        arr2.splice(1, 1, 'декабря')
      };
      let dateStr = arr2.join(' ')
      arr3.push(dateStr)

      });
      this.dates = arr3
    }
  },
  created: function() {
    this.getStages();
    this.niceDate()
  }

}
</script>

<style>
.order-active {
  position: absolute;
  top: 0;
  z-index: 1000;
}
.order {
  padding-top: 100px;
  background-color: #d9ddda;
  width: 86vw;
}
.order-passive {
  display: none;
}
.table {
  background-color: #fff;
  width: 70vw;
  margin: 0 auto 15px;
  box-shadow: 8px 8px 4px rgb(247, 247, 170);
}
</style>
