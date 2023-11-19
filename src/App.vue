<template>
  <div>
    <h1>Valorant</h1>
    <div v-if="randomSkin">
      <h2>{{ randomSkin.displayName  }}</h2>
      <img :src="randomSkin.displayIcon" />
    </div>
    <div v-else>
      <p>Brak danych do wyświetlenia.</p>
    </div>
    <input v-model="enteredName" placeholder="Wpisz aktualną nazwę obiektu" />
    <button @click="checkSkin">Check!</button>
  </div>
  <HighScore />
</template>

<script>
import axios from 'axios'
import HighScore from './components/HighScore.vue';

export default {
  components: {
    HighScore
  },

  name: "App",
  data() {
    return {
      skins: [], 
      randomSkin: null,  
      enteredName: '',
    };
  },
  async mounted() {
    axios
      .get("https://valorant-api.com/v1/weapons/skins")
      .then(response => {
        this.skins = response.data.data.filter(skin => (skin.displayName.includes("Vandal") || skin.displayName.includes("Phantom")));
        this.selectRandomSkin(); 
        this.checkSkin();
      })
      .catch(error => {
        console.log(error);
      });
  },
  methods: {
    selectRandomSkin() {
      const randomIndex = Math.floor(Math.random() * this.skins.length);
      this.randomSkin = this.skins[randomIndex];
    },
    checkSkin() {
  const enteredNameLower = this.enteredName.toLowerCase();
  const skinNameLower = this.randomSkin.displayName.toLowerCase();
  let result = 0;

  if (skinNameLower.includes("vandal")) {
    if (enteredNameLower === skinNameLower.replace("vandal", "").trim()) {
      result = result + 1;
      this.selectRandomSkin();
    } else {
      console.log("pa");
    }
  } else if (skinNameLower.includes("phantom")) {
    if (enteredNameLower === skinNameLower.replace("phantom", "").trim()) {
      result = result + 1;
      this.selectRandomSkin();
    } else {
      console.log("pa po");
    }
  } else {
    console.log("dupa");
  }
  console.log(result)
  this.enteredName = '';
},

  }
  
};



</script>


<style scoped>
</style>
