<template>
    <div class="k-mb-5">
        <klabel :editor-id="id" :editor-valid="valid" :disabled="disabled" :optional="optional">
            {{ label }}
        </klabel>
        <div class="k-form-field-wrap">
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
            <error v-if="showValidationMessage">
                {{ validationMessage }}
            </error>
            <Hint v-else>{{ hint }}</Hint>
        </div>
    </div> 
</template>

<script lang="ts">
    import { Error, Hint, Label } from "@progress/kendo-vue-labels";
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
            error: Error,
            Hint,
            klabel: Label,
            kInput
        },
        computed: {
            showValidationMessage() : any {
                return this.$props.touched && this.$props.validationMessage;
            },
            showHint() : any {
                return !this.showValidationMessage && this.$props.hint;
            },
            hintId() : any {
                return this.showHint() ? `${this.$props.id}_hint` : "";
            },
            errorId() : any {
                return this.showValidationMessage() ? `${this.$props.id}_error` : "";
            }
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