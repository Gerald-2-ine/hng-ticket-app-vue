<template>
  <div class="dashboard-container">
    <header class="dashboard-header">
      <h1>🎟️ Ticket Management Dashboard</h1>
      <button @click="handleLogout" class="logout-btn">Logout</button>
    </header>

    <section class="stats-section">
      <div class="stat-card open">
        <h3>Open Tickets</h3>
        <span class="badge open">Open</span>
        <p>{{ openTickets }}</p>
      </div>

      <div class="stat-card in-progress">
        <h3>In Progress</h3>
        <span class="badge in-progress">In Progress</span>
        <p>{{ inProgressTickets }}</p>
      </div>

      <div class="stat-card closed">
        <h3>Closed Tickets</h3>
        <span class="badge closed">Closed</span>
        <p>{{ closedTickets }}</p>
      </div>

      <div class="stat-card total">
        <h3>Total Tickets</h3>
        <p>{{ totalTickets }}</p>
      </div>
    </section>

    <section class="nav-links">
      <router-link to="/tickets" class="btn-primary">Manage Tickets</router-link>
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const tickets = ref([]);

const loadTickets = () => {
  const storedTickets = JSON.parse(localStorage.getItem("ticketapp_tickets")) || [];
  tickets.value = storedTickets;
};

const handleTicketUpdate = () => loadTickets();

onMounted(() => {
  // Check if logged in
  const session = JSON.parse(localStorage.getItem("ticketapp_session"));
  if (!session) router.push("/login");

  // Load tickets
  loadTickets();

  // Listen for ticket updates
  window.addEventListener("storage", loadTickets);
  window.addEventListener("ticketsUpdated", handleTicketUpdate);
});

onUnmounted(() => {
  window.removeEventListener("storage", loadTickets);
  window.removeEventListener("ticketsUpdated", handleTicketUpdate);
});

const handleLogout = () => {
  localStorage.removeItem("ticketapp_session");
  router.push("/login");
};

const totalTickets = computed(() => tickets.value.length);
const openTickets = computed(() => tickets.value.filter(t => t.status === "open").length);
const inProgressTickets = computed(() => tickets.value.filter(t => t.status === "in-progress").length);
const closedTickets = computed(() => tickets.value.filter(t => t.status === "closed").length);
</script>


<style scoped>
@import "../styles/Dashboard.css";
</style>
