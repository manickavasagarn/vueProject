<template>
  <h5 class="fw-bold text-center my-2">Welcome to my first vue app</h5>
  <employee-from :editData="editData" @reload:employee="Reload" />
  <emplyoee-table :reloadData="reloadData" @edit:employee="handleEdit"/>
</template>

<script>
import EmployeeFrom from './components/EmployeeFrom.vue';
import EmplyoeeTable from "./components/EmplyoeeTable.vue";
export default {
  name: "app",
  components: {
    EmplyoeeTable,
    EmployeeFrom,
  },
  data() {
    return {
      reloadData:false,
      editData:{},
    };
  },
  methods:{
    Reload(){
      this.reloadData=!this.reloadData;
    },
    handleEdit(ele){
      this.editData = ele
    }
  }
};
</script>

<style>
</style>
