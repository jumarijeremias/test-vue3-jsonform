<template>
    <div v-if="visible" :id="id" :class="styles.control.root">
      <klabel :editor-id="id + '-input'" :editor-valid="!errors">
        {{ computedLabel }}
      </klabel>
      <div :class="styles.control.wrapper">
        <slot></slot>
      </div>
      <error v-if="errors">
        {{ errors }}
      </error>
      <Hint v-else-if="showDescription">{{ description }}</Hint>
    </div>
  </template>
  
<script lang="ts">
  import { isDescriptionHidden, computeLabel } from '@jsonforms/core';
  import { defineComponent, type PropType } from 'vue';
  import { type Options, type Styles } from '@jsonforms/vue-vanilla';
  import { Error, Hint, Label } from "@progress/kendo-vue-labels";
  
  export default defineComponent({
    name: 'ControlWrapper',
    components: {
      error: Error,
      Hint,
      klabel: Label,
    },
    props: {
      id: {
        required: true,
        type: String,
      },
      description: {
        required: false as const,
        type: String,
        default: undefined,
      },
      errors: {
        required: false as const,
        type: String,
        default: undefined,
      },
      label: {
        required: false as const,
        type: String,
        default: undefined,
      },
      appliedOptions: {
        required: false as const,
        type: Object as PropType<Options>,
        default: undefined,
      },
      visible: {
        required: false as const,
        type: Boolean,
        default: true,
      },
      required: {
        required: false as const,
        type: Boolean,
        default: false,
      },
      isFocused: {
        required: false as const,
        type: Boolean,
        default: false,
      },
      styles: {
        required: true,
        type: Object as PropType<Styles>,
      },
    },
    computed: {
      showDescription(): boolean {
        return !isDescriptionHidden(
          this.visible,
          this.description,
          this.isFocused,
          !!this.appliedOptions?.showUnfocusedDescription
        );
      },
      computedLabel(): string {
        return computeLabel(
          this.label,
          this.required,
          !!this.appliedOptions?.hideRequiredAsterisk
        );
      },
    },
  });
  </script>