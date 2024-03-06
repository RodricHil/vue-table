<template>
  <div>
    <h1>User Data Table</h1>

    <button @click="showAddUserModal" class="add">Add User</button>

    <AddUserModal
      :showModal="showAddModal"
      @close-modal="closeAddUserModal"
      @add-user="addUser"
    />

    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Age</th>
          <th>Country</th>
          <th>Occupation</th>
          <th>Actions</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="user in paginatedUsers" :key="user.id">
          <td>{{ user.name }}</td>
          <td>{{ user.age }}</td>
          <td>{{ user.country }}</td>
          <td>{{ user.occupation }}</td>
          <td>
            <button @click="editUser(user)">Edit</button>
            <button @click="deleteUser(user)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>

    <EditUserModal
      :showModal="showEditModal"
      :editedUser="editedUser"
      @close-modal="closeEditModal"
      @update-user="updateUser"
    />

    <DeleteUserModal
      :showModal="showDeleteModal"
      :userToDelete="userToDelete"
      @close-modal="closeDeleteModal"
      @delete-user="deleteConfirmedUser"
    />

    <PaginationControls
      :currentPage="currentPage"
      :totalPages="totalPages"
      @page-change="changePage"
    />
  </div>
</template>

<script>
import axios from "axios";
import EditUserModal from "./EditUserModal.vue";
import DeleteUserModal from "./DeleteUserModal.vue";
import AddUserModal from "./AddUserModal.vue";
import PaginationControls from "./PaginationControls.vue";

export default {
  data() {
    return {
      users: [],
      showEditModal: false,
      editedUser: null,
      showDeleteModal: false,
      userToDelete: null,
      showAddModal: false,
      currentPage: 1,
      itemsPerPage: 5,
    };
  },
  computed: {
    totalPages() {
      return Math.ceil(this.users.length / this.itemsPerPage);
    },
    paginatedUsers() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.users.slice(start, end);
    },
  },
  mounted() {
    this.fetchUserData();
  },
  methods: {
    fetchUserData() {
      const apiUrl = "https://jsonplaceholder.typicode.com/users";

      axios
        .get(apiUrl)
        .then((response) => {
          this.users = response.data.map((user) => ({
            id: user.id,
            name: user.name,
            age: Math.floor(Math.random() * 30) + 20,
            country: user.address.country,
            occupation: "Developer",
          }));
        })
        .catch((error) => {
          console.error("Error fetching user data:", error);
        });
    },
    changePage(newPage) {
      this.currentPage = newPage;
    },
    editUser(user) {
      this.editedUser = { ...user };
      this.showEditModal = true;
    },
    updateUser(updatedUser) {
      const index = this.users.findIndex((user) => user.id === updatedUser.id);
      if (index !== -1) {
        this.users[index] = updatedUser;
        this.editedUser = { ...updatedUser };
      }
    },
    closeEditModal() {
      this.showEditModal = false;
    },
    deleteUser(user) {
      this.userToDelete = user;
      this.showDeleteModal = true;
    },
    deleteConfirmedUser() {
      const index = this.users.findIndex(
        (user) => user.id === this.userToDelete.id
      );
      if (index !== -1) {
        this.users.splice(index, 1);
      }
      this.closeDeleteModal();
    },
    closeDeleteModal() {
      this.showDeleteModal = false;
      this.userToDelete = null;
    },
    showAddUserModal() {
      this.showAddModal = true;
    },

    closeAddUserModal() {
      this.showAddModal = false;
    },

    addUser(newUser) {
      this.users.push({
        id: Math.max(...this.users.map((user) => user.id), 0) + 1,
        ...newUser,
      });
      this.closeAddUserModal();
    },
  },
  components: {
    EditUserModal,
    DeleteUserModal,
    AddUserModal,
    PaginationControls,
  },
};
</script>

<style scoped>
h1 {
  text-align: center;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th,
td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

.add {
  text-align: right;
}
</style>
