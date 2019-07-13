<template>

	<div id="app">

		<component v-bind:is="characterSelect" @emitFunction="viewCharacterScreen" @returnHome="returnHome" @emitSound="toggleSound" :characterViewed="myCharacter" :soundState="soundState">

		</component>

    <div id="disclaimer">
      <p>Mortal Kombat 11(TM) is owned by <a target="_blank" href="http://www.netherrealm.com/">Netherrealm studios.</a></p>
    </div>

	</div>

</template>

<script>

import Home from './components/Home.vue'
import CharacterView from './components/CharacterView.vue'

export default {
  name: 'app',

  // NOT on 'this' scope
  components: {
    CharacterView,
    Home
  },

  mounted: function() {
    
    var lFollowX = 0,
        lFollowY = 0,
        x = 0,
        y = 0,
        friction = 1 / 30;

    function moveBackground() {
      x += (lFollowX - x) * friction;
      y += (lFollowY - y) * friction;
      
      let _body = document.getElementsByTagName("body")[0]

      _body.style.backgroundPosition = ( (x - 10) + "px " + (y - 10) + "px")

    }

    window.addEventListener("mousemove", function(e) { 

      var lMouseX = Math.max(-100, Math.min(100, window.innerWidth / 2 - e.clientX));
      var lMouseY = Math.max(-100, Math.min(100, window.innerHeight / 2 - e.clientY));
      lFollowX = (20 * lMouseX) / 100; // 100 : 12 = lMouxeX : lFollow
      lFollowY = (10 * lMouseY) / 100;

    });

    this.soundState = this.soundOff
    setInterval(moveBackground, 50 )

  },

  // on 'this'
  methods: {
    viewCharacterScreen: function(character) {

      if(character == "empty") {
        return;
      }

      this.currentView = "characterView"
      this.myCharacter = character
    },

    returnHome: function() {
      this.currentView = "characterSelect"
    }, 

    toggleSound: function() {
      if(this.titleAudio.paused) {
        this.titleAudio.play()
        this.soundState = this.soundOn
      }

      else {
        this.titleAudio.pause()
        this.soundState = this.soundOff
      }
    }
  },

  // On 'this' scope
  data: function() {
    return {
      soundOff: require("./assets/sound-off.png"),
      soundOn: require("./assets/sound-on.png"),
      soundState: this.soundOff,
      myHome: Home,
      myCharacterView: CharacterView,
      myCharacter: "baraka",
      currentView: "characterSelect",
      titleAudio: new Audio( require("./assets/title.mp3") )
    }
  },

  computed: {
    characterSelect: function() {

      if(this.currentView != "characterView") {
        return this.myHome
      }
      else {
        return this.$options.components.CharacterView
      }

    },
  }
}

</script>

<style>

  @font-face {
    src: url("./assets/fonts/mk11.ttf");
    font-family: "mk11";
  }

  @font-face {
    src: url("http://mortalkombatwarehouse.com/site/fonts/mkx.ttf");
    font-family: "mk11online";
  }

  * {
    padding: 0;
    margin: 0;
  }

  body {
    color: rgb(253, 228, 170);
    background-image: url("assets/background.png");
    background-attachment: fixed;
    font-size: 30px;
    font-family: "mk11", "mk11online";
  }

  .header {
    height: 100px;
    position: relative;
  }

  .header .mk11-logo {
    display: block;
    margin: 0 auto;
    width: auto;
    width: 384px;
    height: 100%;
  }

  .header .img-snd {
    position: absolute;
    right: 8px;
    top: 8px;
    width: auto;
    height: 25px;
    opacity: 0.5;
  }

  .character-icon {
    width: inherit;
    height: inherit;
  }


  .character-wrapper {
    width: 610px;
    margin: 0 auto;
  }
  .character-grid {
    display: inline-grid;
    grid-template-rows: auto auto auto auto auto auto ;
    grid-template-columns: 100px 100px 100px 100px 100px 100px ;
  }

  .character-box {
    /* WR ratio: 1.0166666667 */
    width: 86px;
    height: 95px;
    margin: 5px;
    border: 3px rgb(151, 119, 45) groove;
    background: black;
    background: rgba(0,0,0,0.9);

    cursor: pointer;
    transition: background 0.5s, border 0.5s, opacity 0.5s;
  }

  .character-box:hover {
    opacity: 0.8;
    background: rgba(56, 56, 56, 0.7);
    border: 3px rgb(235, 184, 65) groove;
  }

  .character-view-box {
    margin: 0 auto;
  }

  .character-name {
    text-align: center;
  }

  .empty {
    opacity: 0;
    cursor: default;
    user-select: none;
  }
  
  .empty:hover {
    opacity: 0;
  }

  .character-grid .centrion {
    z-index: 10;
    position: relative;
    left: 50px;
  }

  #disclaimer {
    font-size: 10px;
    color: #666;
    text-align: center;
    padding-top: 50px;
    opacity: 0.5;
  }

  .back-btn {
    position: absolute;
    left: 5px;
    top: 45px;
    width: 70px;

    border: 1px solid rgb(253, 228, 170);
    border-radius: 5px;

    user-select: none;
    background: #000;

    font-size: 0.9em;
    text-align: center;
    transition: background 0.7s;
    cursor: pointer;
  }

  .back-btn:hover {
    background: #444;
  }

  /* ==== CHARACTER SCREEN ==== */

  .character-header {
    padding: 50px 0 20px;
  }

  .ui-wrapper {
    margin-bottom: 30px;
    overflow: hidden;
  }

  .ps4-button-img {
    width: auto;
    height: 20px;
  }

  .combo, .combo-header {
    display: inline-grid;
    width: 100%;
    height: auto;
    grid-template-rows: 100%;
    grid-template-columns:  30% 7.5% 7.75% 7.75% 7.75% 7.75% 7.75% 7.75% 7.75% auto ;

    font-family: "Arial";
    font-size: 2vw;
    text-shadow: 1px 2px 2px rgba(0,0,0,0.5);
  }

  .combo {
    transition: opacity 0.4s, border 1s;
    border: 1px solid rgba(0,0,0,0);
  }

  .combo:hover {
    opacity: 0.7;
    border: 1px solid rgb(248, 248, 248);
    border-left: 1px solid rgba(0,0,0,0);
  }

  .combo-wrapper .combo:nth-child(odd) {
    opacity: 0.85;
  }

  .combo-header {
    width: 100%;
    height: auto;
    top: 0;
    left: 0;
    position: fixed;
    text-align: center;
    font-size: 0.5em;
    font-weight: bold;
    color: gold;

    z-index: 11;
  }
  
  
  .combo-header .combo-combo {
    background: rgb(41, 41, 41);
  }
  .combo-header .combo-type {
    background: rgb(4, 97, 16);
  }
  .combo-header .combo-damage {
    background: rgb(141, 141, 141);
  }

  .combo-header .combo-cancel {
    background: rgba(143, 133, 2, 1);
  }

  .combo-header .startup {
    background: rgba(100,0,0,1);
  }

  .combo-header .hit {
    background: rgb(91, 106, 128);
  }

  .combo > span {
    text-align: center;
    border-right: 1px solid black;
  }

  .combo-header > span {
    border-right: 1px solid black;

  }


  .combo .combo-combo {
    background: rgba(61, 61, 61, 0.2);
  }

  .combo .combo-type {
    background: rgba(6, 223, 60, 0.2);
    font-size: 0.8em;
  }
  .combo .combo-damage {
    background: rgba(231, 231, 231, 0.2);
  }

  .combo .combo-cancel {
    background: rgba(143, 133, 2, 0.2);
  }

  .combo .startup {
    background: rgba(100,0,0,0.2);
  }

  .combo .hit {
    background: rgba(174, 203, 246, 0.2);
  }

  .pointer {
    cursor: pointer;
  }

  .pointer:hover {
    opacity: 0.6;
  }

  .bottom-btn {
    margin: 10px auto;
    position: static;
  }

  @media screen and (min-width: 1900px) {
    body {
      background-size: cover;
    }
  }

  @media screen and (max-width: 1024px) {
    .combo {
      font-size: 3vw;
    }
    .combo-header {
      font-size: 0.4em;
      font-weight: normal;
      word-wrap: break-word;
    }
    .ps4-button-img {
      width: auto;
      height: 16px;
    }
  }

</style>
