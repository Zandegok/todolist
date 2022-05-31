<template>
  <div class="container shadow-lg px-5 py-5 rounded-3">
    <h1 class="mb-5">Планировщик задач</h1>

    <div class="d-flex mb-5">
      <input
          v-model="newTodoItemText"
          type="text"
          placeholder="Введите задачу здесь"
          class="form-control form-input me-3"
      />
      <button type="submit" class="submit-btn px-3 py-2" @click="addNewTodoItem">
        +
      </button>
    </div>

    <div
        class="row todo-list shadow px-3 pt-3 pb-2 align-items-center mb-4"
        v-for="(todoItem, index) in todoItems"
        :key="index"
    >

      <div class="col-7 text-start">
        <h5 :class="{ 'todo-done': todoItem.status === todoItemStatuses.Done }">
          {{ todoItem.text }}
        </h5>
      </div>

      <div class="col-5 text-end action-btn">
        <div class="d-flex justify-content-end">
          <div @click="moveUpTodoItem(index)">
            <i class="ms-4">U</i>
          </div>
          <div @click="moveDownTodoItem(index)">
            <i class="ms-4">D</i>
          </div>
          <div @click="changeTodoItemStatus(index)">
            <i class="ms-4">
              {{ todoItem.status === todoItemStatuses.InProgress ? 'Done' : 'InProgress' }}
            </i>
          </div>
          <div @click="deleteTodoItem(index)">
            <i class="ms-4">DEL</i>
          </div>
        </div>
      </div>

      <!--      <div class="col-2">-->
      <!--        <div class="d-flex justify-content-start align-items-center">-->
      <!--          <div-->
      <!--              class="status-indicator mb-1 me-2"-->
      <!--              :class="{-->
      <!--              'status-indicator-todo': todo.status === 'to-do',-->
      <!--              'status-indicator-ongoing': todo.status === 'on-going',-->
      <!--              'status-indicator-finished': todo.status === 'finished',-->
      <!--            }"-->
      <!--          ></div>-->
      <!--          <div-->
      <!--              class="status-text"-->
      <!--              @click="changeStatus(index)"-->
      <!--              :class="{-->
      <!--              'status-text-todo': todo.status === 'to-do',-->
      <!--              'status-text-ongoing': todo.status === 'on-going',-->
      <!--              'status-text-finished': todo.status === 'finished',-->
      <!--            }"-->
      <!--          >-->
      <!--            <h5>{{ todo.status }}</h5>-->
      <!--          </div>-->
      <!--        </div>-->
      <!--      </div>-->


    </div>

  </div>
</template>

<script>

export default {
  name: 'TodoList',
  data() {
    return {
      newTodoItemText: "",
      todoItemStatuses: {InProgress: "InProgress", Done: "Done"},
      todoItems: []
    }
  },
  methods: {
    addNewTodoItem() {
      this.todoItems.push({
        text: this.newTodoItemText,
        status: this.todoItemStatuses.InProgress
      });

      this.newTodoItemText = "";
    },
    changeTodoItemStatus(index) {
      let currentStatus = this.todoItems[index].status;
      currentStatus = this.changeStatus(currentStatus);
      this.todoItems[index].status = currentStatus;
    },
    changeStatus(status) {
      if (status == this.todoItemStatuses.InProgress) {
        return this.todoItemStatuses.Done;
      } else {
        return this.todoItemStatuses.InProgress;
      }
    },
    moveUpTodoItem(index) {
      if (index === 0) return;

      [this.todoItems[index], this.todoItems[index - 1]] = [this.todoItems[index - 1], this.todoItems[index]]
    },
    moveDownTodoItem(index) {
      if (index === this.todoItems.length - 1) return;

      [this.todoItems[index], this.todoItems[index + 1]] = [this.todoItems[index + 1], this.todoItems[index]]
    },
    deleteTodoItem(index) {
      this.todoItems.splice(index, 1);
    }
  }
}
</script>

<style scoped>
.todo-list {
  border-radius: 50px;
}

.todo-done {
  font-style: italic;
  text-decoration: line-through;
}

.submit-btn {
  background-color: skyblue;
  border-radius: 50%;
  border: none;
  font-size: 20px;
  font-weight: 800;
  color: #333;
}
</style>
