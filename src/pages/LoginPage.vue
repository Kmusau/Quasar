<template>
  <q-page class="flex flex-center">
    <div class="">
      <q-input
        class="q-py-md"
        v-model="email"
        filled
        type="email"
        label="Email"
      />
      <q-input
        class="q-py-md"
        v-model="password"
        filled
        :type="isPwd ? 'password' : 'text'"
        label="Password"
      >
        <template v-slot:append>
          <q-icon
            :name="isPwd ? 'visibility_off' : 'visibility'"
            class="cursor-pointer"
            @click="isPwd = !isPwd"
          />
        </template>
      </q-input>
    </div>
  </q-page>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    return {
      password: ref(""),
      isPwd: ref(true),
    };
  },
};
</script>
