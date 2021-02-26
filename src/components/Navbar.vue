<template>
  <nav class="nav">
    <a class="logo"  href='https://pito0713.github.io/Demo/'>
      <img src="../img/logo.png" alt="logo" />
    </a>
    <div class="navBranch" :class="{ navOps: isOpen }">
      <ul>
        <li class="nav-Item">
          <a href="https://pito0713.github.io/earIngProudct">耳環 Earrings</a>
        </li>
        <li class="nav-Item"><a href="https://pito0713.github.io/carcartEaring/">
        購物車 Cart</a>
          <a v-if="CartBackDatalenght" class="CartDataLenght"
          href="https://pito0713.github.io/carcartEaring/"></a>
        </li>
      </ul>
    </div>
    <!--Navburger-->
    <div class="burger" @click="showUp">
      <div class="burgerLine"></div>
      <div class="burgerLine"></div>
      <div class="burgerLine"></div>
    </div>
  </nav>
</template>

<script>
export default {
  data: function () {
    return {
      isOpen: false,
      scrollTop: '',
      CartBackData: [],
      CartBackDatalenght: false //提醒紅點
    }
  },
  methods: {
    showUp () {
      this.isOpen = !this.isOpen
    },
    CartBackDataFun () {
      this.CartBackData = this.CartBackData.filter( //    用id 屬性篩選我要的
        function (item) {
           return item[4] !== '' // 篩掉數量0的空值
        }
      )    
      if(this.CartBackData.length > 0){
        this.CartBackDatalenght = true
      } else {
        this.CartBackDatalenght =false
      }
      //console.log(this.CartBackDatalenght)
      }
  },
  mounted () {
    fetch('https://script.google.com/macros/s/AKfycbxLQARlHh9k7LbV8-ORSmVjIYAJtgphhKXFS0e6ypXmpAWJX8cV/exec')
      .then(res => {
        return res.json()
      })
      .then(CartBackData => {
        this.CartBackData  = CartBackData
        this.CartBackDataFun ()
      })  
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.nav {
  display: flex;
  justify-content: space-between;
  background-color: #ffffff;
  align-items: center;
  width: calc(100% - 4vw);
  top:0;
  z-index: 10;
  padding: 0 2vw;
}
.logo{
  display: flex;
  align-items: center;
  margin: 0.7rem;
  img {
    width: 60px;
  }
}
.navBranch ul {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.navBranch .nav-Item {
  padding: 0 1rem;
  display: flex;
}
.navOps {
  /*show navbar*/
  transform: translateX(250px);
}
/*-burger-*/
.burger {
  display: none;
}
.burger div {
  width: 20px;
  height: 2px;
  position: relative;
  display: block;
  background-color: #000000;
  margin: 4px 0;
}
.CartDataLenght{
  position: relative;
  display: flex;
    width: 1vw;
    height: 1vw;
    background-color:red;
    border-radius: 9999px;
    text-align: center;
}
@media screen and (max-width: 769px) {
  .burger {
    display: block;
  }
  nav {
    background-color: var(--background-color)
  }
  .navBranch {
    display: block;
    position: fixed;
    top: 82px;
    /*hidden navbar*/
    left: -250px;
    height: 100%;
    transition: all 1s ease;
    border-right: 1px var(--border-color) solid;
    z-index: 99;
    background-color: var(--background-color);
    width: 30%;
    padding-top:5vw;
  }
  .navBranch ul {
    flex-direction: column;
    .nav-Item {
      padding: 1rem;
      margin: 1rem;
    }
    .nav-Item:hover{
        border-bottom:2px var(--border-color) solid;
    }
  }
  @media screen and (max-width: 425px) {
    .navBranch {
    left: -400px;
    height: 100%;
    transition: all 1s ease;
    border-right: 1px var(--border-color) solid;
    z-index: 99;
    background-color: var(--background-color);
    width: 40%;
   }
   
   .navOps {
   /*show navbar*/
   transform: translateX(400px);
   }
  }
}
</style>
