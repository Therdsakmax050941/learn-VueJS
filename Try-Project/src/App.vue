<template>
  <q-layout view="hHh lpR fFf">

    <q-header elevated class="bg-primary text-white" height-hint="98">
      <q-toolbar>
        <q-toolbar-title>
          My App
        </q-toolbar-title>
        <q-tabs>
        <q-route-tab to="/" label="Home" />
        <q-route-tab to="/create" label="Create" />
        <q-route-tab to="/about" label="About" />
        <q-route-tab @click="Logout" label="Logout" />
      </q-tabs>
      </q-toolbar>

    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

  </q-layout>
</template>

<script setup>
import {  RouterView } from 'vue-router'
import router from "./router";

const Logout = () => {
  localStorage.removeItem('token');
  router.push('/login');
}
if (!localStorage.getItem('token')) {
  router.push('/login');
}
</script>