<template>
  <section class="src-components-container">
    <div id="container">
      <div v-for="(color,index) in colors" :key="index">
        <Square :color="color" :index="index" @currentIndex="getCurrentIndex($event)"/>
      </div>
    </div>
  </section>
</template>

<script lang="js">
import Square from "./Square/IndexSquare.vue";

export default  {
    name: 'src-components-container',
    props: ['colors','correctColor'],
    components : {
      Square
    },
    mounted () {

    },
    data () {
      return {
      }
    },
    methods: {
      getCurrentIndex(currentIndex){
        this.$emit('currentIndex',currentIndex)
        this.isTheCorrectColor(currentIndex)
      },
      isTheCorrectColor(currentIndex){
        var message = ""
        if(this.colors[currentIndex] == this.correctColor){
          message = "You Picked Right!"
        }else{
          message = "Try again!"
        }
        this.$emit('message',message)
      },
    },
    computed: {
    }
}


</script>

<style scoped lang="css">
.src-components-container {
}
#container {
  margin: 20px auto;
  max-width: 600px;
}
</style>
