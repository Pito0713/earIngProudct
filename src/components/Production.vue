<template>
  <div >
    <productItem class="slider"/>
  </div>
</template>

<script>
import productItem from '../components/categoryslider.vue'

export default {
  components: {
    productItem
  }
}
</script>

<style scoped lang="scss">
.slider {
  display: flex;
  position: relative;
  padding-top: 2vw;
  margin: 5vw 3vw;
}
@media screen and (max-width:425px){
  .slider {
    display: block;
}
}
</style>
