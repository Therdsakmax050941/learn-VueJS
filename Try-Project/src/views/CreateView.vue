<template>
  <div class="row justify-start items-start content-start">
    <div class="col-6 self-center offset-3 q-gutter-md q-gutter-sm q-col-gutter-sm" style="overflow: auto;">
      <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
        <q-input v-model="fname" outlined label="First Name" />
        <q-input v-model="lname" outlined label="Last Name" />
        <q-input v-model="username" outlined  label="Username" />
        <q-input v-model="password" outlined label="Password" />
        <q-input v-model="email" outlined label="Email" />
        <q-input v-model="avatar" outlined label="Avatar" />
        <q-btn label="Submit" type="submit" color="primary" />
      </q-form>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import router from "../router";

const fname = ref("John");
const lname = ref("inwza");
const username = ref("Johninwza");
const password = ref("John");
const email = ref("John0505@gmail.com");
const avatar = ref(
  "https://images.pexels.com/photos/4340489/pexels-photo-4340489.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1"
);

const onSubmit = () => {
  const data = JSON.stringify({
    fname: fname.value,
    lname: lname.value,
    username: username.value,
    password: password.value,
    email: email.value,
    avatar: avatar.value,
  });

  var myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/json");
  var reqOptions = {
    method: "POST",
    headers: myHeaders,
    body: data,
    redirect: "follow",
  };
  fetch("https://www.melivecode.com/api/users/create", reqOptions)
    .then((response) => response.json())
    .then((result) => {
      alert(result.message);
      if (result.status === "ok") {
        router.push("/");
      }
    })
    .catch((error) => console.log("error", error));
};
const onReset = () => {
  fname.value = "";
  lname.value = "";
  username.value = "";
  password.value = "";
  email.value = "";
  avatar.value = null;
};
</script>

<style>
</style>