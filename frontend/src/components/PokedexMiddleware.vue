<script>
import { pokemonNames } from '@/globals'
import Pokedex from '../base_components/Pokedex.vue'
export default{

    props: ['pokemonRegister', 'buttonB'],
    emits: ['updateInPokedex'],

    components: {
        Pokedex,
    },

    computed : {

        numberOfSeenPokemons(){
            
            return Object.keys(this.pokemonRegister).length;

        },

        numberOfOwnedPokemons(){
            
            return Object.values(this.pokemonRegister).filter(isOwned => isOwned).length;
        },

        pokemonsInfo() {

            // Object.entries(this.pokemonRegister) --> converts de dictionary in a array of arrays
            /*
            with the dicctionary 

                let pokemonRegister = {
                    "16": true,
                    "19": false
                };

            we obtain:
                [
                    ["16", true],
                    ["19", false]
                ]
            
            */
            //map is used to transform each sub-array into a new object. The arrow function takes each [id, captured] pair as arguments.
            //map is a function that creates another object, not overwrites the original 
            let obj =  Object.entries(this.pokemonRegister).map(([id, capturado]) => {
                return {
                    id: id, 
                    owned: capturado, 
                    name: pokemonNames[id].name
                }
            });


            /* if we seen  pidgey and captured ratta
            (2) [{…}, {…}]
            0: {id: '16', owned: true, name: 'PIDGEY'}
            1: {id: '19', owned: false, name: 'RATTATA'}
            */
            return obj
        
        }

    },
 
    watch: {

        buttonB() {
            if (this.buttonB) {
                this.$emit('updateInPokedex', false)
            }
        }

    }
    
       

}


</script>

<template>

    <Pokedex
        :number-of-seen-pokemons="numberOfSeenPokemons"
        :number-of-owned-pokemons="numberOfOwnedPokemons"
        :pokemons-info="pokemonsInfo"
    />
</template>

<style>
</style>