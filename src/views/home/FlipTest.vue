<template>
    <div class="bg-yellow-400 p-5 transitioan-all" 
        :class="{
            'opacity-100': !isOpen,
            'opacity-0': isOpen
        }" 
        data-flip-key="flip-1"
        @click="flip()"
    >
        Test
    </div>

    <div class="bg-yellow-400 fixed bottom-0 left-0 w-full transition-all"
        :class="{
            'hidden': !isOpen,
            'visible': isOpen
        }" 
        data-flip-key="flip-1"
        @click="flip()"
    >
        This is a fullscreen test
    </div>

    <!-- <div class="bg-green-200 fixed inset-0 w-full transitioan-all"
        :class="{
            'opacity-0': !isOpen,
            'opacity-100': isOpen
        }" 
        data-flip-key="flip-1"
        @click="flip()"
    >
        This is a fullscreen test
    </div> -->
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, nextTick } from 'vue'
// import Flipping from 'flipping';

export default defineComponent({
    name: "FlipTest",
    setup() {
        let Flip
        const isOpen = ref<boolean>(false)

        onMounted(() => {
            Flip = new Flipping({
                onRead: elements => console.log("Flip read", elements)
            })
        })
        const flip = async() => {
            // Before a layout change happens
            Flip.read()

            // Any effect that changes the layout
            isOpen.value = !isOpen.value
            await nextTick()

            // After a layout change happens
            // With an adapter, this will start the FLIP animation
            Flip.flip()
        }

        return {
            isOpen,
            flip,
        }
    }
})
</script>