<template>
  <div class="container mt-5">
    <h1 class="text-center mb-4">Employee Management System</h1>

    <!-- Employee Form -->
    <div class="card mb-4">
      <div class="card-header">
        <h5 class="mb-0">{{ isEditing ? 'Edit Employee' : 'Add Employee' }}</h5>
      </div>
      <div class="card-body">
        <form @submit.prevent="saveEmployee">
          <div class="row mb-3">
            <div class="col-md-6">
              <label for="name" class="form-label">Name</label>
              <input type="text" id="name" class="form-control" v-model="currentEmployee.name" required>
            </div>
            <div class="col-md-6">
              <label for="designation" class="form-label">Designation</label>
              <input type="text" id="designation" class="form-control" v-model="currentEmployee.designation" required>
            </div>
          </div>
          <div class="row mb-3">
            <div class="col-md-6">
              <label for="department" class="form-label">Department</label>
              <input type="text" id="department" class="form-control" v-model="currentEmployee.department" required>
            </div>
            <div class="col-md-6">
              <label for="salary" class="form-label">Salary</label>
              <input type="number" id="salary" class="form-control" v-model="currentEmployee.salary" required>
            </div>
          </div>
          <button type="submit" class="btn btn-primary">{{ isEditing ? 'Update' : 'Add' }} Employee</button>
          <button type="button" class="btn btn-secondary ms-2" v-if="isEditing" @click="cancelEdit">Cancel</button>
        </form>
      </div>
    </div>

    <!-- Employee Table -->
    <div class="card">
      <div class="card-header">
        <h5 class="mb-0">Employee List</h5>
      </div>
      <div class="card-body">
        <table class="table table-striped table-hover">
          <thead>
            <tr>
              <th>ID</th>
              <th>Name</th>
              <th>Designation</th>
              <th>Department</th>
              <th>Salary</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="emp in employees" :key="emp.id">
              <td>{{ emp.id }}</td>
              <td>{{ emp.name }}</td>
              <td>{{ emp.designation }}</td>
              <td>{{ emp.department }}</td>
              <td>${{ emp.salary }}</td>
              <td>
                <button class="btn btn-sm btn-warning me-2" @click="editEmployee(emp)">Edit</button>
                <button class="btn btn-sm btn-danger" @click="deleteEmployee(emp.id)">Delete</button>
              </td>
            </tr>
            <tr v-if="employees.length === 0">
              <td colspan="6" class="text-center">No employees found.</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

// The MockAPI endpoint provided by the user
const API_URL = 'https://69f9ab42c509a40d3aa2fb21.mockapi.io/employees';

export default {
  name: 'EmployeeComponent',
  data() {
    return {
      employees: [],
      currentEmployee: {
        id: null,
        name: '',
        designation: '',
        department: '',
        salary: ''
      },
      isEditing: false
    };
  },
  methods: {
    async fetchEmployees() {
      try {
        const response = await axios.get(API_URL);
        this.employees = response.data;
      } catch (error) {
        console.error('Error fetching employees:', error);
      }
    },
    async saveEmployee() {
      try {
        if (this.isEditing) {
          await axios.put(`${API_URL}/${this.currentEmployee.id}`, this.currentEmployee);
        } else {
          await axios.post(API_URL, this.currentEmployee);
        }
        this.fetchEmployees();
        this.resetForm();
      } catch (error) {
        console.error('Error saving employee:', error);
      }
    },
    async deleteEmployee(id) {
      if (confirm('Are you sure you want to delete this employee?')) {
        try {
          await axios.delete(`${API_URL}/${id}`);
          this.fetchEmployees();
        } catch (error) {
          console.error('Error deleting employee:', error);
        }
      }
    },
    editEmployee(employee) {
      this.isEditing = true;
      this.currentEmployee = { ...employee };
    },
    cancelEdit() {
      this.resetForm();
    },
    resetForm() {
      this.isEditing = false;
      this.currentEmployee = {
        id: null,
        name: '',
        designation: '',
        department: '',
        salary: ''
      };
    }
  },
  mounted() {
    this.fetchEmployees();
  }
};
</script>

<style scoped>
/* Scoped styles for the Employee component */
</style>
