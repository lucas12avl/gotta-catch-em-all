<template>
    <div id="playerAndHighGrassWrapper">
        <img id="playerHead" class="player" alt="" :src="playerImgSrc" />
        <img id="playerBody" ref="playerBody" class="player" alt="" :src="playerImgSrc" />
    </div>
</template>

<script>
import standing_south from '@/assets/player/standing_south.png';
import standing_north from '@/assets/player/standing_north.png';
import standing_west from '@/assets/player/standing_west.png';
import standing_east from '@/assets/player/standing_east.png';
import walking_south from '@/assets/player/walking_south.gif';
import walking_north from '@/assets/player/walking_north.gif';
import walking_west from '@/assets/player/walking_west.gif';
import walking_east from '@/assets/player/walking_east.gif';
import { mapBlockSize } from '@/base_globals';

const stringToAsset = {
    "@/assets/player/standing_south.png": standing_south,
    "@/assets/player/standing_north.png": standing_north,
    "@/assets/player/standing_west.png": standing_west,
    "@/assets/player/standing_east.png": standing_east,
    "@/assets/player/walking_south.gif": walking_south,
    "@/assets/player/walking_north.gif": walking_north,
    "@/assets/player/walking_west.gif": walking_west,
    "@/assets/player/walking_east.gif": walking_east,
};

export default {
    name: 'Player',
    props: {
        facing: {
            type: String,
            required: true
        },
        movementType: {
            type: String,
            required: true
        },
        enteringGrass: {
            type: Boolean,
            required: true
        }
    },
    computed: {
        playerImgSrc() {
            if (this.facing && this.movementType) {
                const gifOrPng = this.movementType !== 'standing' ? 'gif' : 'png';
                const url = `@/assets/player/${this.movementType}_${this.facing}.${gifOrPng}`;
                return stringToAsset[url];
            }
            return '';
        },
        playerBodyObjectPosition() {
            return `0 -${mapBlockSize / 2}px`;
        }
    },

    watch:{

        enteringGrass(){
            this.$refs.playerBody.style.zIndex = (this.enteringGrass) ? 9 : 30;
                
        }
  }
    
    
};
</script>

<style scoped>
#playerAndHighGrassWrapper {
    width: 100%;
    height: 100%;
}
.player {
    position: absolute;
    height: calc(var(--map-block-size) / 2);
    width: var(--map-block-size);
    object-fit: none;
    left: calc(50% - (var(--map-block-size) / 2));
}
#playerHead {
    bottom: calc(50% + (var(--map-block-size) / 4));
    object-position: 0 0;
    z-index: 32;
}
#playerBody {
    bottom: calc(50% - (var(--map-block-size) / 4));
    object-position: v-bind(playerBodyObjectPosition);
    z-index: 30;
}
</style>