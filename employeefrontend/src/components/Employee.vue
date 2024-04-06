<template>
  <div>
    <h2>Employee Registation</h2>
    <form @submit.prevent="save">
      <div class="form-group">
        <label>Employee name</label>
        <input
          type="text"
          v-model="employee.name"
          class="form-control"
          placeholder="Employee name"
        />
      </div>

      <div class="form-group">
        <label>Employee Address</label>
        <input
          type="text"
          v-model="employee.address"
          class="form-control"
          placeholder="Employee Address"
        />
      </div>

      <div class="form-group">
        <label>Mobile</label>
        <input
          type="text"
          v-model="employee.mobile"
          class="form-control"
          placeholder="Mobile"
        />
      </div>

      <button type="submit" class="btn btn-primary">Save</button>
    </form>

    <h2>Employee View</h2>
    <table class="table table-dark">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Employee Name</th>
          <th scope="col">Address</th>
          <th scope="col">Mobile</th>
          <th scope="col">Option</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="employee in result" v-bind:key="employee.id">
          <td>{{ employee.id }}</td>
          <td>{{ employee.name }}</td>
          <td>{{ employee.address }}</td>
          <td>{{ employee.mobile }}</td>
          <td>
            <button
              type="button"
              class="btn btn-warning"
              @click="edit(employee)"
            >
              Edit
            </button>
            <button
              type="button"
              class="btn btn-danger"
              @click="remove(employee)"
            >
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";

export default {
  name: "Employee",
  data() {
    return {
      result: {},
      employee: {
        id: "",
        name: "",
        address: "",
        mobile: "",
      },
    };
  },
  created() {
    this.EmployeeLoad();
  },
  methods: {
    EmployeeLoad() {
      axios.get("http://127.0.0.1:8000/api/employees").then(({ data }) => {
        console.log(data);
        this.result = data;
      });
    },
    save() {
      if (this.employee.id == "") {
        this.saveData();
      } else {
        this.updateData();
      }
    },
    saveData() {
      axios
        .post("http://127.0.0.1:8000/api/save", this.employee)
        .then(({ data }) => {
          alert("Saved");
          this.EmployeeLoad();
          this.employee = {
            id: "",
            name: "",
            address: "",
            mobile: "",
          };
        });
    },
    edit(employee) {
      this.employee = { ...employee }; // Use spread operator to avoid mutating original object
    },
    updateData() {
      axios
        .put(
          `http://127.0.0.1:8000/api/update/${this.employee.id}`,
          this.employee
        )
        .then(({ data }) => {
          alert("Updated");
          this.EmployeeLoad();
          this.employee = {
            id: "",
            name: "",
            address: "",
            mobile: "",
          };
        });
    },
    remove(employee) {
      axios
        .delete(`http://127.0.0.1:8000/api/delete/${employee.id}`)
        .then(() => {
          alert("Deleted");
          this.EmployeeLoad();
        });
    },
  },
};
</script>
