<template>
    <div class="w-full rounded-full bg-gray-500 relative h-12">
        <input class="bg-transparent absolute left-0 top-0 bottom-0 w-3/4
            border-0 focus:outline-none focus:ring-0"
            :style="{'width': withIcon ? 'calc(100% - 3rem);' : '100%'}"
            type="text"
            :value="modelValue"
            v-bind="$attrs"
            @input="$emit('update:modelValue', $event.target.value)" 
        >
        <div v-if="withIcon" class="absolute right-2 mt-1 color-">
            <slot name="icon">
            
            </slot>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
    name: "InputText",

    props: {
        modelValue: String
    },

    setup(props, { slots  }) {
        const withIcon = !!slots.icon

        return {
            withIcon
        }
    },

    emits: ['update:modelValue']
})
</script>

<style lang="less" scoped>

input::-webkit-input-placeholder {
    font-style: italic;
    color: white;
    opacity: 0.5;
}
</style>