<template>
  <component v-if="typeof as === 'string'"
      :is="as" :name="name"
      @input="setData($event.target.value, name)" :value="formData[name]" />

  <component v-else :is="as" :name="name"
      :modelValue="formData[name]" v-on:update:modelValue="editData" />
</template>

<script setup>

import {defineProps, inject, ref} from "vue";
import { setDatakey,dataKey } from "@/components/provider/FormProviderKeys";

const props = defineProps({
  "as": {
    type: [String, Object],
    required: false,
    default: "input"
  },
  "name": {
    type: String,
    required: true
  }
}) ;

const formData = inject(dataKey) ;

const setData = inject(setDatakey) ;
const editData = (newData) => {

  setData(newData, props.name) ;
} ;
</script>
