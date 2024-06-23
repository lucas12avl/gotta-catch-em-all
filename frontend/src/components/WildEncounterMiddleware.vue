<script>
import WildEncounter from '@/base_components/WildEncounter.vue';

export default{
    props:['pokemonRegister','buttonA', 'buttonB','moveUp','moveDown', 'inUnown'],
    emits:['inWildEncounter','updatedPokemonRegister','inUnown'],

    components:{
        WildEncounter,
    },


    data(){
        return{
            
            state : 'START',
            selectorPos : 0,
            pokemonId : 0,
            isPokemonOwned : false,
            updatedPokemonRegister :  this.pokemonRegister //the updated pokedex that will pass to the parent after the wildencounter
            
            

        }
    },

    methods:{

        fetchCapture(){

            fetch('http://localhost:8081/capture')
            .then(response => {

               if(response.status == 200){//400 means catched so we have to update pokemonRegister, and change the state to caught (and do an emit)
                    
                    if(this.pokemonRegister[this.pokemonId] == false){
                        this.isPokemonOwned = true
                        this.updatedPokemonRegister[this.pokemonId] = true
                                                
                    }
                    this.state='CAUGHT'
                    //emit of the captured pokemon to the parent (the parent must save it on the pokedex dictionary)
                    this.$emit('updatedPokemonRegister', this.updatedPokemonRegister)
                }
                else{
                     this.state='MAIN'

                }
                
               
            


            })
            .catch((error) =>{
                this.state='MAIN'
                console.error('Error fetching initial info:', error)
            })

        }

    },


    mounted(){
        
        if(!this.inUnown){
        let pokemon = Math.random() <= 0.5 //true if is 5 or below 
        if(pokemon){this.pokemonId = 16} //id 16 = pidgey
        else{this.pokemonId = 19} // id 19 = rattata
        }
        else{ //if we are in the unown event 
            this.pokemonId = 201

        }
        if(this.updatedPokemonRegister[this.pokemonId] != true)
        {
            this.updatedPokemonRegister[this.pokemonId] = false 
        }
        else{
            this.isPokemonOwned = true
        }
    
         
    },
    
    watch:{

        
        //if starts andbuttonA or buttonB pressed, we go to MAIN state 
        buttonA(){
            if(this.buttonA ==true){

                switch( this.state) {

                 case 'START':
                     this.state ='MAIN'
                 break
                 case 'MAIN':
                        if(this.selectorPos == 0){
                            this.state ='CATCHING'
                            this.fetchCapture()
                        }
                        else{
                            this.state = 'RUNNING_AWAY'
                        }
                    break
                    case 'RUNNING_AWAY':
                        this.$emit('updatedPokemonRegister', this.updatedPokemonRegister) //we have to update the podeked before exit the wildEnconuter because may be the first time we seen the pokemon
                        this.$emit('inWildEncounter', false)
                        this.$emit('inUnown', false)
                    break
                    case 'CAUGHT':
                        this.$emit('inWildEncounter', false)
                        this.$emit('inUnown', false)
                    break


                }
            }
            
        },
        buttonB(){

            if(this.buttonB == true){
                
                switch( this.state) {

                 case 'START':
                        this.state ='MAIN'
                 break
                    case 'RUNNING_AWAY':
                        this.$emit('inWildEncounter', false)
                        this.$emit('inUnown', false)
                  break
                    case 'CAUGHT':
                      this.$emit('inWildEncounter', false)
                      this.$emit('inUnown', false)
                 break
                }
            }
        },

       //only on MIAN we have a menu to select what we want to do with the wild pokemon 
        moveUp(){
            if(this.moveUp &&this.state =='MAIN'){
                this.selectorPos =  (++this.selectorPos % 2)
            }

        },
        moveDown(){
            if(this.moveDown &&this.state =='MAIN'){
                  //cant be the same as moveDown because when this arives to 0 starts negatives and do module on negatives
                  this.selectorPos = this.selectorPos === 0 ? 1 : (--this.selectorPos) % 2;
            }

        }


    },
   
}


</script>

<template>

    <WildEncounter
    :state="state"
    :selector-pos="selectorPos"
    :pokemon-id="pokemonId"
    :is-pokemon-owned="isPokemonOwned"

    />
</template>

<style>
</style>