<template>
    <transition name="fade">
        <div v-if="showModal"
             class="fixed inset-0 w-full h-screen flex items-center justify-center z-max"
             @click.self="close">
            <div class="relative w-full max-w-3xl bg-white shadow-lg rounded-md px-16 py-12">
                <button aria-label="close"
                        class="absolute top-0 right-0 text-4xl text-gray-500 my-2 mx-4"
                        @click.prevent="close">
                    ×
                </button>
                <slot></slot>
            </div>
        </div>
    </transition>
</template>

<script>
export default {
    name: 'CardModal',
    props: {
        showModal: {
            required: true,
            type: Boolean
        }
    },
    mounted() {
        this.$vueEventBus.$on('closeUpdateModal', this.close);
    },
    watch: {
        showModal(value) {
            if (value) {
                return document.querySelector('body').classList.add('overflow-hidden');
            }
            document.querySelector('body').classList.remove('overflow-hidden');
        }
    },
    methods: {
        close() {
            this.$emit('close');
        }
    }
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
    transition: all 0.4s;
}

.fade-enter,
.fade-leave-to {
    opacity: 0;
}
</style>
