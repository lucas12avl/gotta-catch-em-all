<script>
import Menu from '@/base_components/Menu.vue'
export default{

    props:['inPokedex','inWildEncounter','buttonA', 'buttonB','moveUp', 'moveDown', 'playerX', 'playerY', 'facingDirection','pokemonRegister', 'buttonMenu'],

    emits:['updateInPokedex','updateInMenu'],

    components:{
        Menu,
    },
        data(){

            return{
                updateInMenu : true, //when leaves emit this to the parent component 
                updateInPokedex : this.inPokedex,
                state : 'START', // (’START’ | ’SAVING’ | ’SAVED’)
                selectorPos : 0,
                

            }
        },

        methods:{

            
            handleSaveGame(){

              

                const saveGameFile = {
                    x: this.playerX,
                    y: this.playerY,
                    direction: this.facingDirection,
                    pokedex: this.pokemonRegister,
                }

                fetch('http://localhost:8081/save',
                {
                    method: 'POST',
                    headers: {
                     'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(saveGameFile)
                })
                .then(response => {

                    if (response.status == 200) {
                     
                        this.state = 'SAVED'
                        
                    }
                }).catch(error => console.log('Error:', error))
                
               


            }

        },


        watch:{ //create a watch with menubutton

            moveDown(){
                if(this.moveDown){ //we only can move down if there are more option down the current 

                    this.selectorPos =  (++this.selectorPos % 3)
                   
                }
            },

            moveUp(){

                if(this.moveUp){ //we only can move down if there are more option down the current 

                    //cant be the same as moveDown because when this arives to 0 starts negatives and do module on negatives
                    this.selectorPos = this.selectorPos === 0 ? 2 : (--this.selectorPos) % 3;
                  
                }
            },

            buttonA(){
                if(this.buttonA){ //we need to only do something if it's pressed (true), if we do not control this, the pressed/releassed action will be percive as a double-click pressed
                   
                  
                    switch(this.selectorPos){
                      case 0:
                           //we are going to the pokedex 
                            this.updateInPokedex = true
                            this.$emit('updateInPokedex', this.updateInPokedex) 
                            //the pokkedexMiddleware have to emit to the father when closes 
                            //and the father give to MenuMiddleware if inPokedex is true or false 
                        break

                        case 1: /*   /save     */
                            //not the same press the selectorPos = 1 when we are in the menu and when we already saved
                            if(this.state == 'SAVED'){ //wen we saved the game,  must quit the menu, 
                                this.updateInMenu = false
                                this.$emit('updateInMenu', this.updateInMenu)
                            }
                            else{ //if we are going to save 
                                this.state = 'SAVING' 
                                this.handleSaveGame()//we have to put () ?
                            }
                        break
                        case 2:
                            this.updateInMenu = false
                            this.$emit('updateInMenu', this.updateInMenu)
                        break

                    }
                
            }


            },

            buttonB(){
                // !this.inPokedex because if we are on the pokedex and want to exit, the menu must appear (so, if we don't put it if we press buttonB, pokedex and menu diappears at once)
                if(this.buttonB && this.state != 'SAVING' && !this.inPokedex){//we need to only do something if it's pressed (true), if we do not control this, the pressed/releassed action will be percive as a double-click pressed
                 
                    // B button must exit the menu  and don't care the position of the selctor
                    
                    this.updateInMenu = false
                    this.$emit('updateInMenu', this.updateInMenu)
                       
                    
                }
            },

            buttonMenu(){

                if(this.buttonMenu && !this.inPokedex){ //if we press the buttonMenu on the menu, we have to exit. but not exit when pokedex is open! 
                    this.updateInMenu = false
                    this.$emit('updateInMenu', this.updateInMenu)


                }

            }


            


        }
    

}


</script>

<template>

    <Menu
    :state="state"
    :selector-pos="selectorPos"
    />
</template>

<style>
</style>