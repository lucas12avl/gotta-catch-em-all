<template>
    <div id="pokedexWrapper">
        <div
            v-for="(row, rowIndex) in composition"
            :key="'pokedex_row_' + rowIndex"
            class="pokedexRow"
        >
            <div
                v-for="(block, blockIndex) in row"
                :key="'pokedex_block_' + rowIndex + '_' + blockIndex"
                alt=""
                class="pokedexBlock"
                :style="blockStyle(block)"
            />
        </div>
    </div>
</template>

<script>
import { compositionBlockIdToSrc, mapBlockSize } from '@/base_globals.js';

export default {
    name: 'Pokedex',
    props: {
        numberOfOwnedPokemons: {
            type: Number,
            required: true
        },
        numberOfSeenPokemons: {
            type: Number,
            required: true
        },
        pokemonsInfo: {
            type: Array,
            required: true
        }
    },
    data() {
        return {
            state: 'START'
        };
    },
    methods: {
        blockStyle(block) {
            const blockId = typeof block === 'string' ? block : block.id;
            let style = blockId in compositionBlockIdToSrc ? `background-image: url(${compositionBlockIdToSrc[blockId]});` : '';
            if (typeof block !== 'string' && 'numberOfBlocks' in block) {
                const multiplier = mapBlockSize / 2;
                style += `background-size: calc(${multiplier}px * ${block['numberOfBlocks']});`;
                if ('offsetX' in block && 'offsetY' in block) {
                    const offsetX = block['offsetX'] * multiplier;
                    const offsetY = block['offsetY'] * multiplier;
                    style += `background-position: -${offsetX}px -${offsetY}px`;
                }
            }
            return style;
        },
        ballOwnAtOffset(offset) {
            if (offset < 0 || offset >= this.pokemonsInfo.length) {
                return '';
            }
            return this.pokemonsInfo[offset].owned ? 'B_O': '';
        },
        pokemonNameAtOffset(offsetX, offsetY) {
            if (offsetY < 0 || offsetY >= this.pokemonsInfo.length || offsetX < 0) {
                return '';
            }
            const pokemonInfo = this.pokemonsInfo[offsetY];
            const pokemonName = pokemonInfo.name;
            if (offsetX >= pokemonName.length) {
                return '';
            }
            return pokemonName[offsetX];
        },
        pokemonIdAtOffset(offsetX, offsetY) {
            if (offsetY < 0 || offsetY >= this.pokemonsInfo.length || offsetX < 0 || offsetX > 2) {
                return '';
            }
            const pokemonId = this.pokemonsInfo[offsetY].id;
            const idSplit = pokemonId.split('');
            const offset = 3 - idSplit.length;
            const pos = offsetX - offset;
            if (pos < 0) {
                return '0';
            }
            return idSplit[pos];
        },
        startComposition(functions) {
            const {b, i, n} = functions;
            const own = this.numberOfOwnedPokemons.toString();
            const seen = this.numberOfSeenPokemons.toString();
            return [
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'NHL_V', '', '',  '',  '',   '' ],
                ['',     'C',    'O',    'N',    'T',    'E',    'N',    'T',    'S',    '',     '',     '',     '',     '',     'NHL_V', '', '',  '',  '',   '' ],
                ['',     i(0,0), i(1,0), i(2,0), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'P_S',   '', 'S', 'E', 'E',  'N'],
                ['',     '',     '',     b(0),   n(0,0), n(1,0), n(2,0), n(3,0), n(4,0), n(5,0), n(6,0), n(7,0), '',     '',     'NHL_V', '', '',  '',  seen, '' ],
                ['',     i(0,1), i(1,1), i(2,1), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'P_S',   '', '',  '',  '',   '' ],
                ['',     '',     '',     b(1),   n(0,1), n(1,1), n(2,1), n(3,1), n(4,1), n(5,1), n(6,1), n(7,1), '',     '',     'NHL_V', '', 'O', 'W', 'N',  '' ],
                ['',     i(0,2), i(1,2), i(2,2), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'P_S',   '', '',  '',  own,  '' ],
                ['',     '',     '',     b(2),   n(0,2), n(1,2), n(2,2), n(3,2), n(4,2), n(5,2), n(6,2), n(7,2), '',     '',     'NHL_V', '', '',  '',  '',   '' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'P_S',   '', '',  '',  '',   '' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'NHL_V', '', '',  '',  '',   '' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'P_S',   '', '',  '',  '',   '' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'NHL_V', '', '',  '',  '',   '' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'P_S',   '', '',  '',  '',   '' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'NHL_V', '', '',  '',  '',   '' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'P_S',   '', '',  '',  '',   '' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'NHL_V', '', '',  '',  '',   '' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'P_S',   '', '',  '',  '',   '' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'NHL_V', '', '',  '',  '',   '' ],
            ];
        }
    },
    computed: {
        composition() {
            const stateToCompositionFunc =  {
                'START': this.startComposition
            };
            const functions = {
                b: this.ballOwnAtOffset,
                i: this.pokemonIdAtOffset,
                n: this.pokemonNameAtOffset
            };
            return stateToCompositionFunc[this.state](functions);
        }
    }
};
</script>

<style scoped>
#pokedexWrapper {
    position: absolute;
    background-color: #f9f9f9;
    height: var(--screen-height);
    width: var(--screen-width);
    top: var(--height-offset);
    left: var(--height-offset);
    z-index: 300;
}
.pokedexRow {
    display: flex;
    height: var(--composition-block-size);
}
.pokedexBlock {
    min-width: var(--composition-block-size);
    width: var(--composition-block-size);
    image-rendering: pixelated;
    background-size: 100%;
}
</style>
