<template>
  <div class="row justify-start items-start content-start">
    <div
      class="col-6 self-center offset-3 q-gutter-md q-gutter-sm q-col-gutter-sm"
      style="overflow: auto"
    >
      <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
        <q-input v-model="id" outlined label="ID" disable />
        <q-input v-model="fname" outlined label="First Name" />
        <q-input v-model="lname" outlined label="Last Name" />
        <q-input v-model="username" outlined label="Username" />
        <q-input v-model="password" outlined label="Password" />
        <q-input v-model="email" outlined label="Email" />
        <q-input v-model="avatar" outlined label="Avatar" />
        <q-btn label="Update" type="submit" color="primary" />
      </q-form>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRoute } from "vue-router";
import router from "../router";

const route = useRoute();

const id = ref(route.params.id);
const fname = ref("");
const lname = ref("");
const username = ref("");
const password = ref("");
const email = ref("");
const avatar = ref("");

const fetchData = () => {
  fetch("https://www.melivecode.com/api/users/" + id.value)
    .then((response) => response.json())
    .then((data) => {
      fname.value = data.user.fname;
      lname.value = data.user.lname;
      username.value = data.user.username;
      password.value = data.user.password;
      email.value = data.user.email;
      avatar.value = data.user.avatar;
    });
};
fetchData();
const onSubmit = () => {
  const raw = JSON.stringify({
    id: id.value,
    fname: fname.value,
    lname: lname.value,
    username: username.value,
    password: password.value,
    email: email.value,
    avatar: avatar.value,
  });

  var myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/json");

  var requestOptions = {
    method: "PUT",
    headers: myHeaders,
    body: raw,
    redirect: "follow",
  };

  fetch("https://www.melivecode.com/api/users/update", requestOptions)
    .then((response) => response.json())
    .then((result) => {
      alert(result.message);
      if (result.status === 'ok') {
        router.push("/");
      }
    })
    .catch((error) => console.log("error", error));
};
</script>
