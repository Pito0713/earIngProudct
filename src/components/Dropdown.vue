<template>
  <div class="PopUp">
    <div v-for="(Product,index) in GetlaterSeeData" :key="Product[1]+index" class="laterSeeData">
        <div class="laterSeeDatatext"  @click="backData (Product[0])">
            <img :src="Product[7]" /><a>{{Product[2]}}</a>
        </div>   
    </div>
  </div>
</template>
<script>

export default {
    data: function(){
        return{
            ProdcutData: [],
            GetlaterSeeData: [],
            GetlaterSeeDatalenght: 0,
        }
    },
    methods:{
        GetlaterSee(){
            this.GetlaterSeeData = this.ProdcutData.filter( //    用id 屬性篩選我要的
                function (item) {
                    return item[5] === true // 篩掉false
            })
            console.log(this.GetlaterSeeData)
            this.GetlaterSeeDatalenght = this.GetlaterSeeData.length;
        }
    },
    mounted () {
        fetch('https://script.google.com/macros/s/AKfycbwzGM7BJ8SnGD626ebzQi3xGdBsJzUlOSdiDIkMmBhplN65FtQ/exec')
            .then(res => {
            return res.json()
        })
        .then(Products => {
            this.ProdcutData = Products
            this.GetlaterSee()
        })
    }
}
</script>

<style>
.PopUp{
    position: fixed;
    z-index: 1;
    width: 100%;
    height: 100%;
    top: 0; left: 0;
    background: rgba(0,0,0,0.5);
}
.laterSeeData{
    margin: 20%;
    width: 60%;
    height: 70%;
    background-color:var(--background-color);
    color: var(--text-color);
    position: absolute;
    font-size: 2.5vw;
}
.laterSeeDatatext{
    height: 10%;
    margin: 4vw 4vw;
    padding: 1vw;
    border-bottom: 1px solid var(--primary-color)
}
.laterSeeDatatext a{
    padding: 0 4vw;
}
.laterSeeDatatext img{
    max-width: 100%;
    max-height: 100%;
}
</style>
