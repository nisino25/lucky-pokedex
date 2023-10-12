<template>
  <div class="region">
    <ul>
      <li><a  @click="scrollToSection('pokemon-1')">カントー</a></li>
      <li><a  @click="scrollToSection('pokemon-152')">ジョウト</a></li>
      <li><a  @click="scrollToSection('pokemon-252')">ホウエン</a></li>
      <li><a  @click="scrollToSection('pokemon-387')">シンオウ</a></li>
      <li><a  @click="scrollToSection('pokemon-494')">イッシュ</a></li> <br>
      <li><a  @click="scrollToSection('pokemon-650')">カロス</a></li>
      <li><a  @click="scrollToSection('pokemon-722')">アローラ</a></li>
      <li><a  @click="scrollToSection('pokemon-813')">ガラル</a></li>
      <li><a  @click="scrollToSection('pokemon-899')">ヒスイ</a></li>
      <li><a  @click="scrollToSection('pokemon-906')">パルデア</a></li>
    </ul>
  </div>
  <div>
    <div class="wrapper">
      <template v-for="(poke, i) in pokeData" :key="i">
        <div class="child" :id="'pokemon-' + (i + 1)" :style="{ backgroundColor: poke ? '#DAA520' : '' }" @click="selectMe(i)">
          <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${i+1}.png`" :alt="`Pokemon ${i+1}`" :style="{ opacity: poke ? 0.5 : 1 }">
        </div>
      </template>

    </div>
  </div>
  
</template>


<script>


export default {
  name: 'App',
  components: {
  },
  data(){
    return{
      pokeData: [],
      currentPoke: null,
    }
  },
  mounted(){
    const savedData = localStorage.getItem('pokeData');
    if (savedData) {
      try {
        // Parse the JSON string from local storage
        this.pokeData = JSON.parse(savedData);
      } catch (error) {
        // Handle parsing error, if any
        this.setup()
      }
    }
    
    
  },
  methods:{
    setup(){
      

      for (let i = 0; i < 1020; i++) {
          this.pokeData.push(false);
      }
    },
    scrollToSection(sectionId) {
      console.log(sectionId)
      const targetElement = document.getElementById(sectionId);
      if (targetElement) {
        const offset = 120; // Extra 20px
        const targetOffsetTop = targetElement.offsetTop - offset;
        window.scrollTo({
          top: targetOffsetTop,
          behavior: "smooth",
        });
      }
    },
    selectMe(i){
      console.log(i)
      if(!this.currentPoke || this.currentPoke !== i) {
        this.currentPoke = i;
        console.log('not smae')
        return
      }
      
      if(i == this.currentPoke){
        console.log('changing')
        this.pokeData[i] = !this.pokeData[i]
        this.currentPoke = null
      }
    },
  },
  watch: {
    pokeData: {
      handler(newPokeData) {
        // Save the updated pokeData to local storage
        localStorage.setItem('pokeData', JSON.stringify(newPokeData));
      },
      deep: true,
    },
  },
}
</script>

<style>
html{
  background:  #333333 ;
  color: #FFFFFF;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
  
}

.region {
  position: sticky;
  top: 0px;
  padding-top: 30px;
  background-color:  #333333 ;
}

.region ul{
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  width: 95vw;
  margin: auto;
  padding: 0;

} 

.region ul li{
  width: calc(20% - 10px);
  margin-bottom: 10px;
  list-style-type: none;
  border: 2px solid black;
  
}

.wrapper{
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  width: 95vw;
  margin: auto;
}

.wrapper .child{
  width: calc(20% - 10px);
  margin-bottom: 10px;
  border: 2px solid black;
}

.wrapper .child img{
  width: 80%;
}
</style>

