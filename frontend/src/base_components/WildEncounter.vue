<template>
    <div id="wildEncounterWrapper">
        <div
            v-for="(row, rowIndex) in composition"
            :key="'wild_encounter_row_' + rowIndex"
            class="row"
        >
            <div
                v-for="(block, blockIndex) in row"
                :key="'wild_encounter_block_' + rowIndex + '_' + blockIndex"
                alt=""
                class="block"
                :style="blockStyle(block)"
            />
        </div>
    </div>
</template>

<script>
import { compositionBlockIdToSrc, mapBlockSize } from '@/base_globals.js';
import { pokemonNames } from '@/globals.js';

export default {
    name: 'WildEncounter',
    emits: ['onError', 'onFinished', 'onPokemonCaught'],
    props: {
        pokemonId: {
            type: Number,
            required: true
        },
        isPokemonOwned: {
            type: Boolean,
            required: true
        },
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
        ashImageAtOffset(offsetX, offsetY) {
            return {
                id: 'ASH',
                numberOfBlocks: 7,
                offsetX,
                offsetY
            };
        },
        ballCatchingAtOffset(offsetX, offsetY) {
            return {
                id: 'BALL_CATCHING',
                numberOfBlocks: 2,
                offsetX,
                offsetY
            };
        },
        ballCaughtAtOffset(offsetX, offsetY) {
            return {
                id: 'BALL_CAUGHT',
                numberOfBlocks: 2,
                offsetX,
                offsetY
            };
        },
        selectorImageAtOffset(offset) {
            return offset === this.selectorPos ? 'DS_E' : '';
        },
        pokemonNameAtOffset(offset) {
            return offset < this.pokemonName.length ? this.pokemonName[offset] : '';
        },
        pokemonImageAtOffset(offsetX, offsetY) {
            return {
                id: `P_${this.pokemonName}`,
                numberOfBlocks: 5,
                offsetX,
                offsetY
            };
        },
        startComposition(functions) {
            const {a, i, n} = functions;
            return [
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     i(0,0), i(1,0), i(2,0), i(3,0), i(4,0), '',      ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     i(0,1), i(1,1), i(2,1), i(3,1), i(4,1), '',      ''     ],
                ['',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     i(0,2), i(1,2), i(2,2), i(3,2), i(4,2), '',      ''     ],
                ['',     a(0,0), a(1,0), a(2,0), a(3,0), a(4,0), a(5,0), a(6,0), '',     '',     '',     '',     '',     i(0,3), i(1,3), i(2,3), i(3,3), i(4,3), '',      ''     ],
                ['',     a(0,1), a(1,1), a(2,1), a(3,1), a(4,1), a(5,1), a(6,1), '',     '',     '',     '',     '',     i(0,4), i(1,4), i(2,4), i(3,4), i(4,4), '',      ''     ],
                ['',     a(0,2), a(1,2), a(2,2), a(3,2), a(4,2), a(5,2), a(6,2), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,3), a(1,3), a(2,3), a(3,3), a(4,3), a(5,3), a(6,3), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,4), a(1,4), a(2,4), a(3,4), a(4,4), a(5,4), a(6,4), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,5), a(1,5), a(2,5), a(3,5), a(4,5), a(5,5), a(6,5), '',     '',     '',     '',     '',     '',     '',     'A',    'S',    'H',    'NHL_V', ''     ],
                ['',     a(0,6), a(1,6), a(2,6), a(3,6), a(4,6), a(5,6), a(6,6), '',     'NHC_W','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHB_SE',''     ],
                ['DC_NW','DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N',  'DC_NE'],
                ['DL_W' ,'',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      'DL_E' ],
                ['DL_W' ,'W',    'i',    'l',    'd',    '',     n(0),   n(1),   n(2),   n(3),   n(4),   n(5),   n(6),   n(7),   '',     '',     '',     '',     '',      'DL_E' ],
                ['DL_W' ,'',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      'DL_E' ],
                ['DL_W' ,'a',    'p',    'p',    'e',    'a',    'r',    'e',    'd',    '!',    '',     '',     '',     '',     '',     '',     '',     '',     'DS_S',  'DL_E' ],
                ['DC_SW','DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S',  'DC_SE'],
            ];
        },
        mainComposition(functions) {
            const {a, i, n, o, s} = functions;
            return [
                ['',     '',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     'NHL_V', n(0),   n(1),   n(2),   n(3),   n(4),   n(5),   n(6),   o,      '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     'NHB_SW','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHC_E','',     '',     i(0,0), i(1,0), i(2,0), i(3,0), i(4,0), '',      ''     ],
                ['',     '',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     i(0,1), i(1,1), i(2,1), i(3,1), i(4,1), '',      ''     ],
                ['',     '',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     i(0,2), i(1,2), i(2,2), i(3,2), i(4,2), '',      ''     ],
                ['',     a(0,0),  a(1,0), a(2,0), a(3,0), a(4,0), a(5,0), a(6,0), '',     '',     '',     '',     '',     i(0,3), i(1,3), i(2,3), i(3,3), i(4,3), '',      ''     ],
                ['',     a(0,1),  a(1,1), a(2,1), a(3,1), a(4,1), a(5,1), a(6,1), '',     '',     '',     '',     '',     i(0,4), i(1,4), i(2,4), i(3,4), i(4,4), '',      ''     ],
                ['',     a(0,2),  a(1,2), a(2,2), a(3,2), a(4,2), a(5,2), a(6,2), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,3),  a(1,3), a(2,3), a(3,3), a(4,3), a(5,3), a(6,3), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,4),  a(1,4), a(2,4), a(3,4), a(4,4), a(5,4), a(6,4), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,5),  a(1,5), a(2,5), a(3,5), a(4,5), a(5,5), a(6,5), '',     '',     '',     '',     '',     '',     '',     'A',    'S',    'H',    'NHL_V', ''     ],
                ['',     a(0,6),  a(1,6), a(2,6), a(3,6), a(4,6), a(5,6), a(6,6), '',     'NHC_W','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHB_SE',''     ],
                ['DC_NW','DL_N',  'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DC_NW','DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N',  'DC_NE'],
                ['DL_W' ,'',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', '',     '',     '',     '',     '',     '',      'DL_E' ],
                ['DL_W' ,'',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', s(0),   'C',    'A',    'T',    'C',    'H',     'DL_E' ],
                ['DL_W' ,'',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', '',     '',     '',     '',     '',     '',      'DL_E' ],
                ['DL_W' ,'',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DL_W', s(1),   '',     '',     'R',    'U',    'N',     'DL_E' ],
                ['DC_SW','DL_S',  'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DC_SW','DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S',  'DC_SE'],
            ];
        },
        catchingComposition(functions) {
            const {a, b0, n, o} = functions;
            return [
                ['',     '',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     'NHL_V', n(0),   n(1),   n(2),   n(3),   n(4),   n(5),   n(6),   o,      '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     'NHB_SW','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHC_E','',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     '',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     '',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,0),  a(1,0), a(2,0), a(3,0), a(4,0), a(5,0), a(6,0), '',     '',     '',     '',     '',     '',     b0(0,0),b0(1,0),'',     '',     '',      ''     ],
                ['',     a(0,1),  a(1,1), a(2,1), a(3,1), a(4,1), a(5,1), a(6,1), '',     '',     '',     '',     '',     '',     b0(0,1),b0(1,1),'',     '',     '',      ''     ],
                ['',     a(0,2),  a(1,2), a(2,2), a(3,2), a(4,2), a(5,2), a(6,2), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,3),  a(1,3), a(2,3), a(3,3), a(4,3), a(5,3), a(6,3), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,4),  a(1,4), a(2,4), a(3,4), a(4,4), a(5,4), a(6,4), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,5),  a(1,5), a(2,5), a(3,5), a(4,5), a(5,5), a(6,5), '',     '',     '',     '',     '',     '',     '',     'A',    'S',    'H',    'NHL_V', ''     ],
                ['',     a(0,6),  a(1,6), a(2,6), a(3,6), a(4,6), a(5,6), a(6,6), '',     'NHC_W','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHB_SE',''     ],
                ['DC_NW','DL_N',  'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N',  'DC_NE'],
                ['DL_W' ,'',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      'DL_E' ],
                ['DL_W' ,'A',     'S',    'H',    '',     'u',    's',    'e',    'd',    '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      'DL_E' ],
                ['DL_W' ,'',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      'DL_E' ],
                ['DL_W' ,'P',     'O',    'K',    'é',    'B',    'A',    'L',    'L',    '!',    '',     '',     '',     '',     '',     '',     '',     '',     '',      'DL_E' ],
                ['DC_SW','DL_S',  'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S',  'DC_SE'],
            ];
        },
        caughtComposition(functions) {
            const {a, b1, n, o} = functions;
            return [
                ['',     '',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     'NHL_V', n(0),   n(1),   n(2),   n(3),   n(4),   n(5),   n(6),   o,      '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     'NHB_SW','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHC_E','',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     '',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     '',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,0),  a(1,0), a(2,0), a(3,0), a(4,0), a(5,0), a(6,0), '',     '',     '',     '',     '',     '',     b1(0,0),b1(1,0),'',     '',     '',      ''     ],
                ['',     a(0,1),  a(1,1), a(2,1), a(3,1), a(4,1), a(5,1), a(6,1), '',     '',     '',     '',     '',     '',     b1(0,1),b1(1,1),'',     '',     '',      ''     ],
                ['',     a(0,2),  a(1,2), a(2,2), a(3,2), a(4,2), a(5,2), a(6,2), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,3),  a(1,3), a(2,3), a(3,3), a(4,3), a(5,3), a(6,3), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,4),  a(1,4), a(2,4), a(3,4), a(4,4), a(5,4), a(6,4), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,5),  a(1,5), a(2,5), a(3,5), a(4,5), a(5,5), a(6,5), '',     '',     '',     '',     '',     '',     '',     'A',    'S',    'H',    'NHL_V', ''     ],
                ['',     a(0,6),  a(1,6), a(2,6), a(3,6), a(4,6), a(5,6), a(6,6), '',     'NHC_W','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHB_SE',''     ],
                ['DC_NW','DL_N',  'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N',  'DC_NE'],
                ['DL_W' ,'',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      'DL_E' ],
                ['DL_W' ,'T',     'h',    'e',    '',     'p',    'o',    'k',    'e',    'm',    'o',    'n',    '',     'w',    'a',    's',    '',     '',     '',      'DL_E' ],
                ['DL_W' ,'',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      'DL_E' ],
                ['DL_W' ,'c',     'a',    'u',    'g',    'h',    't',    '!',    '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DS_S',  'DL_E' ],
                ['DC_SW','DL_S',  'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S',  'DC_SE'],
            ];
        },
        runningAwayComposition(functions) {
            const {a, i, n, o} = functions;
            return [
                ['',     '',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     'NHL_V', n(0),   n(1),   n(2),   n(3),   n(4),   n(5),   n(6),   o,      '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     'NHB_SW','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHC_E','',     '',     i(0,0), i(1,0), i(2,0), i(3,0), i(4,0), '',      ''     ],
                ['',     '',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     i(0,1), i(1,1), i(2,1), i(3,1), i(4,1), '',      ''     ],
                ['',     '',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     i(0,2), i(1,2), i(2,2), i(3,2), i(4,2), '',      ''     ],
                ['',     a(0,0),  a(1,0), a(2,0), a(3,0), a(4,0), a(5,0), a(6,0), '',     '',     '',     '',     '',     i(0,3), i(1,3), i(2,3), i(3,3), i(4,3), '',      ''     ],
                ['',     a(0,1),  a(1,1), a(2,1), a(3,1), a(4,1), a(5,1), a(6,1), '',     '',     '',     '',     '',     i(0,4), i(1,4), i(2,4), i(3,4), i(4,4), '',      ''     ],
                ['',     a(0,2),  a(1,2), a(2,2), a(3,2), a(4,2), a(5,2), a(6,2), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,3),  a(1,3), a(2,3), a(3,3), a(4,3), a(5,3), a(6,3), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,4),  a(1,4), a(2,4), a(3,4), a(4,4), a(5,4), a(6,4), '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      ''     ],
                ['',     a(0,5),  a(1,5), a(2,5), a(3,5), a(4,5), a(5,5), a(6,5), '',     '',     '',     '',     '',     '',     '',     'A',    'S',    'H',    'NHL_V', ''     ],
                ['',     a(0,6),  a(1,6), a(2,6), a(3,6), a(4,6), a(5,6), a(6,6), '',     'NHC_W','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHL_H','NHB_SE',''     ],
                ['DC_NW','DL_N',  'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N', 'DL_N',  'DC_NE'],
                ['DL_W' ,'',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      'DL_E' ],
                ['DL_W' ,'G',     'o',    't',    '',     'a',    'w',    'a',    'y',    '',     's',    'a',    'f',    'e',    'l',    'y',    '!',    '',     '',      'DL_E' ],
                ['DL_W' ,'',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',      'DL_E' ],
                ['DL_W' ,'',      '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     '',     'DS_S',  'DL_E' ],
                ['DC_SW','DL_S',  'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S', 'DL_S',  'DC_SE'],
            ];
        }
    },
    computed: {
        composition() {
            const stateToCompositionFunc =  {
                'START': this.startComposition,
                'MAIN': this.mainComposition,
                'CATCHING': this.catchingComposition,
                'CAUGHT': this.caughtComposition,
                'RUNNING_AWAY': this.runningAwayComposition
            };
            const functions = {
                a: this.ashImageAtOffset,
                b0: this.ballCatchingAtOffset,
                b1: this.ballCaughtAtOffset,
                i: this.pokemonImageAtOffset,
                n: this.pokemonNameAtOffset,
                o: this.pokemonOwnedImage,
                s: this.selectorImageAtOffset
            };
            return stateToCompositionFunc[this.state](functions);
        },
        pokemonName() {
            return this.pokemonId in pokemonNames ? pokemonNames[this.pokemonId].name : '';
        },
        pokemonOwnedImage() {
            return this.isPokemonOwned ? 'B_O' : '';
        }
    }
};
</script>

<style scoped>
#wildEncounterWrapper {
    position: absolute;
    background-color: #f9f9f9;
    height: var(--screen-height);
    width: var(--screen-width);
    top: var(--height-offset);
    left: var(--height-offset);
    z-index: 300;
}
.row {
    display: flex;
    height: var(--composition-block-size);
}
.block {
    min-width: var(--composition-block-size);
    width: var(--composition-block-size);
    image-rendering: pixelated;
    background-size: 100%;
}
</style>
