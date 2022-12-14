<template>
 <slot :data="data" :error="error" :isSubmitting="isSubmitting" :handleSubmit="submitMethod"
 />

</template>

<script setup>
import {ref, provide, onMounted, defineProps} from 'vue' ;
import {
  dataKey, setDatakey,  setIsSubmittingkey
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
  if (props.validate(data.value)) {
    isSubmitting.value = true ;
    props.onSubmit(data.value) ;
  }
  else {
    setError("Invalid form");
  }
} ;


const setData = (newData, fieldName) => {
 data.value[fieldName] = newData ;
} ;

const error = ref([]) ;
const setError = (newError) => {
 error.value = newError ;
} ;

const isSubmitting = ref(false) ;
const setIsSubmitting = (newIsSubmitting) => {
 isSubmitting.value = newIsSubmitting ;
} ;



provide(setDatakey,setData) ;
provide(setIsSubmittingkey,setIsSubmitting) ;
provide(dataKey,data) ;

</script>
