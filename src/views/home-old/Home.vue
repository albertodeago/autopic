<template>
    <div class="p-4 text-center min-h-screen">

        <span class="block text-xl font-semibold pb-4">Configure AutoPic</span>
        
        <!-- TARGET -->
        <div class="text-md text-gray-900 mt-4 mb-2">
            Select the subject you want to take <br>photos of
        </div>
        <transition-group>
            <div class="target text-md text-gray-900 bg-gray-400"
                :class="{
                    'opened': isSelectingTarget
                }"
                @click="() => { if (!isSelectingTarget) isSelectingTarget = true }"
                key="target"
            >
                <div class="font-bold text-2xl  mt-10 transition-all"
                    @click.stop="isSelectingTarget = !isSelectingTarget"
                >
                    {{ appState.target }}
                </div>

                <transition name="fade" mode="out-in" :duration="150">
                    <div v-show="isSelectingTarget"
                        class="target-choices my-2 overflow-y-auto">
                        <div class=""
                            v-for="object in cocoSsdObjectList"
                            :key="object"
                            @click.stop="onTargetClick(object)"
                        >
                            <span :class="{'font-bold': object === appState.target}">
                                {{ object }}
                            </span>
                        </div>
                    </div>
                </transition>

            </div>
        </transition-group>

        <transition-group>
            <!-- <div key="source" @click="appState.checkInterval = appState.checkInterval === 10 ? 1 : 10"
                class="bg-green-500 transition-all duration-1000"
                :class="{
                    'fixed inset-0 bg-green-200': appState.checkInterval === 10
                }"
            >
                Open me
            </div> -->
            <!-- <div key="target" @click="() => { if(appState.checkInterval === 10) appState.checkInterval = 1 }"
                class="fixed inset-0 bg-green-200 opacity-0  transition-all overflow-hidden"
                :class="{
                    'w-0': appState.checkInterval !== 10,
                    'opacity-100': appState.checkInterval === 10
                }"
            >
                I'm the target
            </div> -->
        </transition-group>

        <!-- FLIP ANIMATION TEST -->
        <FlipTest />
        <!-- END FLIP ANIMATION -->

        <!-- NORMAL TIME -->
        <div class="text-md text-gray-900 mt-8 mb-2">
            Select the amount of seconds between photos when the subject is <span class="font-bold">not</span> seen
        </div>
        <div class="font-bold text-2xl mx-auto w-16 h-16 
            border-2 border-gray-400 rounded-full
            flex items-center justify-center"
        >
            {{ appState.checkInterval }}
        </div>
        <InputRange v-model="appState.checkInterval" min="1" max="60" step="1" />

        <!-- STALKING TIME -->
        <div class="text-md text-gray-900 mt-8 mb-2">
            Select the amount of seconds between photos when the subject is seen
        </div>
        <div class="font-bold text-2xl mx-auto w-16 h-16 
            border-2 border-gray-400 rounded-full
            flex items-center justify-center"
        >
            {{ appState.stalkingInterval }}
        </div>
        <InputRange v-model="appState.stalkingInterval" min="0.5" max="10" step="0.5" />

        <!-- ALERT -->
        <div class="text-md text-gray-900 mt-8 mb-2">
            Do you want to emit a sound when the target is detected?
        </div>
        <InputText v-model="appState.message" placeholder="Message">
            <template #icon>
                <svg @click="playSound()" class="inline-block w-10" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z" />
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
            </template>
        </InputText>

        <!-- START -->
        <button class="mx-auto mt-8 mb-4 w-20 h-20 flex items-center justify-center
            font-bold text-2xl bg-gray-500 rounded-full"
            @click="startAutoPic"
        >
            Start
        </button>
    </div>
</template>

<script lang="ts">
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
import InputRange from './InputRange.vue'
import InputText from './InputText.vue'
import { appState, playSound } from '/@/shared/' 
import { cocoSsdObjectList } from './cocoSsdObjectList'
// import { languageList } from './languageList'
import FlipTest from './FlipTest.vue'

export default {
    name: "Home",

    components: {
        InputRange,
        InputText,
        FlipTest
    },

    setup() {
        const router = useRouter()

        const startAutoPic = () => {
            router.push('/record')
        }

        // const width = computed(() => ())

        const isSelectingTarget = ref(false)
        const onTargetClick = target => {
            appState.target = target
            isSelectingTarget.value = false
        }

        return {
            appState,
            playSound,
            startAutoPic,
            cocoSsdObjectList,
            // languageList,
            isSelectingTarget,
            onTargetClick
        }
    }
}
</script>

<style lang="less">

.target {
    transition: all .3s ease;
    width: 150px;
    height: 150px;
    margin: 0 auto;
    padding: 1rem;
    border-radius: 100%;
}
.target.opened {
    width: 300px;
    height: 600px;
    border-radius: 1em;
}
.target-choices {
    height: calc(100% - 75px);
}


</style>