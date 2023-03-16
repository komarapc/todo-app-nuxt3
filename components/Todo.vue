<template>
  <title>Todo</title>
  <div
    class="flex w-full min-h-screen bg-neutral-200 items-center justify-center p-5"
  >
    <div
      class="my-10 w-full md:w-1/2 lg:w-4/12 xl:w-3/12 flex flex-col items-center justify-center"
    >
      <span class="text-4xl my-10">{{ app_name }} </span>
      <div class="flex flex-col gap-4 w-full bg-white shadow-md rounded-lg p-5">
        <input
          type="text"
          class="p-3 w-full outline-none border border-neutral-300 rounded"
          placeholder="Enter todo name"
          v-model="todo_name"
          v-on:keydown="addTodo"
        />
        <div class="flex flex-col gap-4">
          <span>Total {{ todoList.length }}</span>
          <div class="flex flex-col gap-4">
            <div
              v-for="todo in todoList"
              :key="todo.id"
              class="flex flex-row justify-between items-center px-4 py-3 rounded-lg bg-neutral-100 gap-4"
              :class="todo.completed ? 'bg-emerald-100' : ''"
            >
              <Icon
                name="ic:outline-done-all"
                class="text-emerald-600 text-2xl"
                v-if="todo.completed"
              ></Icon>
              <span
                v-if="todo.id !== selectedUpdate.id"
                class="w-full"
                :class="todo.completed ? 'text-emerald-600' : ''"
                @dblclick="selectedUpdate = todo"
                >{{ todo.title }}</span
              >
              <input
                v-if="todo.id === selectedUpdate.id"
                type="text"
                class="p-2 w-full outline-none border border-neutral-300 rounded"
                placeholder="Enter todo name"
                v-model="todo.title"
                @blur="selectedUpdate = {}"
              />
              <div class="flex flex-row gap-4 items-center justify-between">
                <div
                  class="flex items-center justify-center p-1 bg-emerald-600 rounded-full cursor-pointer hover:bg-emerald-500"
                  title="completed"
                  @click="todoCompleted(todo?.id)"
                >
                  <Icon name="ic:outline-done" class="text-white"></Icon>
                </div>
                <div
                  class="flex items-center justify-center p-1 bg-red-600 rounded-full cursor-pointer hover:bg-red-500"
                  title="Remove"
                  @click="removeTodo(todo?.id)"
                >
                  <Icon
                    name="material-symbols:delete-forever-rounded"
                    class="text-white"
                  ></Icon>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { nanoid } from "nanoid";
interface TodoProps {
  id?: string;
  title?: string;
  completed?: boolean;
}
export default {
  name: "Todo",
  data() {
    return {
      app_name: "Todo App",
      todo_name: "",
      todoList: [] as TodoProps[],
      selectedUpdate: {} as TodoProps,
    };
  },
  methods: {
    addTodo(e: KeyboardEvent) {
      if (e.code === "Enter") {
        if (this.todo_name === "") return;
        const copyTodoList = [...this.todoList];
        this.todoList = [
          { id: nanoid(), title: this.todo_name, completed: false },
          ...copyTodoList,
        ];
        this.todo_name = "";
      }
    },
    removeTodo: function (id: string | undefined) {
      this.todoList = this.todoList.filter((todo) => todo.id !== id);
    },
    todoCompleted: function (id: string | undefined) {
      this.todoList = this.todoList.map((todo) => {
        if (todo.id === id) {
          todo.completed = !todo.completed;
        }
        return todo;
      });
    },
  },
};
</script>
