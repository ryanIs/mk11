<template>

  <div class="home-view">

    <div class="header">
      <img class="mk11-logo" src="../assets/mk11-logo.png">
      <img class="img-snd" :src="soundState" @click="toggleSound()">
    </div>

    <div class="character-wrapper">

      <div class="character-grid">

        <div v-for="character in characters" :key="character" class="character-box" v-bind:class="character" @click="emitFunction(character)">
          <img class="character-icon" v-bind:src="require('../assets/characters/' + character + '.png')">
        </div>

      </div>
      
    </div>

  </div>
  
</template>

<script>

  export default {

    name: "home-view",

    props: {
      soundState: String
    },
    
    methods: {
      emitFunction: function(str) {
        this.$emit("emitFunction", str)
      }, 

      toggleSound: function() {
        this.$emit("emitSound")
      }
    },

    data: function() { 
      return {
        characterImagePath: "../images/characters/",
        totalCharacters: 26,
        characters: [
          "empty",
          "empty",
          "shaoKahn",
          "frost",
          "empty",
          "empty",

          "empty",
          "johnny",
          "sonya",
          "cassie",
          "jax",
          "empty",

          "scorpion",
          "noob",
          "baraka",
          "raiden",
          "jacqui",
          "subzero",

          "kano",
          "kabal",
          "liuKang",
          "kitana",
          "kungLao",
          "jade",

          "empty",
          "skarlet",
          "erronBlack",
          "dvorah",
          "kotalKahn",
          "empty",

          "empty",
          "empty",
          "geras",
          "kollector",
          "empty",
          "empty",

          "empty",
          "empty",
          "centrion",
          "empty",
          "empty",
          "empty",
        ] 
      }
    }

  }

</script>