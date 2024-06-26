<template>
  <v-form v-model="form" @submit.prevent="onSubmit">
    <div class="text-subtitle-1 text-medium-emphasis">Account</div>

    <v-text-field
      v-model="email"
      :readonly="loading"
      :rules="[required]"
      placeholder="Email Address"
      prepend-inner-icon="mdi-email-outline"
      variant="outlined"
      clearable
    ></v-text-field>

    <div
      class="text-subtitle-1 text-medium-emphasis d-flex align-center justify-space-between"
    >
      Password
    </div>

    <v-text-field
      v-model="password"
      :readonly="loading"
      :rules="[required]"
      :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
      :type="visible ? 'text' : 'password'"
      placeholder="Enter Password"
      prepend-inner-icon="mdi-lock-outline"
      variant="outlined"
      @click:append-inner="visible = !visible"
      clearable
    ></v-text-field>

    <div
      v-if="!status?.success"
      class="pb-2 text-red text-caption text-decoration-none"
    >
      {{ status?.msg }}
    </div>

    <v-card-actions>
      <v-btn
        :disabled="!form"
        :loading="loading"
        type="submit"
        size="large"
        variant="tonal"
        block
      >
        Sign In
      </v-btn>
    </v-card-actions>

    <div class="text-center mt-10">
      <v-btn @click="$emit('toReset')" variant="text">
        Forgot your password?
      </v-btn>
    </div>
  </v-form>
</template>

<script setup>
import { useRouter } from "vue-router";
import { useStore } from "vuex";
import { ref } from "vue";

const router = useRouter();
const store = useStore();

const visible = ref(false);

const form = ref(null);
const email = ref(null);
const password = ref(null);

const loading = ref(false);
const status = ref(null);

const onSubmit = async () => {
  if (!form.value) return;
  loading.value = true;

  const params = {
    email: email.value,
    password: password.value,
  };

  const res = await store.dispatch("user/login", params);
  loading.value = false;
  status.value = res;
  if (res.success) {
    router.go();
  }
};

const required = (v) => {
  return !!v || "Field is required";
};
</script>

<style lang="scss" scoped></style>
