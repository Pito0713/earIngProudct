<template>
  <div>
    <div class="CartButtomText">
        <a href="https://pito0713.github.io/carcartEaring/"><i class="fas fa-shopping-cart"></i></a>
        <a class="CartDataLenght">{{CartBackDatalenght}}</a>
    </div>
  </div>
</template>
<script>

export default {
    data: function(){
        return{
            CartBackData: [],
            CartBackDatalenght: ''
        }
    },
    methods:{
        CartBackData1 () {
            this.CartBackData = this.CartBackData.filter( //    用id 屬性篩選我要的
                function (item) {
                    return item[4] !== '' // 篩掉數量0的空值
            }
        )    
        this.CartBackDatalenght = this.CartBackData.length
        }
    },
    mounted () {
    fetch('https://script.google.com/macros/s/AKfycbxLQARlHh9k7LbV8-ORSmVjIYAJtgphhKXFS0e6ypXmpAWJX8cV/exec')
      .then(res => {
        return res.json()
      })
      .then(CartBackData => {
        this.CartBackData  = CartBackData
        this.CartBackData1 ()
      })  
    }
  
}
</script>

<style>
.CartButtomText {
    width: 8vw;
    height: 8vw;
    border-radius: 9999px;
    font-size: 5vw;
    border: 1px solid var(--border-color);
    background-color: var(--background-color);
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
}
.CartDataLenght{
    color: red;
    position: absolute;
    font-size: 5vw;
    top:-60%;
    right: -10%;
}
</style>
