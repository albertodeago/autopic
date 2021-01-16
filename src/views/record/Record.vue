<template>
    <main>
        <Suspense>
            <template #default>
                <Video />
            </template>
            <template #fallback>
                <span>Loading...</span>
            </template>
        </Suspense>
    </main>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import Video from './Video.vue'

export default defineComponent({
    name: "Record",

    components: {
        Video
    },

    setup() {
        if (!('mediaDevices' in navigator && 'getUserMedia' in navigator.mediaDevices)) {
            console.error("Device does not support this app")
            // TODO: show an error page...
        }
    }
})
</script>