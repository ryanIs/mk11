<template>
  <div id="character-view">
    
    <div class="character-header">

      <p class="back-btn" @click="emitFunction()">Back</p>

      <div class="character-box character-view-box" v-bind:class="characterViewed">
        <img class="character-icon" v-bind:src="require('../assets/characters/' + characterViewed + '.png')">
      </div>

      <p class="character-name">
        {{ characterViewed.toUpperCase() }}
      </p>

    </div>

    <div class="ui-wrapper">

      <div class="combo-wrapper">

          <div class="combo-header">
            <span class="combo-combo">
              <span class="pointer" @click="emitFunction()">←</span> {{ characterViewed.toUpperCase() }} - Combo/Input
            </span>
            
            <span class="combo-type" title="Move type">
              Move Type
            </span>
            <span class="combo-damage" title="Damage">
              Damage
            </span>
            <span class="combo-start startup" title="Start-up">
              Start-up
            </span>
            <span class="combo-active startup" title="Active">
              Active
            </span>
            <span class="combo-recovery startup" title="Recovery">
              Recovery
            </span>
            <span class="combo-cancel"  title="Cancel ADV">
              Cancel ADV
            </span>
            <span class="combo-hit hit "  title="Hit ADV">
              Hit ADV
            </span>
            <span class="combo-block hit"  title="Block ADV">
              Block ADV
            </span>
            <span class="combo-fBlock hit" title="F/Block ADV">
              F/Block ADV
            </span>
          </div>

          <!-- Combo parsing -->
          <div class="combo" v-for="(combos, index) in characters[characterViewed]" :key="index">

            <span class="combo-combo">

              <template v-if="isValidCombo(combos.Command)">

                <span class="ps4-button" v-for="(buttonInput, index) in combos.Command.toString().split('')" :key="index">
                  
                  <img v-if="isValidButton(buttonInput)" class="ps4-button-img" v-bind:src="require('../assets/buttons/' + buttonInput + '.png')">
                  
                  <span v-if="isValidPlus(buttonInput)" class="ps4-button-plus">
                    +
                  </span>
                  
                  <span v-if="isValidPlus(buttonInput) == false && index < combos.Command.toString().length - 1 && buttonInput != ' ' && combos.Command.toString().split('')[index + 1] != '+'" class="ps4-button-comma">
                    ,
                  </span>
                  
                </span> <!-- ps4 buttons loop -->

              </template>

              <template v-else>
                {{combos.Command}}
              </template>

            </span>
            
            <span class="combo-type" title="Move type">
              {{combos.Propriety || combos.Propriety || combos.Propety || combos.Propreity}}
            </span>
            <span class="combo-damage" title="Damage">
              {{combos.Damage}}
            </span>
            <span class="combo-start startup" title="Start-up">
              {{combos.Startup}}
            </span>
            <span class="combo-active startup" title="Active">
              {{combos.Active}}
            </span>
            <span class="combo-recovery startup" title="Recovery">
              {{combos.Recovery}}
            </span>
            <span class="combo-cancel"  title="Cancel ADV">
              {{combos["Cancel Adv"]}}
            </span>
            <span class="combo-hit hit "  title="Hit ADV">
              {{combos["Hit Adv"]}}
            </span>
            <span class="combo-block hit"  title="Block ADV">
              {{combos["Block Adv"]}}
            </span>
            <span class="combo-fBlock hit" title="F/Block ADV">
              {{combos["F-Block Adv"] || combos["Fblock Adv"]}}
            </span>

          </div> <!-- combos loop -->

      </div> <!-- combo wrapper -->

    </div> <!-- ui-wrapper -->
    
      <p class="back-btn bottom-btn" @click="topFunction()">Top</p>
      <p class="back-btn bottom-btn" @click="emitFunction()">Back</p>

  </div> <!-- character view -->
</template>

<script>
  import * as Characters from '../json/'

  export default {

    name: "character-view",

    methods: {

      emitFunction: function() {
        this.$emit("returnHome")
      },

      isValidButton: function(buttonInput) {

        if(buttonInput == "1" || buttonInput == "2" || buttonInput == "3" || 
           buttonInput == "4" || buttonInput == "U" || buttonInput == "D" || 
           buttonInput == "F" || buttonInput == "B" ) {
             return true
        }

        return false;

      },

      isValidPlus: function(buttonInput) {

        if(buttonInput == "+" || buttonInput == "U" || buttonInput == "D" || 
           buttonInput == "F" || buttonInput == "B") {
             return true
        }

        return false;

      },

      isValidCombo: function(command) {
        if(/a|c|e|[g-t]|[v-z]/i.test(command) == false) {
          return true
        }

        else {
          return false
        }
      }, 

      topFunction: function() {
        window.scrollTo(0,0)
      }

    },

    props: {
      characterViewed: String,
    },

    data: function() {
      return {
        characters: Characters,
      }
    }

  }

</script>

<style>

</style>
