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
      <li @click="scrollToSection('pokemon-495')">
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
      <button @click="exportPokeData()" style="display: block; width: 45%; margin-right: 10%">Export</button>
      <input style="width: 45%;" type="file" @change="importPokeData" />
      <template v-for="(poke, i) in pokeData" :key="i">

        <div v-if="i !== 493" class="child" :id="'pokemon-' + (i + 1)"    :style="{ backgroundColor: poke ? '#DAA520' : '', opacity: currentPoke === i ? 0.2 : 1 }"  @click="selectMe(i)">
          

          <img :src="i < 649 ? `https://img.pokemondb.net/sprites/black-white/anim/normal/${pokeNameList[i]}.gif` : `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${i + 1}.png`" :alt="`Pokemon ${i+1}`">

        </div>
        <div v-if="endOfRegion.includes(i)" class="break-line" style="flex-basis: 100%; height: 0;"></div>
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
      endOfRegion:[150,250,385,492,648,720,808,897,904],
      pokeNameList : [
        "bulbasaur", "ivysaur", "venusaur", "charmander", "charmeleon", "charizard", "squirtle", "wartortle", "blastoise", "caterpie", "metapod", "butterfree", "weedle", "kakuna", "beedrill", "pidgey", "pidgeotto", "pidgeot", "rattata", "raticate", "spearow", "fearow", "ekans", "arbok", "pikachu", "raichu", "sandshrew", "sandslash", "nidoran-f", "nidorina", "nidoqueen", "nidoran-m", "nidorino", "nidoking", "clefairy", "clefable", "vulpix", "ninetales", "jigglypuff", "wigglytuff", "zubat", "golbat", "oddish", "gloom", "vileplume", "paras", "parasect", "venonat", "venomoth", "diglett", "dugtrio", "meowth", "persian", "psyduck", "golduck", "mankey", "primeape", "growlithe", "arcanine", "poliwag", "poliwhirl", "poliwrath", "abra", "kadabra", "alakazam", "machop", "machoke", "machamp", "bellsprout","weepinbell", "victreebel", "tentacool", "tentacruel", "geodude", "graveler", "golem", "ponyta", "rapidash", "slowpoke", "slowbro", "magnemite", "magneton", "farfetchd", "doduo", "dodrio", "seel", "dewgong", "grimer", "muk", "shellder", "cloyster", "gastly", "haunter", "gengar", "onix", "drowzee", "hypno", "krabby", "kingler", "voltorb", "electrode", "exeggcute", "exeggutor", "cubone", "marowak", "hitmonlee", "hitmonchan", "lickitung", "koffing", "weezing", "rhyhorn", "rhydon", "chansey", "tangela", "kangaskhan", "horsea", "seadra", "goldeen", "seaking", "staryu", "starmie", "mr-mime", "scyther", "jynx", "electabuzz", "magmar", "pinsir", "tauros", "magikarp", "gyarados", "lapras", "ditto", "eevee", "vaporeon", "jolteon", "flareon", "porygon", "omanyte", "omastar", "kabuto", "kabutops", "aerodactyl", "snorlax", "articuno", "zapdos", "moltres", "dratini", "dragonair", "dragonite", "mewtwo", "mew", "chikorita", "bayleef", "meganium", "cyndaquil", "quilava", "typhlosion", "totodile", "croconaw", "feraligatr", "sentret", "furret", "hoothoot", "noctowl", "ledyba", "ledian", "spinarak", "ariados", "crobat", "chinchou", "lanturn", "pichu", "cleffa", "igglybuff", "togepi", "togetic", "natu", "xatu", "mareep", "flaaffy", "ampharos", "bellossom", "marill", "azumarill", "sudowoodo", "politoed", "hoppip", "skiploom", "jumpluff", "aipom", "sunkern", "sunflora", "yanma", "wooper", "quagsire", "espeon", "umbreon", "murkrow", "slowking", "misdreavus", "unown", "wobbuffet", "girafarig", "pineco", "forretress", "dunsparce", "gligar", "steelix", "snubbull", "granbull", "qwilfish", "scizor", "shuckle", "heracross", "sneasel", "teddiursa", "ursaring", "slugma", "magcargo", "swinub", "piloswine", "corsola", "remoraid", "octillery", "delibird", "mantine", "skarmory", "houndour", "houndoom", "kingdra", "phanpy", "donphan", "porygon2", "stantler", "smeargle", "tyrogue", "hitmontop", "smoochum", "elekid", "magby", "miltank", "blissey", "raikou", "entei", "suicune", "larvitar", "pupitar", "tyranitar", "lugia", "ho-oh", "celebi", "treecko", "grovyle", "sceptile", "torchic", "combusken", "blaziken", "mudkip", "marshtomp", "swampert", "poochyena", "mightyena", "zigzagoon", "linoone", "wurmple", "silcoon", "beautifly", "cascoon", "dustox", "lotad", "lombre", "ludicolo", "seedot", "nuzleaf", "shiftry", "taillow", "swellow", "wingull", "pelipper", "ralts", "kirlia", "gardevoir", "surskit", "masquerain", "shroomish", "breloom", "slakoth", "vigoroth", "slaking", "nincada", "ninjask", "shedinja", "whismur", "loudred", "exploud", "makuhita", "hariyama", "azurill", "nosepass", "skitty", "delcatty", "sableye", "mawile", "aron", "lairon", "aggron", "meditite", "medicham", "electrike", "manectric", "plusle", "minun", "volbeat", "illumise", "roselia", "gulpin", "swalot", "carvanha", "sharpedo", "wailmer", "wailord", "numel", "camerupt", "torkoal", "spoink", "grumpig", "spinda", "trapinch", "vibrava", "flygon", "cacnea", "cacturne", "swablu", "altaria", "zangoose", "seviper", "lunatone", "solrock", "barboach", "whiscash", "corphish", "crawdaunt", "baltoy", "claydol", "lileep", "cradily", "anorith", "armaldo", "feebas", "milotic", "castform", "kecleon", "shuppet", "banette", "duskull", "dusclops", "tropius", "chimecho", "absol", "wynaut", "snorunt", "glalie", "spheal", "sealeo", "walrein", "clamperl", "huntail", "gorebyss", "relicanth", "luvdisc", "bagon", "shelgon", "salamence", "beldum", "metang", "metagross", "regirock", "regice", "registeel", "latias", "latios", "kyogre", "groudon", "rayquaza", "jirachi", "deoxys-normal", "turtwig", "grotle", "torterra", "chimchar", "monferno", "infernape", "piplup", "prinplup", "empoleon", "starly", "staravia", "staraptor", "bidoof", "bibarel", "kricketot", "kricketune", "shinx", "luxio", "luxray", "budew", "roserade", "cranidos", "rampardos", "shieldon", "bastiodon", "burmy", "wormadam-plant", "mothim", "combee", "vespiquen", "pachirisu", "buizel", "floatzel", "cherubi", "cherrim", "shellos", "gastrodon", "ambipom", "drifloon", "drifblim", "buneary", "lopunny", "mismagius", "honchkrow", "glameow", "purugly", "chingling", "stunky", "skuntank", "bronzor", "bronzong", "bonsly", "mime-jr", "happiny", "chatot", "spiritomb", "gible", "gabite", "garchomp", "munchlax", "riolu", "lucario", "hippopotas", "hippowdon", "skorupi", "drapion", "croagunk", "toxicroak", "carnivine", "finneon", "lumineon", "mantyke", "snover", "abomasnow", "weavile", "magnezone", "lickilicky", "rhyperior", "tangrowth", "electivire", "magmortar", "togekiss", "yanmega", "leafeon", "glaceon", "gliscor", "mamoswine", "porygon-z", "gallade", "probopass", "dusknoir", "froslass", "rotom", "uxie", "mesprit", "azelf", "dialga", "palkia", "heatran", "regigigas", "giratina-altered", "cresselia", "phione", "manaphy", "darkrai", "shaymin-land", "arceus", "victini", "snivy", "servine", "serperior", "tepig", "pignite", "emboar", "oshawott", "dewott", "samurott", "patrat", "watchog", "lillipup", "herdier", "stoutland", "purrloin", "liepard", "pansage", "simisage", "pansear", "simisear", "panpour", "simipour", "munna", "musharna", "pidove", "tranquill", "unfezant", "blitzle", "zebstrika", "roggenrola", "boldore", "gigalith", "woobat", "swoobat", "drilbur", "excadrill", "audino", "timburr", "gurdurr", "conkeldurr", "tympole", "palpitoad", "seismitoad", "throh", "sawk", "sewaddle", "swadloon", "leavanny", "venipede", "whirlipede", "scolipede", "cottonee", "whimsicott", "petilil", "lilligant", "basculin-red-striped", "sandile", "krokorok", "krookodile", "darumaka", "darmanitan","maractus", "dwebble", "crustle", "scraggy", "scrafty", "sigilyph", "yamask", "cofagrigus", "tirtouga", "carracosta", "archen", "archeops", "trubbish", "garbodor", "zorua", "zoroark", "minccino", "cinccino", "gothita", "gothorita", "gothitelle", "solosis", "duosion", "reuniclus", "ducklett", "swanna", "vanillite", "vanillish", "vanilluxe", "deerling", "sawsbuck", "emolga", "karrablast", "escavalier", "foongus", "amoonguss", "frillish", "jellicent", "alomomola", "joltik", "galvantula", "ferroseed", "ferrothorn", "klink", "klang", "klinklang", "tynamo", "eelektrik", "eelektross","elgyem", "beheeyem", "litwick", "lampent", "chandelure", "axew", "fraxure", "haxorus", "cubchoo", "beartic", "cryogonal", "shelmet", "accelgor", "stunfisk", "mienfoo", "mienshao", "druddigon", "golett", "golurk", "pawniard", "bisharp", "bouffalant", "rufflet", "braviary", "vullaby", "mandibuzz", "heatmor", "durant", "deino", "zweilous", "hydreigon", "larvesta", "volcarona", "cobalion", "terrakion", "virizion", "tornadus-incarnate", "thundurus-incarnate", "reshiram", "zekrom", "landorus-incarnate", "kyurem", "keldeo-ordinary", "meloetta-aria", "genesect", "chespin"
    ],

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
        const offset = 230; // Extra 20px
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
    exportPokeData(){
      // Get current date in yyyyMMdd format
      const today = new Date();
      const yyyy = today.getFullYear();
      const mm = String(today.getMonth() + 1).padStart(2, '0'); // Months are zero-indexed
      const dd = String(today.getDate()).padStart(2, '0');
      const formattedDate = `${yyyy}${mm}${dd}`;

      // Convert pokeData to a JSON string
      const jsonData = JSON.stringify(this.pokeData, null, 2); // null and 2 for pretty-print

      // Create a blob and download it
      const blob = new Blob([jsonData], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = `pokeData_${formattedDate}.json`;
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
    },
    importPokeData(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          try {
            this.pokeData = JSON.parse(e.target.result);
            console.log("PokeData imported successfully:", this.pokeData);
          } catch (error) {
            console.error("Error parsing JSON:", error);
          }
        };
        reader.readAsText(file);
      }
    }
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
  /* justify-content: space-between; */
  flex-wrap: wrap;
  width: 95vw;
  margin: auto;
}

.wrappr button{
  
}

.wrapper .child{
  margin: 0px 5px;
  width: calc(20% - 15px);
  margin-bottom: 10px;
  border: 2px solid black;
  display: flex;
  align-items: center;
  min-height: 100px;
  padding: 10px 0px;
}

.wrapper .child img{
  max-width: 80%;
  display: block;
  margin: auto;
}

.break-lime {
  flex-basis: 100%;
  height: 0;
}

.child.break-line::before {
  content: "";
  flex-basis: 100%;
  height: 0;
}


</style>

