<template>
  <div class="card m-3 p-3">
    <h5 class="fw-bold" v-if="empData?.id">Update Employee</h5>
    <h5 class="fw-bold" v-else>New Employee</h5>
    <div class="row">
      <div class="col-4">
        <label>First Name</label>
        <input
          type="text"
          class="form-control"
          v-model="empData.firstname"
          @input="clearErrorMsg"
        />
      </div>
      <div class="col-4">
        <label>Last Name</label>
        <input
          type="text"
          class="form-control"
          v-model="empData.lastname"
          @input="clearErrorMsg"
        />
      </div>
      <div class="col-4">
        <label>Role</label>
        <input
          type="text"
          class="form-control"
          v-model="empData.role"
          @input="clearErrorMsg"
        />
      </div>
      <div class="col-4 mt-2">
        <label>Email</label>
        <input
          type="email"
          class="form-control"
          v-model="empData.email"
          @input="clearErrorMsg"
        />
      </div>
      <div class="col-4 mt-2">
        <label>Password</label>
        <input
          type="text"
          class="form-control"
          v-model="empData.password"
          @input="clearErrorMsg"
        />
      </div>
    </div>
    <div v-if="errorMsg != ''" class="text-center text-danger mt-2">
      {{ errorMsg }}
    </div>
    <div class="d-flex justify-content-end">
      <button @click="handleSubmit" class="btn btn-info me-3">Save</button>
      <button @click="reset" class="btn btn-info">Reset</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
export default {
  name: "employee-from",
  data() {
    return {
      empData: {
        firstname: "",
        lastname: "",
        role: "",
        email: "",
        password: "",
      },
      errorMsg: "",
    };
  },
  props: ["editData"],
  watch: {
    editData(ele) {
      if (Object.keys(ele).length) {
        let tempdata = { ...ele };
        this.empData = tempdata;
        this.errorMsg = "";
      }
    },
  },
  methods: {
    async handleSubmit() {
      let ifEmpty = Object.keys(this.empData)?.filter((ele) => {
        if (this.empData[ele] == "") {
          return ele;
        }
      });
      if (ifEmpty.length) {
        this.errorMsg = "Please fill the required fields";
        return 0;
      }
      try {
        if (this.empData.id) {
          let sendData = { ...this.empData };
          delete sendData.id;
          await axios.put(
            `https://65e6071ed7f0758a76e7fcba.mockapi.io/vue/employee/${this.empData.id}`,
            sendData
          );
          toast.success("User Update Successfully !", {
            position: "top-center",
            autoClose: 3000,
          });
        } else {
          await axios.post(
            "https://65e6071ed7f0758a76e7fcba.mockapi.io/vue/employee",
            this.empData
          );
          toast.success("New User Created Successfully !", {
            position: "top-center",
            autoClose: 3000,
          });
        }

        this.$emit("reload:employee");
        this.empData = {
          firstname: "",
          lastname: "",
          role: "",
          email: "",
          password: "",
        };
      } catch (error) {
        console.log(error);
      }
    },
    reset() {
      this.empData = {};
      this.errorMsg = "";
    },
    clearErrorMsg() {
      this.errorMsg = "";
    },
  },
};
</script>