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

</style>