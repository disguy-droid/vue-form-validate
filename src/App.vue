<script setup lang="ts">
import { useForm, useField } from "vee-validate";
import * as Yup from "yup";

const { handleSubmit } = useForm({
  validationSchema: Yup.object({
    username: Yup.string().required().uppercase(),
    email: Yup.string().required().email(),
    password: Yup.string().required().min(8).max(20),
  }),
});

const { value: username, errorMessage: usernameError } =
  useField<string>("username");
const { value: email, errorMessage: emailError } = useField<string>("email");
const { value: password, errorMessage: passwordError } =
  useField<string>("password");

const onSubmit = handleSubmit(({ username, email, password }) => {
  console.log(
    JSON.stringify(
      {
        username: username,
        email: email,
        password: password,
      },
      null,
      2
    )
  );
  // console.log(JSON.stringify(values, null, 2));
});

const capitalize = (name: string | undefined) => {
  if (name !== undefined) {
    const firstCharacter = name[0].toUpperCase();
    const rest = name.slice(1);

    return firstCharacter + rest;
  }
};
</script>

<template>
  <div
    class="min-h-screen bg-gray-50 text-gray-900 w-full grid place-content-center"
  >
    <h1 class="text-3xl lg:text-4xl mb-12 font-medium">Login Form</h1>
    <form
      @submit.prevent="onSubmit($event)"
      class="flex flex-col space-y-2.5 w-96 mx-auto"
    >
      <div class="space-y-1">
        <input
          type="text"
          name="username"
          id="username"
          placeholder="Username"
          class="py-2.5 px-2.5 bg-gray-100 rounded-md text-gray-900 w-full"
          :class="{ 'outline outline-2 outline-red-500': usernameError }"
          v-model="username"
        />
        <span
          class="inline-block text-red-800"
          v-show="usernameError"
          >{{ capitalize(usernameError) }}</span
        >
      </div>
      <div class="space-y-1">
        <input
          type="email"
          name="email"
          id="email"
          placeholder="Email"
          class="py-2.5 px-2.5 bg-gray-100 rounded-md text-gray-900 w-full"
          :class="{ 'outline outline-2 outline-red-500': emailError }"
          v-model="email"
        />
        <span
          class="inline-block text-red-800"
          v-show="emailError"
          >{{ capitalize(emailError) }}</span
        >
      </div>
      <div class="space-y-1">
        <input
          type="password"
          name="password"
          id="password"
          placeholder="Password"
          class="py-2.5 px-2.5 bg-gray-100 rounded-md text-gray-900 w-full"
          v-model="password"
          :class="{ 'outline outline-2 outline-red-500': passwordError }"
        />
        <span
          class="inline-block text-red-800"
          v-show="passwordError"
          >{{ capitalize(passwordError) }}</span
        >
      </div>
      <button
        type="submit"
        class="py-2.5 w-full bg-gray-900 text-gray-50 rounded-md hover:bg-gray-900/90 transition"
      >
        Submit
      </button>
    </form>
  </div>
</template>

<style scoped></style>
