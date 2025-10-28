<template>
  <div class="signup-container">
    <div v-if="success" class="alert success">{{ success }}</div>
    <div v-if="error" class="alert error">{{ error }}</div>

    <div class="signup-card">
      <h2>Create an Account ✨</h2>
      <p>Join the ticket management system today</p>

      <form @submit.prevent="handleSubmit">
        <div class="form-group">
          <label>Full Name</label>
          <input
            type="text"
            name="name"
            placeholder="Enter your name"
            v-model="formData.name"
            required
          />
        </div>

        <div class="form-group">
          <label>Email</label>
          <input
            type="email"
            name="email"
            placeholder="Enter your email"
            v-model="formData.email"
            required
          />
        </div>

        <div class="form-group">
          <label>Password</label>
          <input
            type="password"
            name="password"
            placeholder="Enter password"
            v-model="formData.password"
            required
          />
        </div>

        <div class="form-group">
          <label>Confirm Password</label>
          <input
            type="password"
            name="confirmPassword"
            placeholder="Confirm password"
            v-model="formData.confirmPassword"
            required
          />
        </div>

        <button type="submit" class="btn-primary">Sign Up</button>

     <p class="switch-text">
     Already have an account?
     <router-link to="/login">Login</router-link>
    </p>

      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const formData = ref({
  name: "",
  email: "",
  password: "",
  confirmPassword: "",
});

const error = ref("");
const success = ref("");

const handleSubmit = () => {
  error.value = "";
  success.value = "";

  const { name, email, password, confirmPassword } = formData.value;

  if (!name || !email || !password || !confirmPassword) {
    error.value = "❌ All fields are required.";
    setTimeout(() => (error.value = ""), 2000);
    return;
  }

  if (password.length < 6) {
    error.value = "❌ Password must be at least 6 characters long.";
    setTimeout(() => (error.value = ""), 2000);
    return;
  }

  if (password !== confirmPassword) {
    error.value = "❌ Passwords do not match.";
    setTimeout(() => (error.value = ""), 2000);
    return;
  }

  const storedUsers = JSON.parse(localStorage.getItem("ticketapp_users")) || [];
  const userExists = storedUsers.some((user) => user.email === email);

  if (userExists) {
    error.value = "⚠️ This email is already registered. Please log in.";
    setTimeout(() => (error.value = ""), 2000);
    return;
  }

  const newUser = { name, email, password };
  storedUsers.push(newUser);
  localStorage.setItem("ticketapp_users", JSON.stringify(storedUsers));

  success.value = "✅ Signup successful! Redirecting to login...";
  setTimeout(() => {
    success.value = "";
    router.push("/login");
  }, 1500);
};
</script>

<style scoped>
@import "../styles/auth.css";
</style>
