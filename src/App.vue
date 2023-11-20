<template>
  <div class="widget">
  <div class="container">
    <h1>Guess the Valorant skin!</h1>
    <div v-if="randomSkin">
      <img :src="randomSkin.displayIcon" />
    </div>
    <div v-else>
      <p>Brak danych do wyświetlenia.</p>
    </div>
    <div v-if="showName && nameDisplayed">  {{ randomSkin.displayName  }}</div>

    <input v-model="enteredName" placeholder="Write name of the skin!" />
    <button @click="checkSkin">Check!</button>
    <button @click="reset"> I don't know </button>
    <button @click="toggleShowName">Show Hint</button>
  </div>
  <div class="sidebar">
  <HighScore :resultOf="result" />
  </div>
</div>

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
      result: null,
      showName: false,
      nameDisplayed: false,
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
    reset(){
      const randomIndex = Math.floor(Math.random() * this.skins.length);
      this.randomSkin = this.skins[randomIndex];
      this.result = 0;
      this.nameDisplayed = false;
    },
    toggleShowName(){
      this.showName = true;
      this.nameDisplayed = true;
    },
    checkSkin() {
    const enteredNameLower = this.enteredName.toLowerCase();
    const skinNameLower = this.randomSkin.displayName.toLowerCase();

      if (skinNameLower.includes("vandal")) {
        if (enteredNameLower === skinNameLower.replace("vandal", "").trim()) {
          if(this.showName === !this.showName){
          this.result += 0.5;
          this.nameDisplayed = false;
          }else{
            this.result += 1;
            this.nameDisplayed = false;
          }
        this.selectRandomSkin();
      } else {
        this.showName = true;
      }
  } 
      else if (skinNameLower.includes("phantom")) {
        if (enteredNameLower === skinNameLower.replace("phantom", "").trim()) {
          if(this.showName === !this.showName){
          this.result += 0.5;
          }else{
            this.result += 1;
          }
        this.selectRandomSkin();
      } else {
        this.showName = true;
    }
  }   else {
      console.log("dupa");
  }

    this.enteredName = '';
},

  }
  
};

</script>

<style scoped>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body, html{
  height: 100%;
  margin: 0;
}

.widget{
  height: 100vh;
  background-image: url(./img/valorant_background.png);
  background-repeat: no-repeat;
  background-size: cover;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 50px;
}

.container{
  background-color: white;
  padding: 20px;
  height: 300px;
  width: 700px;
  text-align: center;
}

.container h1{
  text-align: center;
}

.sidebar{
  background-color: beige;
  height: 300px;
  width: 300px;
}
</style>
