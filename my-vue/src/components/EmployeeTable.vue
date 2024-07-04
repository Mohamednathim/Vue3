<script>
export default{
    name:"EmployeeTable",
    props:{
        employees:Array
    },
    data(){
        return{
           editing:null,
           employee:{
            name:'',
            email:''
           },
           cachedEmployee:null
        }
    },
    methods:{
        editMode(employee){
            this.editing = employee.id
            this.cachedEmployee = Object.assign({},employee)
        },
        editEmployee(employee){
            if(employee.name == '' || employee.email == '') return;
            this.$emit("edit:employee",employee)
            this.editing=null
        },
        cancelEdit(employee){
            this.editing = null;
            Object.assign(employee,this.cachedEmployee)
        }
    }
}
</script>
<template>
    <div class="flex justify-center py-10">
    <p v-if="employees.length<1" class="text-center text-red-500 text-2xl font-semibold">No Employees</p>
    <table v-else class="w-3/4 text-sm text-center shadow-md">
        <thead class="border-2 border-bl bg-gray-200">
            <tr>
                <th class="px-6 py-2 border-b-2 border-r-2 border-black" >Employee Name</th>
                <th class="px-6 py-2 border-b-2 border-r-2 border-black">Employee Email</th>
                <th class="px-6 py-2 border-b-2 border-black">Action</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="employee in employees" :key="employee.id">
                <td v-if="editing===employee.id" class="px-6 py-2 border-r-2 border-b-2 border-black">
                    <input type="text" v-model="employee.name" class="w-50 border-2 border-gray-500 rounded-md text-center focus:border-gray-500 focus:border-2 focus:outline-none"/>
                </td>
                <td v-else class="px-6 py-2 border-b-2 border-r-2 border-black">{{ employee.name }}</td>

                <td v-if="editing===employee.id" class="px-6 py-2 border-r-2 border-b-2 border-black">
                    <input type="email" v-model="employee.email" class="w-50 border-2 border-gray-500 rounded-md text-center focus:border-gray-500 focus:border-2 focus:outline-none"/>
                </td>
                <td v-else class="px-6 py-2 border-b-2 border-r-2 border-black">{{ employee.email }}</td>
                <td v-if="editing===employee.id" class="px-6 py-2 border-b-2 border-black">
                    <button class="px-2 py-1  mx-2 rounded-md border-2 border-blue-500 text-blue-500 bg-white hover:bg-blue-500 hover:border-2 hover:text-white" @click="editEmployee(employee)">Save</button>
                    <button class="px-2 py-1  mx-2 rounded-md border-2 border-red-500 text-red-500 bg-white hover:bg-red-500 hover:border-2 hover:text-white" @click="cancelEdit(employee)">Cancel</button>
                </td>
                <td  v-else class="px-6 py-2 border-b-2 border-black">
                    <button class="px-2 py-1  mx-2 rounded-md border-2 border-yellow-500 text-yellow-500 bg-white hover:bg-yellow-500 hover:border-2 hover:text-white" @click="editMode(employee)">Edit</button>
                    <button class="px-2 py-1 mx-2 rounded-md border-2 border-green-500 text-green-500 bg-white hover:bg-green-500 hover:border-2 hover:text-white" @click="$emit('delete:employee',employee.id)">Delete</button>
                </td>
            </tr>
        </tbody>
    </table>
</div>
</template>
<style scoped>
</style>