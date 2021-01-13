<template>
  <div id="app">
    <Navbar  id="nav"/>
    <Production  id="Production"/>
    <Footer />
    <Cartbuttom class="Cartbuttom" id="Cartbuttom"/>
  </div>
</template>
<script>
import Navbar from './components/Navbar.vue'
import Production from './components/Production.vue'
import Footer from './components/Footer.vue'
import Cartbuttom from './components/Cartbuttom.vue'

export default {
  data: function () {
    return {
      scrollTop: '',
      clientWidth:''
    }
  },
  components: {
    Navbar,
    Production,
    Footer,
    Cartbuttom
  },
  mounted () {
    var _this = this
    window.onscroll = function () {
      _this.scrollTop =
        document.documentElement.scrollTop || document.body.scrollTop
    }
    window.onresize = function () {
      _this.clientWidth =
        window.screenWidth || document.body.clientWidth
    }
  },
  watch: {
    clientWidth:function(){
      console.log(this.clientWidth)
    },
    scrollTop: function () {
      var nav = document.getElementById('nav')
      //  var navFix = document.getElementById('navFix')
      var Production = document.getElementById('Production')
      var navHeight = nav.offsetHeight
      if (this.scrollTop > navHeight) {
        Production.style.marginTop = '68px'
        nav.style.backgroundColor = 'var(--background-color)'
        nav.style.position = 'fixed'
      } else {
        Production.style.marginTop = '0px'
        nav.style.position = 'relative'
        nav.style.backgroundColor = '#ffffff'
      }
      var categoryLIst = document.getElementById('categoryLIst')
      //  var navFix = document.getElementById('navFix')
      var categoryHeight = categoryLIst.offsetHeight
      if (this.scrollTop > categoryHeight) {
        if(this.clientWidth < 481) {
          categoryLIst.style.backgroundColor = 'var(--background-color)'
          categoryLIst.style.position = 'fixed'
          categoryLIst.style.width = '100%'
          categoryLIst.style.top = '69px'
        } else {
          categoryLIst.style.backgroundColor = 'var(--background-color)'
          categoryLIst.style.position = 'fixed'
          categoryLIst.style.width = '18%'
          categoryLIst.style.top = '69px'
        }
      } else {
        console.log(categoryHeight)
        categoryLIst.style.position = 'relative'
        categoryLIst.style.backgroundColor = 'var(--product-bg-color)'
        categoryLIst.style.width = '100%'
        categoryLIst.style.top = '0px'
      }
    }
  }
}
</script>

<style>
@import './css/container.css';
#app {
  font-family:var(--font-Pragati);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--text-color);
  background-color: var(--product-bg-color)
}
.Cartbuttom {
  position: fixed;
  bottom: 5vw;
  right: 2vw;
  display: none;
  z-index: 3;
}
@media screen and (max-width:768px){
  .Cartbuttom {
    display: block
  }
}
</style>
