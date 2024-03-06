<!-- src/components/AddUserModal.vue -->
<template>
  <div class="modal" v-if="showModal">
    <div class="modal-content">
      <span class="close" @click="closeModal">&times;</span>
      <h2>Add User</h2>
      <form @submit.prevent="addUser">
        <label for="addName">Name:</label>
        <input type="text" v-model="newUser.name" id="addName" required />

        <label for="addAge">Age:</label>
        <input type="number" v-model="newUser.age" id="addAge" required />

        <label for="addCountry">Country:</label>
        <input type="text" v-model="newUser.country" id="addCountry" required />

        <label for="addOccupation">Occupation:</label>
        <input
          type="text"
          v-model="newUser.occupation"
          id="addOccupation"
          required
        />

        <button type="submit">Add</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    showModal: Boolean,
  },
  data() {
    return {
      newUser: {
        name: "",
        age: 0,
        country: "",
        occupation: "",
      },
    };
  },
  methods: {
    closeModal() {
      this.$emit("close-modal");
    },
    addUser() {
      this.$emit("add-user", { ...this.newUser });

      this.newUser = {
        name: "",
        age: 0,
        country: "",
        occupation: "",
      };

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
  display: flex;
  align-items: center;
  justify-content: center;
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
  background-color: #5bc0de;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #46b8da;
}
</style>
