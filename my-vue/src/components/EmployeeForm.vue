<script>
export default {
    name: 'EmployeeForm',
    data() {
        return {
            submitting: false,
            success: false,
            error: false,
            employee: {
                name: '',
                email: ''
            }
        }
    },
    methods: {
        handleSubmit() {
            this.submitting = true

            this.clearStatus()

            if (this.invalidEmail || this.invalidName) {
                this.error = true
                return
            }

            this.$emit('add:employee', this.employee)

            this.$refs.firstInput.focus()
            
            this.employee={
                name: '',
                email: ''
            }
              
            this.success=true;
            this.error = false;
            this.submitting= false;
        },
        clearStatus() {
            this.success = false,
            this.error = false
        }
    },
    computed: {
        invalidName() {
            return this.employee.name === ""
        },
        invalidEmail() {
            return this.employee.email === ""
        }
    }
}
</script>

<template>
    <div class="flex justify-center p-5 ">
        <form @submit.prevent="handleSubmit">
            <div class="w-96 p-6 shadow-lg rounded-md bg-white">
                <h1 class="text-3xl font-semibold text-center text-indigo-500">Login</h1>
                <hr class="mt-2">
                <div class="mt-3">
                    <label for="name" class="block text-base mb-1 font-semibold">Name</label>
                    <input type="text" placeholder="Enter your name"
                        class="w-full focus:border-indigo-500 border-2 border-gray-300 rounded-md text-base focus:border-2 px-2 py-1 focus:outline-none"
                     v-model="employee.name"  :class="{'has-employee':submitting&&invalidName}" @focus="clearStatus" @keypress="clearStatus" ref="firstInput"/>
                </div>
                <div class="mt-3">
                    <label for="name" class="block text-base mb-1 font-semibold">Email</label>
                    <input type="email" placeholder="Enter your email"
                        class="w-full border-2 border-gray-300 focus:border-indigo-500 rounded-md text-base focus:border-2 px-2 py-1 focus:outline-none"
                     v-model="employee.email" :class="{'has-employee':submitting&&invalidEmail}" @focus="clearStatus" @keypress="clearStatus"/>
                </div>
                <p v-if="submitting && error" class="error-msg mt-2 text-base">Please fill out all the required fields</p>
                <p v-if="success" class="success-msg mt-2 text-base">User created successfully...👍🏻</p>
                <div class="mt-6">
                    <button type="submit"
                        class="w-full border-2 bg-indigo-500 rounded-md px-2 py-1 text-white hover:bg-white hover:text-indigo-500 hover:border-indigo-500">Login</button>
                </div>
            </div>
        </form>
    </div>

</template>

<style scoped>
.has-employee{
    border: 2px solid red;
}

.error-msg{
    color: rgb(250, 20, 20);
}

.success-msg{
    color:rgb(3, 159, 3)
}
</style>
