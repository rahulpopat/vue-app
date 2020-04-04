<template>
  <div id="employee-table">
    <p v-if="employeesList.length < 1" class="empty-table">
      No employees
    </p>
    <table v-else>
      <thead>
        <tr>
          <th>Employee name</th>
          <th>Employee email</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="employee in employeesList" :key="employee.id">
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{employee.name}}</td>
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.email" />
          </td>
          <td v-else>{{employee.email}}</td>
          <td v-if="editing === employee.id">
            <button @click="editEmployeeLocal(employee)">Save</button>
            <!-- <button class="muted-button" @click="editing = null" >Cancel</button> -->
            <button @click="cancelEdit(employee)" >Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(employee)">Edit</button>
            <button @click="$emit('delete:employee', employee.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  export default {
    name: 'employee-table',
    props: {
        employeesList: Array,
    },
    data() {
      return {
        editing: null,
      }
    },
    methods: {
      editMode(employee) {
        console.log('start editing ' + employee)
        this.cachedEmployee = Object.assign({}, employee)
        this.editing = employee.id
      },
      cancelEdit(employee) {
        console.log('cancel edit ' + employee)
        Object.assign(employee, this.cachedEmployee)
        this.editing = null;
      },
      editEmployeeLocal(employee) {
        console.log('editEmployeeLocal ' + employee)
        if (employee.name === '' || employee.email === '') return
        this.$emit('edit:employee', employee.id, employee)
        this.editing = null
      },
    }
  }
</script>

<style scoped>
  button {
    margin: 0 0.5rem 0 0;
  }
</style>

