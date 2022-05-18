<template>
    <div class="flex flex-row items-center gap-x-4">
        <label
            :for="`todo-${todo.id}`" 
            class="todo-title"
            :class="{ 'todo-title-checked': isCompleted }"
        >
            <input
                :id="`todo-${todo.id}`"
                :name="`todo-${todo.id}`"
                type="checkbox"
                class="todo-input"
                v-model="isCompleted"
            />
            {{ todo.title }}
        </label>
    </div>
</template>

<script>
import moment from 'moment';

export default {
    name: 'TodoItem',

    props: ['todo'],

    data: function () {
        return {
            isCompleted: false
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
        })
    }
}
</script>

<style scoped>
    .todo-title {
        @apply font-medium text-neutral-200 select-none py-4 cursor-pointer
    };

    .todo-title-checked {
        @apply text-neutral-300 line-through
    };

    .todo-input {
        @apply focus:ring-indigo-500 h-4 w-4 text-indigo-600 border-gray-800 rounded cursor-pointer
    };
</style>