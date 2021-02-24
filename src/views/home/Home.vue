<template>
    <div v-motion="'smoothestDiv'"
        :initial="{
            opacity: 0,
            y: 100,
        }"
        :enter="{
            opacity: 1,
            y: 0,
            transition: {
                type: 'tween',
                repeat: Infinity,
                repeatType: 'mirror',
                duration: 1000
            },
        }"
        style="background-color: salmon; width: 100px; height: 100px;"
    >
    </div>
    <button @click="toogle">toogle</button>

    <!-- <div class="grid-container p-4 min-h-screen">
        <div class="option w-full h-full relative"
            @click="selectOption('one')"
        >
            <div class="option bg-blue-300 flex items-center rounded-md justify-center w-full h-full relative"
                data-flip-key="one"
            >
                <transition name="fade" mode="out-in">
                    <div class="w-1/2" v-if="showTargetContent">
                        <svg class="w-full" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z" />
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 13a3 3 0 11-6 0 3 3 0 016 0z" />
                        </svg>
                    </div>
                </transition>
            </div>
            <div class="shadow-option absolute inset-0"
                :class="{
                    'bg-gray-200': selectedOption === 'one' && selectedFlipKey === 'one'
                }" 
                data-flip-key="shadow-one"
            >
            </div>
        </div>
        <div class="option w-full h-full relative"
            @click="selectOption('two')"
        >
            <div class="option bg-yellow-300 flex items-center justify-center rounded-md w-full h-full relative"
                data-flip-key="two"
            >
                <transition name="fade" mode="out-in">
                    <div class="w-1/2" v-if="showTargetContent">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                    </div>
                </transition>
            </div>
            <div class="shadow-option absolute inset-0"
                :class="{
                    'bg-gray-200': selectedOption === 'two' && selectedFlipKey === 'two'
                }" 
                data-flip-key="shadow-two"
            >
            </div>
        </div>
        <div class="option w-full h-full relative"
            @click="selectOption('three')"
        >
            <div class="option bg-green-300 flex items-center justify-center rounded-md w-full h-full relative"
                data-flip-key="three"
            >
                <transition name="fade" mode="out-in">
                    <div class="w-1/2" v-if="showTargetContent">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 10h.01M12 10h.01M16 10h.01M9 16H5a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v8a2 2 0 01-2 2h-5l-5 5v-5z" />
                        </svg>
                    </div>
                </transition>
            </div>
            <div class="shadow-option absolute inset-0" 
                :class="{
                    'bg-gray-200': selectedOption === 'three' && selectedFlipKey === 'three'
                }"
                data-flip-key="shadow-three"
            >
            </div>
        </div>

        <div class="target-container rounded-2xl p-4 h-full relative"
            :class="{
                'bg-blue-300': selectedOption === 'one',
                'bg-yellow-300': selectedOption === 'two',
                'bg-green-300': selectedOption === 'three',
                'opacity-0': selectedOption !== selectedFlipKey
            }"
            :data-flip-key="selectedFlipKey"
        >
            <transition name="fade">
                <div v-if="showTargetContent">
                    <div key="one" v-if="selectedOption === 'one'">
                        Selected one
                    </div>
                    <div key="two" v-if="selectedOption === 'two'">
                        Selected two
                    </div>
                    <div key="three" v-if="selectedOption === 'three'">
                        Selected three
                    </div>
                </div>
            </transition>
        </div>
    </div> -->
    
</template>

<script lang="ts">
import { ref, onMounted, nextTick } from 'vue'
import { useRouter } from 'vue-router'
import { useMotions } from '@vueuse/motion'

const ANIMATION_DURATION = 225

export default {
    name: "Home",

    components: {
    },

    setup() {
        const router = useRouter()
        let Flip
        let smoothestDiv

        // const startAutoPic = () => {
        //     router.push('/record')
        // }

        onMounted(() => {
            Flip = new Flipping({
                onRead: (elements: HTMLElement) => console.log("Flip read", elements),
                duration: ANIMATION_DURATION,
                easing: 'ease-in-out'
            })
            
            // Get access to motion instance using useMotions
            const motions = useMotions()
            smoothestDiv = motions.smoothestDiv
        })

        const showTargetContent = ref<boolean>(true)
        const selectedFlipKey = ref<string>("one")
        const selectedOption = ref<string>("one")
        const selectOption = async(opt: string) => {
            if (selectedOption.value === opt)
                return

            showTargetContent.value = false

            setTimeout(async() => {
                Flip.read()
                selectedFlipKey.value = `shadow-${opt}`
                await nextTick()
                Flip.flip()

                selectedFlipKey.value = opt

                setTimeout(() => {
                    requestAnimationFrame(async () => {
                        Flip.read()
                        selectedOption.value = opt
                        await nextTick()
                        Flip.flip()

                        setTimeout(() => showTargetContent.value = true, ANIMATION_DURATION)
                    })
                }, ANIMATION_DURATION)
            }, 200)
        }

        const toogle = () => {
            smoothestDiv.apply({
                x: 100,
                y: 100
            })
        }

        return {
            showTargetContent,
            selectedFlipKey,
            selectedOption,
            selectOption,
            toogle
        }
    }
}
</script>

<style lang="less">

.grid-container {
    display: grid;
    grid-template-columns: 30% 30% 30%;
    grid-template-rows: 15% auto;
    gap: 1em 5%;
}
.option {
    transition: opacity .2s ease;
}
.target-container {
    grid-column: 1 / 4;
}

</style>