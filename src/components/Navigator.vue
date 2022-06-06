<template>
  <section>
    <div id="navigator">
      <button @click="restart()">{{message ==  "You Picked Right!"? "Play again!" : "New colors!"}}</button>
      <span id="message"> {{message}} </span>
      <button :class="{ selected: isEasy }" @click="changeToEasy()">
        easy
      </button>
      <button :class="{ selected: !isEasy }" @click="changeToHard()">
        hard
      </button>
    </div>
    <Container :correctColor="correctColor" :colors="colors" @message="getMessage($event)" @currentIndex="currentIndex=$event"/>
  </section>
</template>

<script lang="js">
  import Container from "./Container.vue";
  export default  {
    name: 'src-components-navigator',
    components: {
      Container
    },
    props: [],
    beforeMount(){
      this.restart()
    },
    mounted () {

    },
    data () {
      return {
        isEasy: false,
        colors: [],
        correctColor: null,
        colorCount: 6,
        message : "",
        currentIndex : null
      }
    },
    methods: {
      restart(){
        this.message = ""
        this.colors = this.createColors(this.colorCount)
        this.correctColor = this.colors[this.pickCorrectColor()]
        this.$emit('correctColor',this.correctColor)
        this.$emit('message',this.message)
      },
      changeToEasy(){
        if(!this.isEasy){
          this.colorCount = 3      
          this.isEasy = true
          this.restart()
        }
      },
      changeToHard(){
        if(this.isEasy){
          this.colorCount = 6
          this.isEasy = false
          this.restart()
        }
      },
      createRandomStringColor(){
        var newColor = "rgb(" + this.randomInt() + ", " + this.randomInt() + ", " + this.randomInt() + ")"
        return newColor
      },
      randomInt(){
        return Math.floor(Math.random()*256)
      },
      pickCorrectColor(){
        return Math.floor(Math.random() * this.colorCount);
      },
      createColors(){
        var arr = []
        for (var i = 0; i < this.colorCount; i++) {
          arr.push(this.createRandomStringColor());
        }
        return arr
      },
      setAllCorrectColor(){
        /* Genero un array nuevo ya que si actualizo colors por posicion (this.colors[i] = this.correctColor), la prop en Container no se actualiza */
        var allToCorrect = []
        for (var i = 0; i < this.colorCount; i++) {
          allToCorrect.push(this.correctColor);
        }
        this.colors = allToCorrect
      },
      changeByError(){
        /* Genero un array nuevo ya que si actualizo colors por posicion (this.colors[this.currentIndex] = "#232323"), la prop en Container no se actualiza */
        var colorsChangedByError = []
        for (var i = 0; i < this.colorCount; i++) {
          if(i == this.currentIndex){
            colorsChangedByError.push("#232323");
          } else{
            colorsChangedByError.push(this.colors[i]);
          }
        }
        this.colors = colorsChangedByError
      },
      getMessage(message){
        this.message = message
        if(message == "You Picked Right!"){
          this.setAllCorrectColor()
        }else{
          this.changeByError()
        }
        this.$emit('message',message)
      }
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
.src-components-navigator {
}
#navigator {
  background: #ffffff;
  height: 30px;
  text-align: center;
  margin: 0;
  margin-top: -30px;
}
#message {
  color: #000000;
  display: inline-block;
  width: 20%;
  text-transform: none;
}
.selected {
  background-color: steelblue;
  color: white;
}
button {
  border: none;
  background-color: white;
  font-family: "Montserrat", "Avenir";
  text-transform: uppercase;
  height: 100%;
  font-weight: 700;
  letter-spacing: 1px;
  color: steelblue;
  transition: all 0.3s;
  outline: none;
}
button:hover {
  color: white;
  background-color: steelblue;
}
h1 {
  color: white;
}
</style>
