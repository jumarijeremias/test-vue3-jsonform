<template>
    <json-forms
      :data="data"
      :schema="schema"
      :uischema="uischema"
      :renderers="renderers"
      @change="onChange"
    />
  </template>
  
  <script lang="ts">
  
  import { JsonForms } from '@jsonforms/vue';
  import { vanillaRenderers } from '@jsonforms/vue-vanilla';
  import { defineComponent } from 'vue';
  
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
            },
          ],
        },
      };
    },
    methods: {
      onChange(event: any) {
        this.data = event.data;
      },
    },
  });
  </script>