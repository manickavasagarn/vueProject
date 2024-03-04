<template>
  <div class="mx-4">
    <table v-if="empData.length" class="table mt-4 w-100 table-bordered">
      <thead>
        <tr class="text-center">
          <th>Firstname</th>
          <th>Lastname</th>
          <th>Role</th>
          <th>Email</th>
          <th>Password</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="employee in empData" :key="employee.id">
          <td>{{ employee?.firstname }}</td>
          <td>{{ employee?.lastname }}</td>
          <td>{{ employee?.role }}</td>
          <td>{{ employee?.email }}</td>
          <td>{{ employee?.password }}</td>
          <td>
            <div class="d-flex justify-content-around">
              <button class="btn btn-info" @click="handleEdit(employee)">
                Edit
              </button>
              <button class="btn btn-danger" @click="handleDelete(employee)">
                Delete
              </button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-else class="text-center mt4">No rows Found</div>
  </div>
</template>

<script>
import axios from "axios";
import { toast } from "vue3-toastify";
import "vue3-toastify/dist/index.css";
export default {
  name: "employee-table",
  data() {
    return {
      empData: [],
    };
  },
  props: ["reloadData"],
  watch: {
    reloadData() {
      this.getEmpData();
    },
  },
  methods: {
    async getEmpData() {
      try {
        let data = await axios.get(
          "https://65e6071ed7f0758a76e7fcba.mockapi.io/vue/employee"
        );
        let tempdata= (data.data).sort((a,b)=> b.id-a.id);
        this.empData = tempdata;
      } catch (error) {
        console.log(error);
      }
    },
    handleEdit(ele) {
      this.$emit("edit:employee", ele);
    },
    async handleDelete(ele) {
      try {
        await axios.delete(
          `https://65e6071ed7f0758a76e7fcba.mockapi.io/vue/employee/${ele.id}`
        );
        toast.success("User Deleted Successfully !", {
          position: "top-center",
          autoClose: 3000,
        });
        this.getEmpData();
      } catch (error) {
        console.log(error);
      }
    },
  },
  mounted() {
    this.getEmpData();
  },
};
</script>

