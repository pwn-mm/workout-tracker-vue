<template>
  <div class="max-w-screen-sm px-4 py-10 mx-auto">
    <!-- // ! Error Handling -->
    <div v-if="errorMsg" class="p-4 mb-10 rounded-md shadow-lg bg-light-grey">
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!-- // ? Register -->
    <form
      @submit.prevent="register"
      class="flex flex-col p-8 rounded-md shadow-lg bg-light-grey"
    >
      <h1 class="mb-4 text-3xl text-at-light-green">Register</h1>

      <div class="flex flex-col mb-2">
        <label for="email" class="mb-1 text-sm text-at-light-green"
          >Email</label
        >
        <input
          type="text"
          required
          class="p-2 text-gray-500 focus:outline-none"
          id="email"
          v-model="email"
        />
      </div>

      <div class="flex flex-col mb-2">
        <label for="password" class="mb-1 text-sm text-at-light-green"
          >Password</label
        >
        <input
          type="password"
          required
          class="p-2 text-gray-500 focus:outline-none"
          id="password"
          v-model="password"
        />
      </div>

      <div class="flex flex-col mb-2">
        <label for="confirmPassword" class="mb-1 text-sm text-at-light-green"
          >Confirm Password</label
        >
        <input
          type="password"
          required
          class="p-2 text-gray-500 focus:outline-none"
          id="confirmPassword"
          v-model="confirmPassword"
        />
      </div>

      <button
        type="submit"
        class="self-start px-6 py-2 mt-6 text-sm text-white duration-200 border-2 border-transparent border-solid rounded-sm bg-at-light-green hover:border-at-light-green hover:bg-white hover:text-at-light-green"
      >
        Register
      </button>

      <router-link class="mt-6 text-sm text-center" :to="{ name: 'Login' }">
        Already have an account? <span class="text-at-light-green">Login</span>
      </router-link>
    </form>
  </div>
</template>

<script>
import { ref } from "vue";
import { supabase } from "../supabase/init";
import { useRouter } from "vue-router";

export default {
  name: "register",
  setup() {
    //? Create data / vars
    const router = useRouter();
    const email = ref(null);
    const password = ref(null);
    const confirmPassword = ref(null);
    const errorMsg = ref(null);

    //? Register function
    const register = async () => {
      if (password.value === confirmPassword.value) {
        try {
          const { error } = await supabase.auth.signUp({
            email: email.value,
            password: password.value,
          });
          if (error) throw error;
          router.push({ name: "Login" });
        } catch (error) {
          errorMsg.value = error.message;
          setTimeout(() => {
            errorMsg.value = null;
          }, 5000);
        }
        return;
      }
      errorMsg.value = "Error: Passwords do not match!";
      setTimeout(() => {
        errorMsg.value = null;
      }, 5000);
    };

    return {
      email,
      password,
      confirmPassword,
      errorMsg,
      register,
    };
  },
};
</script>
