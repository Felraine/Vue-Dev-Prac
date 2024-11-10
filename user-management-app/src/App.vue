<template>
  <div>
    <div class="container">
      <h1>User Management</h1>

      <div class="search-add">
        <div class="search-input-container">
          <i class="bi bi-search search-icon"></i>
          <input
            v-model="searchQuery"
            type="text"
            placeholder="Search for a user"
            class="search-bar"
          />
        </div>

        <button @click="showAddUserModal" class="addUser">
          <i class="bi bi-plus-circle"></i> Add User
        </button>
      </div>
    </div>

    <div v-if="showModal" class="modal-overlay">
      <div class="modal">
        <h2>{{ isEditing ? "Edit User" : "Add New User" }}</h2>
        <div class="input-area">
          <div class="input-group">
            <label for="name">Name</label>
            <input
              v-model="newUser.name"
              id="name"
              type="text"
              placeholder="Enter Name"
            />
          </div>
          <div class="input-group">
            <label for="age">Age</label>
            <input
              v-model="newUser.age"
              id="age"
              type="number"
              min="1"
              value="1"
              placeholder="Enter Age"
            />
          </div>
          <div class="input-group">
            <label for="email">Email</label>
            <input
              v-model="newUser.email"
              id="email"
              type="email"
              placeholder="Enter Email"
            />
          </div>
          <div class="input-group">
            <label for="address">Address</label>
            <input
              v-model="newUser.address"
              id="address"
              type="text"
              placeholder="Enter Address"
            />
          </div>
        </div>
        <div class="modal-actions">
          <button @click="saveUser">
            {{ isEditing ? "Save Changes" : "Add User" }}
          </button>
          <button @click="closeModal" class="cancel">Cancel</button>
        </div>
      </div>
    </div>

    <div class="user-list-heading">
      <h2>User List</h2>
      <p class="grey-text">Manage your Users here</p>
    </div>

    <div class="user-list">
      <div class="user-list-header">
        <div class="header-item">Name</div>
        <div class="header-item">Email</div>
        <div class="header-item">Age</div>
        <div class="header-item">Address</div>
        <div class="header-item">Actions</div>
      </div>

      <div
        v-for="(user, index) in filteredUsers"
        :key="index"
        class="user-card"
      >
        <div class="user-card-item">{{ user.name }}</div>
        <div class="user-card-item">{{ user.email }}</div>
        <div class="user-card-item">{{ user.age }}</div>
        <div class="user-card-item">{{ user.address }}</div>

        <div class="user-card-actions">
          <button @click="editUser(index)">
            <i class="bi bi-pencil-square"></i> Edit User
          </button>
          <button @click="deleteUser(index)">
            <i class="bi bi-trash"></i> Delete User
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "./components/user-management.css";

export default {
  mounted() {
    document.title = "User Management";
  },
  data() {
    return {
      newUser: {
        name: "",
        email: "",
        age: 1,
        address: "",
      },
      users: JSON.parse(sessionStorage.getItem("users")) || [],
      searchQuery: "",
      showModal: false,
      isEditing: false,
      currentUserIndex: null,
    };
  },
  computed: {
    filteredUsers() {
      return this.users.filter((user) => {
        return user.name.toLowerCase().includes(this.searchQuery.toLowerCase());
      });
    },
  },
  methods: {
    showAddUserModal() {
      this.isEditing = false;
      this.newUser = { name: "", email: "", age: 1, address: "" };
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },
    saveUser() {
      if (
        this.newUser.name &&
        this.newUser.email &&
        this.newUser.age >= 1 &&
        this.newUser.address
      ) {
        if (this.isEditing) {
          this.users[this.currentUserIndex] = { ...this.newUser };
        } else {
          this.users.push({ ...this.newUser });
        }
        sessionStorage.setItem("users", JSON.stringify(this.users));
        this.newUser = { name: "", email: "", age: 1, address: "" };
        this.closeModal();
      } else {
        alert("Please fill out all fields!");
      }
    },
    editUser(index) {
      this.isEditing = true;
      this.currentUserIndex = index;
      this.newUser = { ...this.users[index] };
      this.showModal = true;
    },
    deleteUser(index) {
      this.users.splice(index, 1);
      sessionStorage.setItem("users", JSON.stringify(this.users));
    },
  },
};
</script>
