<template>
    <div>
        <div class="w-full border-2 border-gray-300 h-18 p-4 rounded-sm">
            <form @submit.prevent="createUser">
                <div class="flex space-x-3">
                    <div class="flex-col">
                        <input v-model="user.name" type="text" required
                               @change="validationErrors.name = false"
                               class="px-4 py-1 border focus:ring-2 focus:ring-blue-300 focus:border-blue-300 border-gray-300 rounded-md"
                               placeholder="Name">
                        <p class="text-red-700" v-if="validationErrors.name" v-for="error in validationErrors.name">{{ error }}</p>
                    </div>
                    <div class="flex-col">
                        <input v-model="user.email" type="email" required
                               @change="validationErrors.email = false"
                               class="px-4 py-1 border focus:ring-2 focus:ring-blue-300 focus:border-blue-300 border-gray-300 rounded-md"
                               placeholder="Email">
                        <p class="text-red-700 text-sm" v-if="validationErrors.email" v-for="error in validationErrors.email">{{ error }}</p>
                    </div>
                    <button class="rounded-md text-white bg-blue-500 px-3 py-1 max-h-9">Submit</button>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CreateUser',
    props: {
        mode: {
            type: String,
            default: 'create'
        },
        id: Number
    },
    data() {
        return {
            user: {
                name: '',
                email: '',
            },
            validationErrors: {},
        }
    },
    mounted() {
        if (this.mode === 'edit') {
            this.getUser()
        }
    },
    methods: {
        createUser() {
            if (this.mode === 'edit') {
                axios.put('/api/users/' + this.id, this.user)
                    .then(response => {
                        this.$vueEventBus.$emit('userCreated', response.data.data)
                        this.$vueEventBus.$emit('closeUpdateModal');
                    })
                    .catch(error => {
                        if (error.response.status === 422) {
                            this.validationErrors = error.response.data.errors;
                        }
                    })
            } else {
                axios.post('/api/users', this.user)
                    .then(response => {
                        this.$vueEventBus.$emit('userCreated', response.data.data)
                        this.user = {name: '', email: ''}
                    })
                    .catch(error => {
                        if (error.response.status === 422) {
                            this.validationErrors = error.response.data.errors;
                        }
                    })
            }
        },
        getUser() {
            axios.get('/api/users/' + this.id)
                .then(response => {
                    this.user = response.data.data
                })
                .catch(error => console.log(error.response));
        }
    }
}
</script>

<style scoped>

</style>
