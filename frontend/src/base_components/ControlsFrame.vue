<template>
    <div id="exteriorWrapper">
        <div id="topFrame" class="frame" />
        <div id="bottomFrame" class="frame" />
        <div id="leftFrame" class="frame">
            <button @mousedown="$emit('onMovementUp', true)" @mouseup="$emit('onMovementUp', false)">↑</button>
            <div id="horizontalMovementButtons">
                <button @mousedown="$emit('onMovementLeft', true)" @mouseup="$emit('onMovementLeft', false)">←</button>
                <button @mousedown="$emit('onMovementRight', true)" @mouseup="$emit('onMovementRight', false)">→</button>
            </div>
            <button @mousedown="$emit('onMovementDown', true)" @mouseup="$emit('onMovementDown', false)">↓</button>
            <button id="menuButton" @mousedown="$emit('onButtonMenu', true)" @mouseup="$emit('onButtonMenu', false)">MENU</button>
        </div>
        <div id="rightFrame" class="frame">
            <button @mousedown="$emit('onButtonB', true)" @mouseup="$emit('onButtonB', false)">B</button>
            <button @mousedown="$emit('onButtonA', true)" @mouseup="$emit('onButtonA', false)">A</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ControlsFrame',
    emits: [
        'onButtonA',
        'onButtonB',
        'onButtonMenu',
        'onMovementUp',
        'onMovementDown',
        'onMovementLeft',
        'onMovementRight'
    ]
};
</script>

<style>
body {
  --map-block-size: 64px;
  --composition-block-size: calc(var(--map-block-size) / 2);
  --screen-height: calc(var(--map-block-size) * 9);
  --screen-width: calc(var(--map-block-size) * 10);
  --height-offset: calc(50% - (var(--screen-height) / 2));
}
img {
  image-rendering: pixelated;
}
</style>

<style scoped>
#exteriorWrapper {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 20;
}
.frame {
    position: absolute;
}
#topFrame {
    top: 0;
    height: var(--height-offset);
    width: 100%;
    background-color: lightgray;
}
#bottomFrame {
    bottom: 0;
    height: var(--height-offset);
    width: 100%;
    background-color: lightgray;
}
#horizontalMovementButtons {
    display: flex;
    gap: 30px;
}
#leftFrame {
    left: 0;
    top: var(--height-offset);
    height: var(--screen-height);
    width: var(--height-offset);
    background: lightgray;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}
#leftFrame button {
    font-size: 30px;
}
#menuButton {
    margin-top: 40px;
}
#rightFrame {
    right: 0;
    top: var(--height-offset);
    height: var(--screen-height);
    width: calc(var(--height-offset) - var(--map-block-size));
    background: lightgray;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 30px;
}
#rightFrame button {
    font-size: 30px;
}
</style>
