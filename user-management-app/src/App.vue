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
        <h2>Add New User</h2>
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
          <button @click="addUser">Add User</button>
          <button @click="closeModal" class="cancel">Cancel</button>
        </div>
      </div>
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
export default {
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
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },
    addUser() {
      if (
        this.newUser.name &&
        this.newUser.email &&
        this.newUser.age >= 1 &&
        this.newUser.address
      ) {
        this.users.push({ ...this.newUser });
        sessionStorage.setItem("users", JSON.stringify(this.users));
        this.newUser = { name: "", email: "", age: 1, address: "" };
        this.closeModal();
      } else {
        alert("Please fill out all fields!");
      }
    },
    editUser(index) {
      const user = this.users[index];
      this.newUser = { ...user };
      this.deleteUser(index);
    },
    deleteUser(index) {
      this.users.splice(index, 1);
      sessionStorage.setItem("users", JSON.stringify(this.users));
    },
  },
};
</script>

<style scoped>
body {
  margin: 0;
  padding: 0;
}

.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  border-bottom: 1px solid lightgrey;
  padding-bottom: 1rem;
  padding-top: 8px;
}

h1 {
  margin: 0;
}

.search-add {
  display: flex;
  align-items: center;
  gap: 15px;
}

.search-input-container {
  position: relative;
}

.search-bar {
  padding: 8px 8px 8px 40px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 20px;
  width: 200px;
}

.search-icon {
  position: absolute;
  top: 50%;
  left: 10px;
  transform: translateY(-50%);
  font-size: 1.2rem;
  color: grey;
}

button {
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 10px 15px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.addUser {
  margin-right: 25px;
}

button:hover {
  background-color: #0056b3;
}

.input-area {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
  flex-wrap: wrap;
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.input-area input {
  padding: 8px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.input-area .input-group:nth-child(1) {
  margin-right: 1rem;
}

.input-area .input-group:nth-child(2) {
  margin-left: 13px;
}

.input-area .input-group:nth-child(3) {
  margin-right: 1rem;
}

.input-area .input-group:nth-child(4) {
  margin-left: 13px;
}

.user-card {
  background-color: #f9f9f9;
  padding: 15px;
  margin: 10px 0;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.user-card h4 {
  margin: 0;
  color: #333;
}

.user-card p {
  color: #555;
}

.user-list {
  margin-top: 20px;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: #f8f8f8;
  padding: 20px;
  border-radius: 10px;
  width: 466px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.modal h2 {
  margin-top: 0;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 20px;
}

.modal-actions button {
  padding: 8px 15px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.modal-actions button:hover {
  background-color: #0056b3;
}

.user-list-header {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  background-color: #e6e6e6;
  padding: 10px;
  font-weight: bold;
  border-bottom: 2px solid #ccc;
  font-size: 20px;
}

.header-item {
  text-align: left;
  padding: 5px;
}

.user-card {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  background-color: #f2f2f2;
  padding: 15px;
  margin: 10px 0;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.user-card-item {
  padding: 5px;
}

.user-card-actions {
  display: flex;
  gap: 10px;
}

.user-card-actions button {
  padding: 5px 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.user-card-actions button:hover {
  background-color: #0056b3;
}

.user-card-item:nth-child(2) {
  color: #717171;
}
</style>
