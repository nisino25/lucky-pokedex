<template>
  <div class="region">
    <ul>
      <li style="" @click="scrollToSection('pokemon-1')">
        <div class="percentage" :style="{ width: (luckyCounts[0] / 151 * 100) + '%' }"></div>
        カントー<br>{{ luckyCounts[0] }}/151
      </li>
      <li @click="scrollToSection('pokemon-152')">
        <div class="percentage" :style="{ width: (luckyCounts[1] / 100 * 100) + '%' }"></div>
        ジョウト<br>{{ luckyCounts[1] }}/100
      </li>
      <li @click="scrollToSection('pokemon-252')">
        <div class="percentage" :style="{ width: (luckyCounts[2] / 135 * 100) + '%' }"></div>
        ホウエン<br>{{ luckyCounts[2] }}/135
      </li>
      <li @click="scrollToSection('pokemon-387')">
        <div class="percentage" :style="{ width: (luckyCounts[3] / 113 * 100) + '%' }"></div>
        シンオウ<br>{{ luckyCounts[3] }}/113
      </li>
      <li @click="scrollToSection('pokemon-494')">
        <div class="percentage" :style="{ width: (luckyCounts[4] / 146 * 100) + '%' }"></div>
        イッシュ<br>{{ luckyCounts[4] }}/146
      </li>
      <li @click="scrollToSection('pokemon-650')">
        <div class="percentage" :style="{ width: (luckyCounts[5] / 72 * 100) + '%' }"></div>
        カロス<br>{{ luckyCounts[5] }}/72
      </li>
      <li @click="scrollToSection('pokemon-722')">
        <div class="percentage" :style="{ width: (luckyCounts[6] / 91 * 100) + '%' }"></div>
        アローラ<br>{{ luckyCounts[6] }}/91
      </li>
      <li @click="scrollToSection('pokemon-813')">
        <div class="percentage" :style="{ width: (luckyCounts[7] / 86 * 100) + '%' }"></div>
        ガラル<br>{{ luckyCounts[7] }}/86
      </li>
      <li @click="scrollToSection('pokemon-899')">
        <div class="percentage" :style="{ width: (luckyCounts[8] / 7 * 100) + '%' }"></div>
        ヒスイ<br>{{ luckyCounts[8] }}/7
      </li>
      <li @click="scrollToSection('pokemon-906')">
        <div class="percentage" :style="{ width: (luckyCounts[9] / 114 * 100) + '%' }"></div>
        パルデア<br>{{ luckyCounts[9] }}/114
      </li>
    </ul>
  </div>
  <div>
    <div class="wrapper">
      <template v-for="(poke, i) in pokeData" :key="i">
        <div class="child" :id="'pokemon-' + (i + 1)"    :style="{ backgroundColor: poke ? '#DAA520' : '', opacity: currentPoke === i ? 0.2 : 1 }"  @click="selectMe(i)">
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
      currentPoke: 99999,
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
        
      }
    }else{
      this.setup()
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
      if(this.currentPoke == 99999 || this.currentPoke !== i) {
        this.currentPoke = i;
        console.log('not smae')
        return
      }
      
      if(i == this.currentPoke){
        console.log('changing')
        this.pokeData[i] = !this.pokeData[i]
        this.currentPoke = 99999
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
  computed:{
    luckyCounts() {
      let masterarray = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0,]; // Initialize masterarray with two zeros
      for (let i = 0; i < this.pokeData?.length; i++) {
        if (this.pokeData[i] === true) {
          if (i >= 0 && i <= 150) {
            masterarray[0]++;
          } else if (i <= 250) {
            masterarray[1]++;
          } else if (i <= 385) {
            masterarray[2]++;
          } else if (i <= 492) {
            masterarray[3]++;
          } else if (i <= 648) {
            masterarray[4]++;
          } else if (i <= 720) {
            masterarray[5]++;
          } else if (i <= 808) {
            masterarray[6]++;
          } else if (i <= 897) {
            masterarray[7]++;
          } else if (i <= 904) {
            masterarray[8]++;
          }else{
            masterarray[9]++;
          }
          // You can continue the pattern for other ranges
        }
      }
      return masterarray;
    }
  }
}
</script>

<style>
body {
  touch-action: manipulation;
}

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
  z-index: 99;
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
  position: relative;
  width: calc(20% - 10px);
  margin-bottom: 10px;
  list-style-type: none;
  border: 2px solid black;

  padding:10px 0px;
  line-height: 1.75;

  
}

.region ul li .percentage{
  position: absolute;
  display: block;

  top: 0;
  left: 0;
  height: 100%;
  z-index: 0;
  background: #DAA520;
  opacity: 0.3;

}

.wrapper{
  z-index: 98;
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

