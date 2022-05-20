<template>
    <div class="flex flex-row items-center justify-between w-full text-neutral-300">
        <div class="flex flex-row-reverse items-center gap-x-4">
            <label
                :for="`todo-${todo.id}`" 
                class="todo-title"
                :class="{ 'todo-title-checked': isCompleted }"
            >
                {{ todo.title }}
            </label>
            <input
                :id="`todo-${todo.id}`"
                :name="`todo-${todo.id}`"
                type="checkbox"
                class="todo-input"
                v-model="isCompleted"
            />         
        </div>
        <div class="relative">
            <DotsHorizontalIcon
                class="w-6 h-6 cursor-pointer"
                @click="showDropdown = !showDropdown"
            />
            <div v-show="showDropdown" class="absolute right-0 rounded-lg shadow-lg flex flex-col py-3 bg-neutral-800 z-10 border border-neutral-700">
                <div class="flex flex-row items-center gap-4 py-0.5 px-4 hover:bg-neutral-900 transition cursor-pointer">
                    <PencilIcon class="text-white w-4 h-4" />
                    <span>Edit</span>
                </div>

                <div class="flex flex-row items-center gap-4 py-0.5 px-4 hover:bg-neutral-900 transition cursor-pointer"
                    @click="destroy"
                >
                    <TrashIcon class="text-white w-4 h-4" />
                    <span>Delete</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import moment from 'moment';
import { DotsHorizontalIcon, PencilIcon, TrashIcon } from '@heroicons/vue/outline'

export default {
    name: 'TodoItem',

    components: { 
        DotsHorizontalIcon,
        PencilIcon,
        TrashIcon,
    },

    props: ['todo'],

    data: function () {
        return {
            isCompleted: false,
            showDropdown: false,
        }
    },

    created() {
        this.isCompleted = Boolean(this.todo.completed_at);
    },

    updated() {
        const isCompleted = this.isCompleted
            ? moment().format()
            : null;

        this.$emit('updatedCheck', {
            ...this.todo,
            completed_at: isCompleted,
        });
    },

    methods: {
        destroy() {
            this.$emit('deleteTodo', this.todo.id);
        }
    }
}
</script>

<style scoped>
    .todo-title {
        @apply font-medium text-neutral-200 select-none py-4 cursor-pointer
    };

    .todo-title-checked {
        @apply line-through
    };

    .todo-input {
        @apply focus:ring-indigo-500 h-4 w-4 text-indigo-600 border-gray-800 rounded cursor-pointer
    };
</style>