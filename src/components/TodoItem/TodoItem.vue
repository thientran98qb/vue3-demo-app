<script lang="ts">
import { defineComponent, nextTick, PropType, reactive, ref, toRefs } from 'vue';
import { DeleteOutlined, CheckOutlined, CloseOutlined, EditOutlined, CheckSquareOutlined } from "@ant-design/icons-vue"
import { TodoType } from '@/types/Todo'
export default defineComponent({
  name: "TodoItem",
  props: {
    todo : {
      type: Object as PropType<TodoType>
    }
  },
  components: {
    DeleteOutlined,
    CheckOutlined,
    CloseOutlined,
    EditOutlined,
    CheckSquareOutlined
  },
  emits: ['deleteItem', 'updateItem', 'completed'],
  setup(props, context) {
    const inputUpdateRef = ref<HTMLElement | null>()
    const state = reactive({
      editItems: {
        ...props.todo,
        edit: false
      },
      textUpdate: props.todo?.title
    })
    const deleteTodoItem = (id: string | number) => {
      context.emit('deleteItem', id)
    }
    const editTodoItem = () => {
      state.editItems.edit = !state.editItems.edit
      nextTick(() => {
        inputUpdateRef.value?.focus();
      });
    }
    const updateTodoItem = (id: string | number, textUpdate: string) => {
      context.emit('updateItem', id, textUpdate)
      state.editItems.edit = false
    }

    const completedTask = (id: string | number) => {
      context.emit('completed', id)
    }

    return {
      deleteTodoItem,
      editTodoItem,
      updateTodoItem,
      completedTask,
      ...toRefs(state),
      inputUpdateRef
    }
  }
})
</script>
<template>
  <li class="border w-full p-2 text-left font-bold shadow-md mb-1 flex justify-between flex-col border-slate-300 rounded">
    <div class="flex items-center justify-between">
      <span :class="todo.completed && 'line-through'">{{ todo.title }}</span>
      <div class="flex">
        <div
          class="border bg-green-500 rounded text-white align-middle p-2 flex items-center hover:bg-green-600 cursor-pointer"
          @click="completedTask(todo.id)"
        >
          <check-square-outlined />
        </div>
        <div
          class="border bg-blue-500 rounded text-white align-middle p-2 flex items-center hover:bg-blue-600 cursor-pointer"
          @click="editTodoItem"
        >
          <edit-outlined />
        </div>
        <div
          class="border bg-red-500 rounded text-white align-middle p-2 flex items-center hover:bg-red-600 cursor-pointer"
          @click="deleteTodoItem(todo.id)"
        >
          <delete-outlined />
        </div>
      </div>
    </div>
    <div v-show="editItems.edit" class="border mt-2 ml-2 flex justify-between items-center rounded">
      <input type="text" v-model="textUpdate" v-show="editItems.edit" class="w-full p-1 focus:outline-none" ref="inputUpdateRef">
      <div class="flex gap-2 pr-2">
        <span
          class="flex text-center items-center font-bold rounded-full text-white text-sm bg-green-600 cursor-pointer hover:opacity-80"
          @click="updateTodoItem(todo.id, textUpdate)"
        >
          <check-outlined class="p-1"/>
        </span>
        <span class="flex text-center items-center  font-bold rounded-full text-white bg-red-600 text-sm cursor-pointer hover:opacity-80">
          <close-outlined @click="editItems.edit = false" class="p-1"/>
        </span>
      </div>
    </div>
  </li>
</template>
