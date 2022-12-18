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
  isSubmitting.value = true ;
  error.value = props.validate(data.value) ;
  if (Object.keys(error.value).length === 0) {
    props.onSubmit(data.value) ;
  }

  setTimeout(() => {
    isSubmitting.value = false ;
  }, 2000) ;

} ;


const setData = (newData, fieldName) => {
 data.value[fieldName] = newData ;
} ;

const error = ref([]) ;
const isSubmitting = ref(false) ;



provide(setDatakey,setData) ;
provide(dataKey,data) ;

</script>
