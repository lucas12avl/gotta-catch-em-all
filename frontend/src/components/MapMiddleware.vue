<script>
import Map from '../base_components/Map.vue';
import ControlsFrame from '@/base_components/ControlsFrame.vue';
import { blocks, isBlockWalkable } from '@/globals.js';

export default {
  emits: ['updatePlayerPosition', 'updateInWildEncounter', 'isInGrass'],
  props: ['playerX', 'playerY', 'moveUp','moveDown', 'moveRight', 'moveLeft','inWildEncounter','inPokedex','inMenu', 'facingDirection', 'inUnown'],

  components: {
    Map,
    ControlsFrame
  },

  data() {

    return {

      isMapMoving: false,
      /**
       * It is actually the map that moves visually, because the player is always in the center, 
       * so for him to always be in the center and NOT move from there, he cannot move if the map does too, 
       * because otherwise , the player would no longer be in the center of the screen where he should always be
       */
      oldDirection: false,
      oldPressed: false,
      oldFacingDirection: this.facingDirection,
    }

  },


  methods: {

    handleMovement(direction, isPressed) {

      this.oldDirection = direction
      this.oldPressed = isPressed
      this.$emit('updatePlayerPosition', { x: this.playerX, y: this.playerY, movement:"standing", facingDir:this.oldFacingDirection });

      if (!this.isMapMoving && !this.inWildEncounter && !this.inMenu && !this.inPokedex && isPressed && !this.inUnown) { //the restrictions to not allow the movement 

        let newX = this.playerX
        let newY = this.playerY
        

        switch (this.oldDirection) {
          case 'up':
            newY--
            this.oldFacingDirection = 'north'
            break
          case 'down':
            newY++
            this.oldFacingDirection = 'south'
            break
          case 'left':
            newX--
            this.oldFacingDirection = 'west'
            break
          case 'right':
            newX++
            this.oldFacingDirection = 'east'
            break
        }
        if (this.isWalkable(newX, newY)) {
          this.$emit('updatePlayerPosition', { x: newX, y: newY, movement: "walking", facingDir: this.oldFacingDirection});
        //remember props are readonly, thats why use emits and the parent chnages the veriables

        }
        
      }
     
      
    },

    isWalkable(x, y) {
      const blockType = blocks[y][x];
      return isBlockWalkable[blockType];
    },

    enterGrass() { //only if Map emits that the player enters to the grass
      this.$emit('isInGrass', true)
      fetch('http://localhost:8081/enter_grass') // call to endpoint /enter_grass
        .then(response => {
          if (response.status == 400) { 
            //this.inWildEncounter = true  // can not modificate props of parent componnet because are read-only, we must do an emit and the parent listen it and modificate the prop hisself
            this.$emit('updateInWildEncounter', true)//track if we are on a wild encounter// leave the wild enconter must be notificate by wildencounterMiddleware
            
          }
        })
        
    },
    leaveGrass() { //only if Map emits that the player leaves to the grass
      this.$emit('isInGrass', false)
      fetch('http://localhost:8081/leave_grass') // call to endpoint /leave_grass
        .catch(error => console.error('Error:', error));
    },
  },
  watch:{
    
    moveUp(newVal){
      this.handleMovement('up', newVal)

    },

    moveDown(newVal){
      this.handleMovement('down', newVal)
      
    },

    moveRight(newVal){
      this.handleMovement('right', newVal)

    },

    moveLeft(newVal){
      this.handleMovement('left', newVal)

    },




  }
  
}
</script>

<template>  <!--kebab-case if we want to send any property-->
  <Map :player-x="playerX" :player-y="playerY" 
      
        @on-started-moving="isMapMoving = true"
        @on-finished-moving="isMapMoving = false, handleMovement(this.oldDirection, this.oldPressed)"
        @on-entered-grass="enterGrass"
        @on-exited-grass="leaveGrass">
  </Map> <!--emits (@) from Map component that we must listen-->

  <!--$event is if its pressed or released-->
  

 
  <!--implemnts a event if menu is pressed (Menumiddleware) or released to allow the user to move -->
  

</template>



<style>

</style>
