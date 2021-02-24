<template>
    <input type="range" :value="modelValue" v-bind="$attrs" 
        @input="$emit('update:modelValue', parseFloat($event.target.value))" 
    >
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
    name: "InputRange",

    props: {
        modelValue: Number
    },

    emits: ['update:modelValue']
})
</script>

<style lang="less" scoped>

// light version
@color__default: rgba(255,255,255,.5);
@color__hover:   rgba(255,255,255,.7);
@color__active:  rgba(255,255,255,1);


input[type=range] {
    box-sizing: border-box;
    font-size: 1rem;
    line-height: 1;
    height: 2em;
    background-color: transparent;
    cursor: pointer;

  // * * * RESET * * *

    -webkit-appearance: none;
    width: 100%;

    &::-webkit-slider-thumb {
        -webkit-appearance: none;
    }
    &:focus {
        outline: none;
    }
    &::-ms-track {
        width: 100%;
        cursor: pointer;
        background: transparent; 
        border-color: transparent;
        color: transparent;
    }

  // * * * TRUMP * * *

    .mx-trange-trump(){
        width: 1.5em;
        height: 1.5em;
        margin-top: 0.15em;
        background-color: rgba(156, 163, 175, var(--tw-bg-opacity));
        // background-color: @color__fill;
        border-radius: 1em;
        border: 2px solid @color__default;
        cursor: pointer;
    }

    &::-webkit-slider-thumb {
        -webkit-appearance: none;
        .mx-trange-trump();
    }

    &::-moz-range-thumb {
        .mx-trange-trump();
    }

    &::-ms-thumb {
        .mx-trange-trump();
    }

    &:hover{
        &::-webkit-slider-thumb { border-color: @color__hover; }
        &::-moz-range-thumb     { border-color: @color__hover; }
        &::-ms-thumb            { border-color: @color__hover; }
    }

  //&:focus,
    &:active{
        &::-webkit-slider-thumb { border-color: @color__active; }
        &::-moz-range-thumb     { border-color: @color__active; }
        &::-ms-thumb            { border-color: @color__active; }
    }

    // * * * TRACK * * *

    .mx-range-track(){
        width: 100%;
        cursor: pointer;
        height: 1em;
        border-bottom: 2px solid @color__default;
        background-color: transparent;
    }


    &::-webkit-slider-runnable-track {
        .mx-range-track();
    }

    &:focus::-webkit-slider-runnable-track {

    }

    &::-moz-range-track {
        .mx-range-track()
    }

    &::-ms-track {
        background: transparent;
        border-color: transparent;
        color: transparent;
    }
    &::-ms-fill-lower {

    }
    &:focus::-ms-fill-lower {

    }
    &::-ms-fill-upper {

    }
    &:focus::-ms-fill-upper {

    }
}
</style>