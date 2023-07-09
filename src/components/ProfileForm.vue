<template>
    <json-forms
      :data="data"
      :schema="schema"
      :uischema="uischema"
      :renderers="renderers"
      @change="onChange"
    />
    <br>
    <button :disabled="data.age < 18">
      Next
    </button>
  </template>
  
  <script lang="ts">
  
  import { JsonForms } from '@jsonforms/vue';
  import { vanillaRenderers } from '@jsonforms/vue-vanilla';
  import { defineComponent } from 'vue';
  import moment from 'moment';
  
  const renderers = [
    ...vanillaRenderers,
  ];
  
  export default defineComponent({
    components: {
      JsonForms,
    },
    data() {
      return {
        renderers: Object.freeze(renderers),
        data: {
          firstName: "",
          lastName: "",
          birthDay: "",
          email: "",
          age: 0,
        },
        schema: {
          properties: {
            firstName: {
              type: "string"
            },
            lastName: {
              type: "string"
            },
            birthDay: {
              type: "string",
              format: 'date',
            },
            email: {
              type: "string",
              // format: "email",
              pattern: "^\\S+@\\S+\\.\\S+$",
            },
            age: {
              type: "number",
            }
          }
        },
        uischema: {
          type: 'VerticalLayout',
          elements: [
            {
              type: 'Control',
              label: "First Name",
              scope: '#/properties/firstName',
            },
            {
              type: 'Control',
              label: "Last Name",
              scope: '#/properties/lastName',
            },
            {
              type: 'Control',
              label: "Birth Day",
              scope: '#/properties/birthDay',
            },
            {
              type: 'Control',
              label: "Email",
              scope: '#/properties/email',
              rule: {
                effect: "SHOW",
                condition: {
                  scope: "#/properties/age",
                  schema: { 
                    minimum: 19,
                  }
                }
              }
            },
          ],
        },
      };
    },
    methods: {
      onChange(event: any) {
        // calculate age value
        event.data.age = event.data.birthDay ? moment().diff(moment(event.data.birthDay, 'YYYY-MM-DD'), 'years') : 0;

        this.data = event.data;

        console.log(this.data)
      },
    },
  });
  </script>