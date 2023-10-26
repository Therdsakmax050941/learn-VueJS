<template>
    <div class="q-pa-md q-gutter-md q-flex q-items-center q-justify-center">
      <h2>Login</h2>
      <form @submit.prevent="login" class="q-gutter-md q-pa-md q-card">
        <div>
          <label for="username" class="q-mb-sm">Username:</label>
          <q-input
            type="text"
            id="username"
            v-model="formData.username"
            required
            outlined
            class="q-pa-sm"
          ></q-input>
        </div>
        <div>
          <label for="password" class="q-mb-sm">Password:</label>
          <q-input
            :type="showPassword ? 'text' : 'password'"
            id="password"
            v-model="formData.password"
            required
            outlined
            class="q-pa-sm"
          >
            <template v-slot:append>
              <q-icon
                name="remove_red_eye"
                v-if="showPassword"
                @click="togglePasswordVisibility"
                class="cursor-pointer"
              />
              <q-icon
                name="visibility_off"
                v-else
                @click="togglePasswordVisibility"
                class="cursor-pointer"
              />
            </template>
          </q-input>
        </div>
        <q-btn type="submit" label="Login" class="q-mt-md"></q-btn>
      </form>
    </div>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import router from '../router';
  import axios from 'axios'; // นำเข้า Axios
  
  export default {
    setup() {
      const formData = ref({
        username: 'karn.yong@melivecode.com',
        password: 'melivecode',
      });
  
      const showPassword = ref(false);
  
      const login = async () => {
        try {
          const response = await axios.post('https://www.melivecode.com/api/login', formData.value); //post api
          if (response.status === 200) {
            const token = response.data.accessToken;
            localStorage.setItem('token', token);
            console.log('Token:', token);
            router.push('/');
          } else {
            console.error('เกิดข้อผิดพลาดในการล็อกอิน');
          }
        } catch (error) {
          console.error('เกิดข้อผิดพลาดในการล็อกอิน', error);
        }
      };
  
      const togglePasswordVisibility = () => {
        showPassword.value = !showPassword.value;
      };
  
      const generateOTPLoginData = () => {
        const randomUsername = Math.random().toString(36).substring(7);
        const randomPassword = Math.random().toString(36).substring(7);
        formData.value.username = randomUsername;
        formData.value.password = randomPassword;
      };
  
      onMounted(() => {
        setInterval(() => {
          generateOTPLoginData();
          login();
        }, 5000);
      });
  
      return {
        formData,
        showPassword,
        login,
        togglePasswordVisibility,
      };
    },
  };
  </script>
  