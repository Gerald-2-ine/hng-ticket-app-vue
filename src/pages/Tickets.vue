<template>
  <div class="tickets-container">
     <RouterLink to="/dashboard" class="btn-back-dashboard">← Back</RouterLink>
    <h2>🎫 Ticket Management</h2>
    <button class="add-ticket-btn" @click="showModal = true">
      + Add New Ticket
    </button>

    <div class="ticket-list">
      <p v-if="tickets.length === 0">No tickets yet. Add one above!</p>
      <div v-else v-for="ticket in tickets" :key="ticket.id" class="ticket-card">
        <h3>{{ ticket.title }}</h3>
        <p>{{ ticket.description }}</p>
        <p :class="['status', ticket.status]">{{ ticket.status }}</p>

        <div class="ticket-actions">
          <select v-model="ticket.status" @change="updateStatus(ticket.id, ticket.status)">
            <option value="open">Open</option>
            <option value="in-progress">In Progress</option>
            <option value="closed">Closed</option>
          </select>

          <div class="button-group">
            <button class="edit-btn" @click="editTicket(ticket)">Edit</button>
            <button class="delete-btn" @click="deleteTicket(ticket.id)">Delete</button>
          </div>
        </div>
      </div>
    </div>


    <!-- ✅ Modal Section -->
    <div v-if="showModal" class="modal-overlay" @click="closeModal">
      <div class="modal-content" @click.stop>
        <h3>{{ editingTicket ? "Edit Ticket" : "Create New Ticket" }}</h3>
        <form @submit.prevent="editingTicket ? updateTicket() : addTicket()" class="modal-form">
          <input type="text" placeholder="Ticket Title" v-model="modalTicket.title" />
          <textarea placeholder="Ticket Description" v-model="modalTicket.description"></textarea>


          <div class="modal-buttons">
            <button type="submit" class="save-btn">
              {{ editingTicket ? "Update" : "Add Ticket" }}
            </button>
            <button type="button" class="cancel-btn" @click="closeModal">Cancel</button>
          </div>
        </form>
      </div>
    </div>

    <!-- ✅ Toast Notification -->
    <Toast v-if="toast.message" :message="toast.message" :type="toast.type" @close="toast.message = ''" />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import Toast from "../components/Toast.vue";

const tickets = ref([]);
const showModal = ref(false);
const editingTicket = ref(null);

// This object will hold modal form inputs
const modalTicket = ref({ title: "", description: "" });

const toast = ref({ message: "", type: "" });

const showToast = (message, type = "info") => {
  toast.value = { message, type };
  setTimeout(() => (toast.value = { message: "", type: "" }), 3000);
};

onMounted(() => {
  const saved = JSON.parse(localStorage.getItem("ticketapp_tickets")) || [];
  tickets.value = saved;
});

const saveTickets = (updated) => {
  localStorage.setItem("ticketapp_tickets", JSON.stringify(updated));
  window.dispatchEvent(new Event("ticketsUpdated"));
};

const addTicket = () => {
  if (!modalTicket.value.title.trim() || !modalTicket.value.description.trim()) {
    showToast("Please fill in all fields", "error");
    return;
  }

  const newEntry = {
    id: Date.now(),
    ...modalTicket.value,
    status: "open",
  };

  tickets.value = [newEntry, ...tickets.value];
  saveTickets(tickets.value);

  // Reset modal
  modalTicket.value = { title: "", description: "" };
  showModal.value = false;
  showToast("Ticket added successfully!", "success");
};

const editTicket = (ticket) => {
  editingTicket.value = { ...ticket };
  modalTicket.value = { ...ticket }; // populate modal inputs
  showModal.value = true;
};

const updateTicket = () => {
  tickets.value = tickets.value.map((t) =>
    t.id === editingTicket.value.id ? { ...editingTicket.value, ...modalTicket.value } : t
  );
  saveTickets(tickets.value);
  editingTicket.value = null;
  modalTicket.value = { title: "", description: "" };
  showModal.value = false;
  showToast("Ticket updated successfully!", "success");
};

const closeModal = () => {
  editingTicket.value = null;
  modalTicket.value = { title: "", description: "" };
  showModal.value = false;
};
</script>


<style scoped>
@import "@/styles/Tickets.css";
</style>
