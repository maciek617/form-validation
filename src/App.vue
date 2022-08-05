<template>
  <form @submit.prevent="submitForm" class="max-w-sm m-auto">

    <div class="flex flex-col items-center mt-7">

      <label>Name<span class="text-blue-500">*</span></label>
      <input type="text" placeholder="Enter your name" v-model="state.name" class="w-full p-2 rounded-xl bg-none border border-gray-500">
      <p v-if="v$.name.$error" class="text-red-600 mt-3">This field cannot be empty</p>

      <label class="mt-4">Email<span class="text-blue-500">*</span></label>
      <input type="email" placeholder="Enter your email" v-model="state.email" class="w-full p-2 rounded-xl bg-none border border-gray-500" autocomplete="email">
      <p v-if="v$.email.$error" class="text-red-600 mt-3">{{v$.email.$errors[0].$message}}</p>

      <label class="mt-4">Password<span class="text-blue-500">*</span></label>
      <input type="password" placeholder="Enter your password" v-model="state.password.password" class="w-full p-2 rounded-xl bg-none border border-gray-500" autocomplete="new-password">
      <p v-if="v$.password.password.$error" class="text-red-600 mt-3">{{v$.password.password.$errors[0].$message}}</p>

      <label class="mt-4">Confirm password<span class="text-blue-500">*</span></label>
      <input type="password" placeholder="Enter your password again" v-model="state.password.confirmPassword" class="w-full p-2 rounded-xl bg-none border border-gray-500" autocomplete="new-password">
      <p v-if="v$.password.confirmPassword.$error" class="text-red-600 mt-3">{{v$.password.confirmPassword.$errors[0].$message}}</p>
    </div>

    <button class="bg-gray-900 text-white py-2 px-4 mt-5">Submit</button>

  </form>
</template>

<script>
import {reactive, computed} from "vue";
import useVuelidate from '@vuelidate/core'
import {required, email, sameAs, minLength, helpers} from '@vuelidate/validators'

export default {
  name: "mortgageView",
  setup() {
    const state = reactive({
      name: '',
      email: '',
      password: {
        password: '',
        confirmPassword: ''
      }
    });

    const rules = computed(() => {
      return {
        name: {required},
        email: {required, email},
        password: {
          password: {
            minLength: minLength(8), required, containsPasswordRequirement: helpers.withMessage(() => 'The password requires an uppercase, lowercase, number and special character', (value) =>
                /(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[!@#$%^&*])/.test(value.toString()))
          },
          confirmPassword: {sameAs: sameAs(state.password.password), required}
        }
      }
    });

    const v$ = useVuelidate(rules, state);

    const submitForm = async () => {
      const isFormCorrect = await v$.value.$validate();
      if(!isFormCorrect) return
      alert('passed')
    }

    return {state, v$, submitForm}
  }
}
</script>

<style scoped>

</style>