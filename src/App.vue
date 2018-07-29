<template>
<div id="app">
  <header :class="!scrolled?'header-show':'header-hidden'">
    <h1 class="main_title">{{!status?titleCatalog:titleOrder}}</h1>
  </header>
  <img src="./assets/menu.svg" alt="" class="menu_logo" v-on:click="showMenu = !showMenu" v-show="!showMenu">
  <img src="./assets/cancel.svg" alt="" class="menu_logo" v-on:click="showMenu = !showMenu" v-show="showMenu">
  <nav :class="!showMenu?'nav-menu':'nav-menu-active'">
    <div>
      <button type="button" name="button" v-on:click="status = !status">{{!showMenu?"К":"КАТАЛОГ"}}</button>
    </div>
    <button type="button" name="button" v-on:click="status = !status">{{!showMenu?"З":"ЗАКАЗЫ"}}</button>
  </nav>
  <div class="container">
    <router-view :results="results" @categoryGet="categories = $event" />
    <Order :results="results" :categories="categories" :status="status"></Order>
    <img src="./assets/arrow-up.svg" alt="" @click="up" class="up">
  </div>
</div>
</template>

<script>
import Order from '@/components/Order'
export default {
  name: 'App',
  data() {
    return {
      status: false,
      titleCatalog: 'Каталог',
      titleOrder: 'Заказы',
      showMenu: false,
      results: [],
      categories: [],
      scrolled: false,
      scroll: null,
      timer: null
    }
  },
  components: {
    Order
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
    handleScroll(event) {
      this.scrolled = window.scrollY > 0;
    },
    up() {
      this.scroll = window.pageYOffset;
      this.scrollToTop();
    },
    scrollToTop() {
      if (this.scroll > 0) {
        window.scrollTo(0, this.scroll);
        if (this.scroll < 3000) {
          this.scroll = this.scroll - 100;
        } else {
          this.scroll = this.scroll - 2000;
        }
        this.timer = setTimeout(this.scrollToTop, 20);
      } else {
        clearTimeout(this.timer);
        window.scrollTo(0, 0);
      }
    }
  },
  created: function() {
    this.getInfo();
    window.addEventListener('scroll', this.handleScroll);
  },
  destroyed() {
    window.removeEventListener('scroll', this.handleScroll);
  }
}
</script>

<style>
  html {
    background-color: #d9ddda;
  }

  body {
    margin: 0;
  }

  p,
  h3 {
    margin: 0;
  }

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  .header-show {
    width: 100%;
    background: linear-gradient(to top, #6c737f, #000);
    height: 80px;
    position: fixed;
    z-index: 10000;
    opacity: 1;
    transform: translateY(0);
    transition: transform .3s cubic-bezier(.4, 0, .2, 1) .3s, opacity 0s .3s;
    box-shadow: 0 2px 4px rgba(0, 0, 0, .5);
  }

  .header-hidden {
    width: 100%;
    background-color: #6c737f;
    height: 80px;
    position: fixed;
    z-index: 10000;
    opacity: 0;
    transform: translateY(-100%);
    transition: transform .3s cubic-bezier(.4, 0, .6, 1), opacity 0s .3s;
    box-shadow: 0 2px 4px rgba(0, 0, 0, .5);
  }

  button {
    font-size: 200%;
    margin: 10px 25px;
  }

  .main_title {
    color: #fff;
  }

  .nav-menu {
    padding-top: 50px;
    width: 20vw;
    height: 100vh;
    position: fixed;
    top: 0;
    left: -20px;
    background: linear-gradient(to bottom, #6c737f, #000);
    box-shadow: 4px 2px 4px rgba(0, 0, 0, .5);
    margin-top: 80px;
    transition: all .3s cubic-bezier(.65, .05, .36, 1);
    transform: translate(-60%);
    z-index: 100000;
    text-align: right;
  }

  .nav-menu-active {
    padding-top: 50px;
    width: 20vw;
    height: 100vh;
    position: fixed;
    top: 0;
    left: -20px;
    background: linear-gradient(to bottom, #6c737f, #000);
    box-shadow: 4px 2px 4px rgba(0, 0, 0, .5);
    margin-top: 80px;
    transition: all .3s cubic-bezier(.65, .05, .36, 1);
    z-index: 100000;
    transform: translate(0);
  }

  .container {
    padding-left: 10vw;
    position: relative;
    background-color: #d9ddda;
  }

  .menu_logo {
    width: 50px;
    height: 50px;
    position: fixed;
    top: 20px;
    left: 20px;
    cursor: pointer;
    z-index: 200000;
  }
  .menu_logo:hover {
    opacity: 0.8;
  }
  .up {
    position: fixed;
    width: 50px;
    height: 50px;
    bottom: 30px;
    right: 8px;
    z-index: 2000;
    cursor: pointer;
  }
  .up:hover {
    opacity: 0.7;
  }
</style>
