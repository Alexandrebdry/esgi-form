<script  setup>
import FormProvider from "@/components/provider/FormProvider.vue";
import Field from "@/components/Field.vue";
import Captcha from "@/components/Captcha.vue";

const onSubmit = (data) => {
  console.log(data) ;
} ;

const initialData = {
  name: "John",
  email: "test@test.fr"
} ;

const validate = (data) => {
  const errors = {} ;
  if (data.name.length < 3) {
    errors["name"] = "Name must be at least 3 characters long" ;
  }
  if (data.email.length < 3) {
    errors["email"] = "Email must be at least 3 characters long" ;
  }
  return errors;
};
const options = Array.from({ length: 9 }, (_, i) => ({
  id: i,
  href: `https://picsum.photos/200?random=${i}`,
}));

</script>

<template>

  <FormProvider

    :onSubmit="onSubmit"
    :initialData="initialData"
    :validate="validate"

    v-slot="{data, error, handleSubmit, isSubmitting}"
    >
    <form @submit.prevent="handleSubmit">
      <Field :name="'name'" />
      <Field type="email" :name="'email'" />
      <Field type="password" :name="'password'" />
      <Field type="password" :name="'passwordConfirmation'" />
      <Field :as="Captcha" name="captcha" :options="options" />
      <button type="submit" :disabled="isSubmitting" >Submit</button>
    </form>
  </FormProvider>

</template>