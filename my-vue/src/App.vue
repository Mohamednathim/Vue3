<script>
import RouterView from 'vue-router'
import EmployeeTable from "./components/EmployeeTable.vue"
import EmployeeForm from "./components/EmployeeForm.vue"
import Header from "./components/Header.vue"
export default {
  name: "App",
  components: {
    EmployeeTable,
    EmployeeForm,
    Header

  },
  data() {
    return {
      employees: []
    }
  },
  methods: {
    async addEmployee(employee) {
      try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users", {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(employee)
        })
        const data = await response.json()

        // const lastId = this.employees.length > 0 ? this.employees[this.employees.length - 1].id : 0
        // const id = lastId + 1
        // const newEmployee = { ...employee, id }
        // this.employees = [...this.employees, newEmployee]

        this.employees = [...this.employees, data]
      } catch (error) {
        console.error(error)
      }
    },
    async deleteEmployee(id) {
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`,{
        method:'DELETE'
      })

      this.employees = this.employees.filter((employee) => {
        return employee.id !== id
      })
      } catch (error) {
        console.error(error)
      }
    },
    async editEmployee(employee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${employee.id}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(employee)
        }
        )

        const data = await response.json()

        this.employees.map((emp) => {
          return emp.id == employee.id ? data : emp;
        })

      } catch (err) {
        console.error(err)
      }


    },
    async getEmployees() {
      try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users")
        const data = await response.json()
        this.employees = data
      } catch (err) {
        console.error(err)
      }

    }
  },
  mounted() {
    this.getEmployees()
  }
}
</script>

<template>
  <RouterView/>
  <!-- <h1 class="text-3xl font-bold text-center pt-5">Employees</h1>
  <EmployeeForm @add:employee="addEmployee" />
  <EmployeeTable v-bind:employees="employees" @delete:employee="deleteEmployee" @edit:employee="editEmployee" /> -->

</template>

<style scoped></style>
