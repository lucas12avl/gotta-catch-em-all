<script>

import Player from '../base_components/Player.vue'


export default{
    props:['facingDirection', 'movementType', 'isInGrass'], 

    components:{
        Player,
       
    },
    data(){
        return{
            
        }
    
    },
 
   
}


</script>

<template>
    <Player :facing="facingDirection" :movement-type="movementType" :entering-grass="isInGrass"/>
</template>

<style>
</style>