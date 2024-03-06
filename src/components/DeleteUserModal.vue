<template>
  <div class="modal" v-if="showModal">
    <div class="modal-content">
      <h2>Delete User</h2>
      <p>Do you want to delete the entry?</p>
      <div class="button-container">
        <button class="delete-button" @click="deleteConfirmedUser">Yes</button>
        <button class="cancel-button" @click="closeModal">No</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    showModal: Boolean,
    userToDelete: Object,
  },
  methods: {
    closeModal() {
      this.$emit("close-modal");
    },
    deleteConfirmedUser() {
      this.$emit("delete-user", this.userToDelete);
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
  padding: 20px;
  border: 1px solid #888;
  border-radius: 5px;
  text-align: center;
}

h2 {
  margin-bottom: 15px;
}

p {
  margin-bottom: 20px;
}

.button-container {
  display: flex;
  justify-content: center;
}

.delete-button,
.cancel-button {
  padding: 10px;
  margin: 0 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.delete-button {
  background-color: #d9534f;
  color: white;
}

.delete-button:hover {
  background-color: #c9302c;
}

.cancel-button {
  background-color: #5bc0de;
  color: white;
}

.cancel-button:hover {
  background-color: #46b8da;
}
</style>
