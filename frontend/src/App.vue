<script>
import MapMiddleware from './components/MapMiddleware.vue'
import MenuMiddleware from './components/MenuMiddleware.vue'
import PlayerMiddleware from './components/PlayerMiddleware.vue'
import PokedexMiddleware from './components/PokedexMiddleware.vue'
import WildEncounterMiddleware from './components/WildEncounterMiddleware.vue'
import ControlsFrame from './base_components/ControlsFrame.vue'
import Player from './base_components/Player.vue'

export default {
  name: 'App',

  components:{
    MapMiddleware,
    MenuMiddleware,
    PlayerMiddleware,
    PokedexMiddleware,
    WildEncounterMiddleware,
    ControlsFrame,
    Player,

},
  
  data(){
    return{ //variables of the player
      playerX: 15,
      playerY: 12,
      inMenu: false,
      inPokedex: false,
      pokemonRegister: {},

      facingDirection: 'north',
      movementType: 'standing',
      inWildEncounter: false,

      moveUp: false,
      moveDown: false,
      moveRight: false,
      moveLeft: false,

      buttonA: false,
      buttonB: false,
      buttonMenu: false,

      inUnown:false,
      isInGrass: false,

      loading: false, //we MUST NOT RENDER anything on template before the data has obtained with the fetchInitialInfo

      

      //OBS: not the same inMenu(when menu is open) and buttonMenu(pressed/released state of the menu button)
      
    }
  },

  created() {
    // calls  /initial_info endpoint when app.vue is mounted (first thing in being executed after compile?)
    this.fetchInitialInfo();
  },

  methods: {
    fetchInitialInfo() {
      fetch('http://localhost:8081/initial_info')
        .then(response => response.json())
        .then(data => {
          // update inital variables 
          this.playerX = data.x
          this.playerY = data.y
          this.facingDirection = data.direction
          this.pokemonRegister = data.pokedex

          this.loading = true
          
        })
        .catch(error => console.error('Error fetching initial info:', error));
    },
    
    UpdatePlayerPosition({ x, y, movement, facingDir}) { //when we are updating the player position, we change the sprite movement and the direction

      
      this.movementType = movement
      this.playerX = x;
      this.playerY = y;
      this.facingDirection = facingDir
      
    },

    menuIsOpen(menuButton){
      if(!this.inWildEncounter && menuButton){ //menu only appears if we are not on wild encounter
        this.inMenu = true
      }
     

    },

    fetchUnown(){

      fetch('http://localhost:8081/my_status_code_is_unknown')
      .then(response => {
          if (response.status == 201) { 
            this.inUnown = true
          }
        })
      
    }
    
    
  },

  watch:{

    buttonA(){ //the last action that activates the unown event 

      //the requirements to activate the inUnown event + this.buttonA on true bc the watcher detects the change press/unpress when capture and this loops the event 
      if (
        (Object.values(this.pokemonRegister).filter(isOwned => isOwned).length >= 2 ) && 
      ((
        this.playerY == 30 && this.playerX == 9 && this.facingDirection == 'north' && !this.inUnown && this.buttonA) ||
       (this.playerY == 28 && this.playerX == 9 && this.facingDirection == 'south' && !this.inUnown && this.buttonA) ||
       (this.playerY == 29 && this.playerX == 8 && this.facingDirection == 'east' && !this.inUnown && this.buttonA

       ))
        
          ){
        this.fetchUnown()

      }

    }
  }

 
 
};
</script>

<template> <!--@props must be on kebab-case   @example-of-use="exampleOfUse"-->

  <PlayerMiddleware v-if="loading" :facing-direction="facingDirection" :movement-type="movementType" :is-in-grass="isInGrass"/>
  
  <MapMiddleware v-if="loading"

    
    :player-x="playerX" 
    :player-y="playerY"
    :move-up="moveUp" 
    :move-down="moveDown" 
    :move-left="moveLeft" 
    :move-right="moveRight"
    :in-wild-encounter="inWildEncounter"
    :in-pokedex="inPokedex"
    :in-menu="inMenu"
    :facing-direction="facingDirection"
    :in-unown="inUnown"

  
  @update-player-position="UpdatePlayerPosition"
  @update-in-wild-encounter="this.inWildEncounter = $event"
  @is-in-grass="isInGrass = $event"

  
  

  /> 
  <ControlsFrame v-if="loading"
    @on-movement-up="moveUp = $event" 
    @on-movement-down="moveDown = $event"
    @on-movement-left="moveLeft = $event"
    @on-movement-right="moveRight = $event"
    @on-button-a="buttonA = $event"
    @on-button-b="buttonB = $event"
    @on-button-menu="menuIsOpen($event), buttonMenu=$event">
  
  </ControlsFrame>
  <!--inMenu is diferent bc menu is pressed releassed quick, so we have to put it on true and MenuMiddleware is the only one have to put it on false-->
    
  <!--we have to listen an emit that puts inWildEncounter to false to dterminate that its finished-->
  <WildEncounterMiddleware v-if="(inUnown || inWildEncounter) && loading"
  :pokemon-register="pokemonRegister"
  :button-a="buttonA"
  :button-b="buttonB"
  :move-up="moveUp"
  :move-down="moveDown"
  :in-unown="inUnown"
  @in-wild-encounter="this.inWildEncounter=$event"
  @update-pokemon-register="this.pokemonRegister = $event"
  @in-unown="inUnown = $event"

  
  
  /> 
  <!--we have to listen the updateInMenu when is closed, and listen the /save to send it to the backend --> 
  <MenuMiddleware v-if="inMenu && loading" 
  :in-pokedex="inPokedex"
  :in-wild-encounter="inWildEncounter"
  :button-a="buttonA"
  :button-b="buttonB"
  :move-up="moveUp"
  :move-down="moveDown"
  :button-menu="buttonMenu"

  
  :player-x="playerX" 
  :player-y="playerY"
  :facing-direction="facingDirection"
  :pokemon-register="pokemonRegister"

  @update-in-menu=" this.inMenu = $event"
  @update-in-pokedex="this.inPokedex = $event"
  
  
  
  />
  <!--don't need to put loading on v-if because only if callable with the MenuMiddleware and this one alredy have the v-if loadding-->
  
  <PokedexMiddleware v-if="inPokedex && inMenu" 

  
  :pokemon-register="pokemonRegister"
  :button-b="buttonB"
  @update-in-pokedex="inPokedex = $event"
  
  
  />
  
  
  
</template>