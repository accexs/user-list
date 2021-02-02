<template>
    <div>
        <table class="w-full table-fixed border-2 border-gray-300 rounded-sm">
            <tr class="text-left font-extrabold">
                <th class="border-2 border-gray-300 p-3 w-64">Name</th>
                <th class="border-2 border-gray-300 p-3">Email</th>
                <th class="border-2 border-gray-300 p-3 text-center w-52">Actions</th>
            </tr>
            <tbody>
            <tr v-for="user in users" :key="user.id">
                <td class="border-2 border-gray-300 p-3">{{ user.name }}</td>
                <td class="border-2 border-gray-300 p-3">{{ user.email }}</td>
                <td class="border-2 border-gray-300 p-3">
                    <div class="flex justify-center space-x-1 text-white h-9">
                        <button class="w-full bg-green-400 rounded-md" @click="updateUser(user.id)">Update</button>
                        <button class="w-full bg-red-600 rounded-md" @click="deleteUser(user.id)">Delete</button>
                    </div>
                </td>
            </tr>
            </tbody>
        </table>
        <card-modal :showModal="showUpdateModal" @close="closeModal">
            <create-user mode="edit" :id="userId"></create-user>
        </card-modal>
    </div>
</template>

<script>
import CardModal from "@/components/CardModal";
import CreateUser from "@/components/CreateUser";

export default {
    name: 'UserList',
    components: {CreateUser, CardModal},
    data() {
        return {
            users: [],
            showUpdateModal: false,
            userId: Number,
        }
    },
    mounted() {
        this.getUsers();
        this.$vueEventBus.$on('userCreated', this.getUsers);
    },
    methods: {
        getUsers() {
            axios.get('/api/users')
                .then(response => {
                    this.users = response.data.data
                })
                .catch(error => console.log(error.response));
        },
        deleteUser(id) {
            axios.delete('/api/users/' + id)
                .then(response => {
                    let i = this.users.map(item => item.id).indexOf(id);
                    this.users.splice(i, 1);
                })
        },
        async updateUser(id) {
            this.userId = id;
            this.showModal();
        },
        showModal() {
            this.showUpdateModal = true;
        },
        closeModal() {
            this.showUpdateModal = false;
        }
    }
}
</script>

<style scoped>

</style>
