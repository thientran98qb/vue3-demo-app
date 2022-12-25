<script lang="ts">
import { defineComponent, PropType } from 'vue';
import { DeleteOutlined } from "@ant-design/icons-vue"
import { TodoType } from '@/types/Todo'
export default defineComponent({
  name: "TodoItem",
  props: {
    todo : {
      type: Object as PropType<TodoType>
    }
  },
  components: {
    DeleteOutlined
  },
  emits: ['deleteItem'],
  setup(_, context) {
    const deleteTodoItem = (id: string | number) => {
      context.emit('deleteItem', id)
    }
    return {
      deleteTodoItem
    }
  }
})
</script>
<template>
  <li class="border w-full p-2 text-left font-bold shadow-md mb-1 flex items-center justify-between border-slate-300 rounded">
    <span>{{ todo.title }}</span>
    <div
      class="border bg-red-500 rounded text-white align-middle p-2 flex items-center hover:bg-red-600 cursor-pointer"
      @click="deleteTodoItem(todo.id)"
    >
      <delete-outlined />
    </div>
  </li>
</template>
