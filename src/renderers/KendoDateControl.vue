<template>
    <control-wrapper
      v-bind="controlWrapper"
      :styles="styles"
      :is-focused="isFocused"
      :applied-options="appliedOptions"
      class="k-mb-5"
    >
        <KendoDate 
            :id="control.id + '-input'"
            :class="styles.control.input"
            :valid="!control.errors"
            :disabled="!control.enabled"
            :autofocus="appliedOptions.focus"
            :placeholder="appliedOptions.placeholder"
            @change="onChange"
            @focus="isFocused = true"
            @blur="isFocused = false"
        ></KendoDate>
    </control-wrapper>
</template>

<script lang="ts">
    import { defineComponent } from 'vue';
    import { useJsonFormsControl, type RendererProps, rendererProps } from '@jsonforms/vue';
    import { type ControlElement, type JsonFormsRendererRegistryEntry, rankWith, isDateControl } from '@jsonforms/core';
    import KendoDate from '../components/controls/KendoDate.vue';
    import { useVanillaControl } from '@jsonforms/vue-vanilla';
    import ControlWrapper from '../components/controls/ControlWrapper.vue';

    const controlRenderer = defineComponent({
        name: 'control-renderer',
        components: {
            ControlWrapper,
            KendoDate,
        },
        props: {
            ...rendererProps<ControlElement>(),
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
        tester: rankWith(1002, isDateControl),
    }
</script>