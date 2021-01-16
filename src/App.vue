<template>
    <main class="bg-gray-200">
        <router-view v-slot="{ Component }">
            <transition name="fade">
                <component :is="Component" />
            </transition>
        </router-view>
    </main>
</template>

<script lang="ts">
import { defineComponent, onMounted } from 'vue'
import { initScreen } from '/@/shared/useScreen'

export default defineComponent({
    name: "App",
    setup() {
        onMounted(() => initScreen())
    }
})
</script>

<style>
.fade-enter-active,
.fade-leave-active {
    opacity: 1;
    transition: opacity .3s ease;
}
.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>