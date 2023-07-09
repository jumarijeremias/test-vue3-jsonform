<template>
    <datepicker 
        :format="'dd/MMMM/yyyy'" 
        :valid="valid"
        :id="id"
        :disabled="disabled"
        :placeholder="placeholder"
        @change="handleChange"
        @blur="handleBlur"
        @focus="handleFocus"
    >
    </datepicker>
</template>

<script lang="ts">
    import { DatePicker } from '@progress/kendo-vue-dateinputs';
    import moment from 'moment';

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
        },
        components: {
            datepicker: DatePicker,
        },
        emits: {
            input: null,
            change: null,
            blur: null,
            focus: null
        },
        methods: {
            handleChange(e: any){
                let dateValue = moment(e.value).format('YYYY-MM-DD');
                e.value = e.target.value = dateValue;
                this.$emit('change', e);
            },
            handleBlur(e: any){
                this.$emit('blur', e);
            },
            handleFocus(e: any){
                this.$emit('focus', e);
            }
        }
    };
    
    </script>