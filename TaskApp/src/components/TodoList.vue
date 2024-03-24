<template>
  <div>
    <div class="app-container">
      <div class="heading1">My To-Do List</div>
      <div class="todo-heading">
        <button @click="showAddTodoPopup" class="add-button">Add Todo</button>
      </div>
    </div>
    <div class="quotation">
      ||----------------------------ORGANIZE YOUR WORK AND LIFE,
      FINALLY.-------------------------||
    </div>

    <div class="todo-container">
      <div v-for="(todo, index) in todos" :key="index" class="todo2">
        <TodoItem
          :title="todo.title"
          :description="todo.description"
          :status="todo.status"
          @updateStatus="updateTodoStatus(index, $event)"
          @deleteTodo="deleteTodo(index)"
          @updateTodo="updateTodo(index, $event)"
        />
      </div>
    </div>

    <div v-if="isAddTodoPopupVisible" class="add-todo-popup">
      <div class="popup-content">
        <h2 class="red5" style="color: blue; font-family: Sofia, sans-serif">
          Add Todo
        </h2>

        <input
          v-model="newTodo.title"
          placeholder="Title"
          class="title1"
          style="margin-right: 10px; height: 20px"
        />
        <input
          v-model="newTodo.description"
          placeholder="Description"
          class="desp"
          style="height: 20px"
        />

        <button
          @click="addTodo"
          class="add-button"
          style="margin-left: 10px; margin-right: 10px"
        >
          Add Todo
        </button>
        <button @click="hideAddTodoPopup" class="cancel-button">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, defineEmits, defineProps, onMounted, onBeforeUnmount } from "vue";
import TodoItem from "./TodoItem.vue";
const props = defineProps(["title", "description", "status"]);
const emit = defineEmits(["updateTodoStatus", "deleteTodo", "updateTodo"]);
const todos = ref([]);

const newTodo = ref({ title: "", description: "", status: "pending" });
const isAddTodoPopupVisible = ref(false);

const addTodo = () => {
  todos.value.push({ ...newTodo.value });
  saveData();
  newTodo.value = { title: "", description: "", status: "pending" };
  hideAddTodoPopup();
};

const updateTodoStatus = (index, newStatus) => {
  todos.value[index].status = newStatus;
  saveData();
};
const updateTodo = (index, updatedTodo) => {
  todos.value[index].title = updatedTodo.title;
  todos.value[index].description = updatedTodo.description;
  saveData();
};
const deleteTodo = (index) => {
  todos.value.splice(index, 1);
  saveData();
  removeData(index);
};

const showAddTodoPopup = () => {
  isAddTodoPopupVisible.value = true;
};

const hideAddTodoPopup = () => {
  isAddTodoPopupVisible.value = false;
  newTodo.value.title = "";
  newTodo.value.description = "";
};

onMounted(() => {
  getData();
});
onBeforeUnmount(() => {
  //   saveData();
});

const saveData = () => {
  localStorage.setItem("todos", JSON.stringify(todos.value));
};
const getData = () => {
  const storeData = localStorage.getItem("todos");
  todos.value = storeData ? JSON.parse(storeData) : [];
};

const removeData = (index) => {
  const store = JSON.parse(localStorage.getItem("todos")) || [];
  store.splice(index, 1);
  localStorage.removeItem("todos");
  localStorage.setItem("todos", JSON.stringify(store));
};
</script>

<style scoped>
.app-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-image: url("../assets/image2.jpg");

  padding: 20px;
  border-bottom: 2px solid #ccc;
  border-radius: 20px;
  margin-bottom: 10px;
}
body {
  background-image: url("../assets/image4.jpg");
}
.heading1 {
  flex: 1;
  text-align: center;
  display: block;
  font-size: 30px;
  font-weight: bolder;
  padding-left: 80px;
  align-content: center;
  height: 40px;
}
.heading1 :hover {
  transition: background-color 0.3s ease, transform 0.3s ease;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}
.add-button {
  background-color: #f06a04;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.add-button:hover {
  background-color: #39b2e6;
  transform: scale(1.05);
}

.todo-container {
  display: grid;
  margin-right: 20px;
  margin-left: 20px;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
.quotation {
  text-align: center;
  margin-bottom: 30px;
  align-content: center;
  font-family: "Trirong", serif;
  font-size: 25px;
  font-weight: bold;
  background-color: whitesmoke;
  font-family: "Yatra One", system-ui;
  font-weight: 400;
  font-style: normal;
  background-color: rgb(240, 217, 221);
  height: 40px;
  padding-top: 15px;
  border-radius: 30px;
  white-space: nowrap;
  overflow: hidden;
  border: 2px red;
  transition: color 0.3s ease;
}

.quotation:hover {
  background-color: rgb(255, 250, 193);

  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.todo2 {
  width: 430px;
  background-color: white;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.todo2:hover {
  transform: scale(1.05);
}

.add-todo-popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.popup-content {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.cancel-button {
  background-color: #ccc;
  color: white;
  border: none;
  padding: 10px 20px;
  margin-top: 10px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.cancel-button:hover {
  background-color: #999;
}
</style>
