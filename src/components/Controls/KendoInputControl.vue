<template>
    <KendoInput 
        :label="control.label" 
        :id="control.id + '-input'"
        :class="styles.control.input"
        :value="control.data"
        :valid="!control.errors"
        :disabled="!control.enabled"
        :autofocus="appliedOptions.focus"
        :placeholder="appliedOptions.placeholder"
        @change="onChange"
        @focus="isFocused = true"
        @blur="isFocused = false"
    ></KendoInput>
</template>

<script lang="ts">
    import { defineComponent } from 'vue';
    import { useJsonFormsControl, type RendererProps, rendererProps } from '@jsonforms/vue';
    import { type ControlElement, type JsonFormsRendererRegistryEntry, rankWith, isStringControl, isControl } from '@jsonforms/core';
    import KendoInput from './KendoInput.vue';
    import { useVanillaControl } from '@jsonforms/vue-vanilla';

    const controlRenderer = defineComponent({
        name: 'control-renderer',
        components: {
            KendoInput,
        },
        props: {
            ...rendererProps<ControlElement>(),
            label: String,
        },
        setup (props: RendererProps<ControlElement>) {
            return useVanillaControl(
                useJsonFormsControl(props),
                (target) => target.value || undefined
            );
        },
    })

    export default controlRenderer;
    
    export const entry: JsonFormsRendererRegistryEntry = {
        renderer: controlRenderer,
        tester: rankWith(1001, isStringControl),
    }
</script>