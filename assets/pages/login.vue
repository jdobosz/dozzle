<template>
  <div class="card w-96 flex-shrink-0 bg-base-lighter shadow-2xl">
    <div class="card-body">
      <form action="" method="post" @submit.prevent="onLogin" ref="form">
        <div class="form-control">
          <label class="label">
            <span class="label-text"> {{ $t("label.username") }} </span>
          </label>
          <input
            class="input input-bordered"
            type="text"
            name="username"
            autocomplete="username"
            v-model="username"
            autofocus
          />
        </div>
        <div class="form-control">
          <label class="label">
            <span class="label-text">Password</span>
          </label>
          <input
            class="input input-bordered"
            type="password"
            name="password"
            autocomplete="current-password"
            v-model="password"
          />
        </div>
        <label class="label text-red" v-if="error">
          {{ $t("error.invalid-auth") }}
        </label>
        <div class="form-control mt-6">
          <button class="btn btn-primary" type="submit">{{ $t("button.login") }}</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script lang="ts" setup>
const { t } = useI18n();

setTitle(t("title.login"));

let error = $ref(false);
let username = $ref("");
let password = $ref("");
let form: HTMLFormElement | undefined = $ref();

async function onLogin() {
  const url = config.authProvider === "simple" ? "/api/token" : "/api/validateCredentials";
  const response = await fetch(withBase(url), {
    body: new FormData(form),
    method: "POST",
  });

  if (response.status == 200) {
    error = false;
    window.location.href = withBase("/");
  } else {
    error = true;
  }
}
</script>
<route lang="yaml">
meta:
  layout: splash
</route>
