<template>
  <!-- src/components/EditUserModal.vue -->
  <div class="modal" v-if="showModal">
    <div class="modal-content">
      <span class="close" @click="closeModal">&times;</span>
      <h2>Edit User</h2>
      <form @submit.prevent="updateUser">
        <label for="editName">Name:</label>
        <input
          type="text"
          v-model="editedUserData.name"
          id="editName"
          required
        />

        <label for="editAge">Age:</label>
        <input
          type="number"
          v-model="editedUserData.age"
          id="editAge"
          required
        />

        <label for="editCountry">Country:</label>
        <input
          type="text"
          v-model="editedUserData.country"
          id="editCountry"
          required
        />

        <label for="editOccupation">Occupation:</label>
        <input
          type="text"
          v-model="editedUserData.occupation"
          id="editOccupation"
          required
        />

        <button type="submit">Update</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    showModal: Boolean,
    editedUser: Object,
  },
  data() {
    return {
      editedUserData: {
        name: "",
        age: 0,
        country: "",
        occupation: "",
      },
    };
  },
  watch: {
    editedUser: {
      immediate: true, // Run the handler immediately on component creation
      handler(newVal) {
        // Update the local data when the prop changes
        this.editedUserData = { ...newVal };
      },
    },
  },
  methods: {
    closeModal() {
      this.$emit("close-modal");
    },
    updateUser() {
      console.log("Updating user:", this.editedUserData);
      this.$emit("update-user", this.editedUserData);
      this.closeModal();
    },
  },
};
</script>

<style scoped>
.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

form {
  display: flex;
  flex-direction: column;
}

label {
  margin-top: 10px;
}

input {
  margin-bottom: 10px;
}

button {
  padding: 10px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
</style>
