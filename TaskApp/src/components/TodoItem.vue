<template>
  <div class="todo-card">
    <h3
      class="red12"
      style="
        font-family: Koulen, sans-serif;
        font-weight: 400;
        font-style: normal;
        text-transform: uppercase;
        font-size: 25px;
        margin-bottom: 2px;
        background-color: rgb(252, 238, 113);
        width: 150px;
        text-align: center;
        border-radius: 7px;
        border: 2px solid #ccc;
      "
    >
      {{ updatedTitle }}
    </h3>
    <p
      style="
        font-family: Fira Sans Extra Condensed, sans-serif;
        font-weight: 400;
        font-size: 18px;
        font-style: normal;
        margin-bottom: 5px;
      "
    >
      {{ updatedDescription }}
    </p>
    <p
      style="
        font-family: Sansita, sans-serif;
        font-weight: 400;
        font-style: normal;
        font-size: 20px;
        margin-bottom: 20px;
      "
    >
      Status: {{ updatedStatus }}
    </p>

    <div class="cont2" style="display: flex">
      <button
        class="updateButton"
        @click="updateStatus"
        style="background-color: rgb(243, 11, 139)"
      >
        Update Status
      </button>
      <button
        class="deleteButton"
        @click="deleteTodo"
        style="background-color: rgb(78, 243, 52)"
      >
        Delete Todo
      </button>
      <button
        class="updateButton"
        @click="updateTodo"
        style="background-color: rgb(21, 186, 241)"
      >
        Update Todo
      </button>
    </div>

    <div
      v-if="isUpdatePopupVisible"
      class="update-todo-popup"
      style="background-color: white"
    >
      <div class="popup-content">
        <h2 style="color: blue; font-family: Sofia, sans-serif">
          Update Status
        </h2>
        <select v-model="updatedStatus" class="status-dropdown">
          <option value="pending">Pending</option>
          <option value="completed">Completed</option>
        </select>
        <button @click="confirmUpdate" class="updateButton">Confirm</button>
        <button @click="cancelUpdate" class="cancel-button">Cancel</button>
      </div>
    </div>

    <div v-if="isUpdateTodoPopupVisible" class="update-todo-popup">
      <div class="popup-content">
        <h2 class="red5">Update Todo</h2>
        <input v-model="updatedTitle" placeholder="Title" class="title1" />
        <input
          v-model="updatedDescription"
          placeholder="Description"
          class="desp"
        />
        <select v-model="updatedStatus" class="status-dropdown">
          <option value="pending">Pending</option>
          <option value="completed">Completed</option>
        </select>

        <button @click="confirmUpdateTodo" class="updateButton">Save</button>
        <button @click="cancelUpdateTodo" class="cancel-button">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, ref, defineEmits, onMounted } from "vue";

const props = defineProps(["title", "description", "status"]);
const emit = defineEmits(["updateStatus", "deleteTodo", "updateTodo"]);

const todoStatus = ref(props.status);
const isUpdatePopupVisible = ref(false);
const isUpdateTodoPopupVisible = ref(false);
const updatedStatus = ref(todoStatus.value);
const updatedTitle = ref(props.title);
const updatedDescription = ref(props.description);

const updateStatus = () => {
  isUpdatePopupVisible.value = true;
};

const confirmUpdate = () => {
  emit("updateStatus", updatedStatus.value);
  isUpdatePopupVisible.value = false;
};

const cancelUpdate = () => {
  updatedStatus.value = todoStatus.value;
  isUpdatePopupVisible.value = false;
};

const updateTodo = () => {
  isUpdateTodoPopupVisible.value = true;
};

const confirmUpdateTodo = () => {
  emit("updateTodo", {
    title: updatedTitle.value,
    description: updatedDescription.value,
    status: updatedStatus.value,
  });
  isUpdateTodoPopupVisible.value = false;
};

const cancelUpdateTodo = () => {
  updatedTitle.value = props.title;
  updatedDescription.value = props.description;
  updatedStatus.value = todoStatus.value;
  isUpdateTodoPopupVisible.value = false;
};

const deleteTodo = () => {
  emit("deleteTodo");
};
</script>
<style scoped>
.todo-card {
  border: 1px solid #ccc;
  padding: 15px;
  margin: 10px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.updateStatus {
  margin-right: 10px;
  margin-left: 5px;
}
.updateButton {
  margin-top: 10px;
  color: white;
  background-color: rgb(19, 220, 19);
  border: 1px solid white;
  height: 40px;
  width: 120px;
  border-radius: 10px;
  margin-right: 10px;
}

.deleteButton {
  margin-top: 10px;
  color: white;
  background-color: rgb(16, 185, 16);
  border: 1px solid white;
  width: 120px;
  height: 40px;
  border-radius: 10px;
  margin-right: 10px;
}
.deleteButton:hover {
  transform: scale(1.05);
}
.update-todo-popup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
  width: 450px;
  padding: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.popup-content {
  text-align: center;
}
.red12 {
  background-color: yellow;
  transform: scale(1.05);
}
.red5 {
  color: #1d09fb;
  font-size: 24px;
  font-weight: bold;
  font-family: "Sofia", sans-serif;
}

.title1,
.desp {
  width: 400px;
  height: 30px;
  border-radius: 7px;
  padding-right: 20px;
  padding-left: 20px;
  margin-bottom: 10px;
}
.status-dropdown {
  width: 390px;
  height: 35px;

  border-radius: 7px;
  padding-right: 20px;
  padding-left: 20px;

  margin-bottom: 10px;
  border: 2px dotted black;
}
.updateButton,
.cancel-button {
  margin-top: 10px;
  color: white;
  background-color: rgb(19, 220, 19);
  border: 1px solid white;
  height: 40px;
  width: 120px;
  border-radius: 10px;
  margin-right: 10px;
}
.updateButton:hover {
  transform: scale(1.05);
}
.cancel-button:hover {
  background-color: #999;
}
.status-dropdown {
  margin-top: 10px;
  width: 100%;
  padding: 8px;
}
</style>
