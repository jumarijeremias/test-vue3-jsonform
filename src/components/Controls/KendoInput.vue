<template>
    <kInput
        :valid="valid"
        :id="id"
        :value="value"
        :disabled="disabled"
        :placeholder="placeholder"
        @input="handleChange"
        @blur="handleBlur"
        @focus="handleFocus"
    />
</template>

<script lang="ts">
    import { Input as kInput } from "@progress/kendo-vue-inputs";

    export default {
        props: {
            optional: Boolean,
            disabled: Boolean,
            placeholder: String,
            touched: Boolean,
            label: String,
            validationMessage: String,
            hint: String,
            id: String,
            valid: Boolean,
            value: {
                type: String,
                default: ''
            }
        },
        components: {
            kInput
        },
        emits: {
            input: null,
            change: null,
            blur: null,
            focus: null
        },
        methods: {
            handleChange(e: any){
                this.$emit('change', e);
            },
            handleBlur(e: any){
                this.$emit('blur', e);
            },
            handleFocus(e: any){
                this.$emit('focus', e);
            }
        }
    }

</script>