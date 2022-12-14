<template>
 <slot :data="data" :error="error" :isSubmitting="isSubmitting" :handleSubmit="submitMethod"
 />

</template>

<script setup>
import {ref, provide, onMounted, defineProps} from 'vue' ;
import {
  dataKey, setDatakey
} from "@/components/provider/FormProviderKeys";

const props = defineProps({
  onSubmit: {
    type: Function,
    required: true,
  },
  initialData: {
    type: Object,
    required: false,
  },
  validate: {
    type: Function,
    required: false,
  },
}) ;

const data = ref(props.initialData) ;
onMounted(() => {
  if (props.initialData) {
    for(const item in props.initialData) {
      setData(props.initialData[item], item) ;
    }
  }
}) ;

const submitMethod = () => {
  error.value = props.validate(data.value) ;
  if (error.value.length === 0) {
    isSubmitting.value = true ;
    props.onSubmit(data.value) ;
  }
} ;


const setData = (newData, fieldName) => {
 data.value[fieldName] = newData ;
} ;

const error = ref([]) ;
const isSubmitting = ref(false) ;



provide(setDatakey,setData) ;
provide(dataKey,data) ;

</script>
