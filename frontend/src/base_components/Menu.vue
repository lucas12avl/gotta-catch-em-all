<template>
    <div id="menuWrapper">
        <div
            v-for="(row, rowIndex) in composition"
            :key="'menu_row_' + rowIndex"
            class="menuRow"
        >
            <div
                v-for="(block, blockIndex) in row"
                :key="'menu_block_' + rowIndex + '_' + blockIndex"
                alt=""
                class="menuBlock"
                :style="blockStyle(block)"
            />
        </div>
    </div>
</template>

<script>
import { compositionBlockIdToSrc, mapBlockSize } from '@/base_globals.js';

export default {
    name: 'Menu',
    props: {
        selectorPos: {
            type: Number,
            required: true
        },
        state: {
            type: String,
            required: true
        }
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
        selectorImageAtOffset(offset) {
            return offset === this.selectorPos ? 'DS_E' : '_';
        },
        menuComposition(functions) {
            const {s} = functions;
            return [
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DC_NW','DL_N', 'DL_N', 'DL_N','DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DC_NE'],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', s(0),   'P',    'O',   'K',    'E',    'D',    'é',    'X',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', s(1),   '_',    '_',   '_',    'S',    'A',    'V',    'E',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', s(2),   '_',    '_',   '_',    'E',    'X',    'I',    'T',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DC_SW','DL_N', 'DL_N', 'DL_N','DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DC_NE'],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
            ];
        },
        savingComposition(functions) {
            const {s} = functions;
            return [
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DC_NW','DL_N', 'DL_N', 'DL_N','DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DC_NE'],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', s(0),   'P',    'O',   'K',    'E',    'D',    'é',    'X',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', s(1),   '_',    '_',   '_',    'S',    'A',    'V',    'E',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', s(2),   '_',    '_',   '_',    'E',    'X',    'I',    'T',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DC_SW','DL_N', 'DL_N', 'DL_N','DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DC_NE'],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['DC_NW','DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N','DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DC_NE'],
                ['DL_W' ,'_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['DL_W', 'N',    'o',    'w',    '_',    's',    'a',    'v',    'i',    'n',    'g',    '.',    '.',    '.',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['DL_W' ,'_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['DL_W' ,'_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['DC_SW','DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S','DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DC_SE'],
            ];
        },
        savedComposition(functions) {
            const {s} = functions;
            return [
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DC_NW','DL_N', 'DL_N', 'DL_N','DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DC_NE'],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', s(0),   'P',    'O',   'K',    'E',    'D',    'é',    'X',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', s(1),   '_',    '_',   '_',    'S',    'A',    'V',    'E',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', s(2),   '_',    '_',   '_',    'E',    'X',    'I',    'T',    'DL_E' ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DC_SW','DL_N', 'DL_N', 'DL_N','DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DC_NE'],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',    '',     '',     '',     '',     '',     ''     ],
                ['DC_NW','DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N','DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DC_NE'],
                ['DL_W' ,'_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['DL_W', 'A',    'S',    'H',    '_',    's',    'a',    'v',    'e',    'd',    '_',    '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['DL_W' ,'_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',    '_',   '_',    '_',    '_',    '_',    '_',    'DL_E' ],
                ['DL_W' ,'t',    'h',    'e',    '_',    'g',    'a',    'm',    'e',    '!',    '_',    '_',    '_',    '_',   '_',    '_',    '_',    '_',    'DS_S', 'DL_E' ],
                ['DC_SW','DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S','DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DC_SE'],
            ];
        }
    },
    computed: {
        composition() {
            const stateToCompositionFunc =  {
                'START': this.menuComposition,
                'SAVING': this.savingComposition,
                'SAVED': this.savedComposition
            };
            const functions = {
                s: this.selectorImageAtOffset
            };
            return stateToCompositionFunc[this.state](functions);
        }
    }
};
</script>

<style scoped>
#menuWrapper {
    position: absolute;
    height: var(--screen-height);
    width: var(--screen-width);
    top: var(--height-offset);
    left: var(--height-offset);
    z-index: 290;
}
.menuRow {
    display: flex;
    height: var(--composition-block-size);
}
.menuBlock {
    min-width: var(--composition-block-size);
    width: var(--composition-block-size);
    image-rendering: pixelated;
    background-size: 100%;
}
</style>
