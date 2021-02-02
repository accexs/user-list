<template>
    <div>
        <div class="flex w-full border-2 border-gray-300 h-18 p-4 rounded-sm space-x-3">
            <form @submit.prevent="createUser">
                <input v-model="user.name" type="text" required
                       class="px-4 py-1 border focus:ring-2 focus:ring-blue-300 focus:border-blue-300 border-gray-300 rounded-md"
                       placeholder="Name">
                <input v-model="user.email" type="email" required
                       class="px-4 py-1 border focus:ring-2 focus:ring-blue-300 focus:border-blue-300 border-gray-300 rounded-md"
                       placeholder="Email">
                <button class="rounded-md text-white bg-blue-500 px-3 py-1">Submit</button>
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
                email: ''
            }
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
                    .catch(error => console.log(error))
            } else {
                axios.post('/api/users', this.user)
                    .then(response => {
                        this.$vueEventBus.$emit('userCreated', response.data.data)
                        this.user = {name: '', email: ''}
                    })
                    .catch(error => console.log(error))
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
