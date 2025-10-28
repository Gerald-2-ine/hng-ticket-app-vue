<template>
  <div class="login-container">
    <!-- Alerts -->
    <div v-if="success" class="alert success">{{ success }}</div>
    <div v-if="error" class="alert error">{{ error }}</div>

    <div class="login-card">
      <h2>Welcome Back 👋</h2>
      <p>Login to continue managing your tickets</p>

      <form @submit.prevent="handleSubmit">
        <div class="form-group">
          <label>Email</label>
          <input
            type="email"
            v-model="formData.email"
            placeholder="Enter your email"
            required
          />
        </div>

        <div class="form-group">
          <label>Password</label>
          <input
            type="password"
            v-model="formData.password"
            placeholder="Enter your password"
            required
          />
        </div>

        <button type="submit" class="btn-primary">Login</button>
      </form>

      <p class="switch-text">
        Don’t have an account?
        <router-link to="/signup">Sign up</router-link>
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

// Form state
const formData = ref({ email: "", password: "" });
const error = ref("");
const success = ref("");

// Redirect to dashboard if already logged in
onMounted(() => {
  const session = JSON.parse(localStorage.getItem("ticketapp_session"));
  if (session) router.push("/dashboard");
});

const handleSubmit = () => {
  error.value = "";
  success.value = "";

  const storedUsers = JSON.parse(localStorage.getItem("ticketapp_users")) || [];
  const validUser = storedUsers.find(
    (user) =>
      user.email === formData.value.email &&
      user.password === formData.value.password
  );

  if (validUser) {
    // Save session and redirect
    localStorage.setItem(
      "ticketapp_session",
      JSON.stringify({ email: validUser.email })
    );
    success.value = "✅ Login successful! Redirecting...";

    setTimeout(() => {
      success.value = "";
      router.push("/dashboard");
    }, 1000);
  } else {
    error.value = "❌ Invalid email or password.";
    setTimeout(() => (error.value = ""), 2000);
  }
};
</script>

<style scoped>
@import "../styles/auth.css";
</style>
