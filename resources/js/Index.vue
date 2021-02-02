<template>
    <div class="flex h-screen">
        <div class="flex-col h-4/5 w-3/4 m-auto border-2 border-black p-20">
            <div class="flex">
                <div class="w-1/3 my-auto">Date: {{ date }}</div>
                <div class="w-1/3 text-center">
                    <h1 class="text-4xl underline">'My Awesome list'</h1>
                </div>
            </div>
            <div class="mt-6">
                <div class="flex">
                    <div class="w-1/2 text-left">
                        <h2># Add user</h2>
                    </div>
                    <div class="w-1/2 text-right"> By: Ronny Arvelo</div>
                </div>
                <create-user class="mt-2"></create-user>
            </div>
            <div class="mt-6">
                <div>
                    <h2># Users</h2>
                </div>
                <user-list class="overflow-y-scroll md:h-60 mt-2"></user-list>
            </div>
        </div>
    </div>
</template>

<script>
import UserList from "@/components/UserList";
import CreateUser from "@/components/CreateUser";

export default {
    name: 'Index',
    components: {CreateUser, UserList},
    data() {
        return {
            date: ''
        }
    },
    mounted() {
        console.log('Component mounted.')
        this.date = this.getCurrentDate()
    },
    methods: {
        getCurrentDate() {
            let today = new Date();
            let dd = String(today.getDate()).padStart(2, '0');
            let mm = String(today.getMonth() + 1).padStart(2, '0'); //January is 0!
            let yyyy = today.getFullYear();

            return dd + '/' + mm + '/' + yyyy;
        }
    }
}
</script>
