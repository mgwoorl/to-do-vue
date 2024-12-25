<template>
  <div class="todo-container">
    <h2>Список задач</h2>
    <div>
      <input v-model="newTaskName" placeholder="Название задачи" />
      <select v-model="newTaskCategory">
        <option>Работа</option>
        <option>Личное</option>
        <option>Учеба</option>
      </select>
      <div>
        <label>
          <input type="radio" value="Высокий" v-model="newTaskPriority" />
          Высокий приоритет
        </label>
        <label>
          <input type="radio" value="Низкий" v-model="newTaskPriority" />
          Низкий приоритет
        </label>
      </div>
      <button @click="addTask">Добавить задачу</button>
    </div>

    <ul>
      <li v-for="task in tasks" :key="task.id" :class="{ completed: task.completed }">
        <div v-if="!task.editing">
          <input type="checkbox" v-model="task.completed" />
          <span v-if="task.priority === 'Высокий'">⚡</span>
          <strong>{{ task.name }}</strong> ({{ task.category }}) - {{ task.priority }}

          <button @click="editTask(task)">Редактировать</button>
          <button @click="removeTask(task.id)">Удалить</button>
        </div>
        <div v-else>
          <input v-model="task.editableCopy.name" />
          <select v-model="task.editableCopy.category">
            <option>Работа</option>
            <option>Личное</option>
            <option>Учеба</option>
          </select>
          <div>
            <label>
              <input type="radio" value="Высокий" v-model="task.editableCopy.priority" />
              Высокий приоритет
            </label>
            <label>
              <input type="radio" value="Низкий" v-model="task.editableCopy.priority" />
              Низкий приоритет
            </label>
          </div>
          <button @click="saveTask(task)">Сохранить</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTaskName: '',
      newTaskCategory: 'Работа',
      newTaskPriority: 'Низкий',
      tasks: [],
      nextId: 1,
    };
  },
  methods: {
    addTask() {
      if (this.newTaskName.trim() === '') {
        return;
      }
      this.tasks.push({
        id: this.nextId++,
        completed: false,
        name: this.newTaskName,
        category: this.newTaskCategory,
        priority: this.newTaskPriority,
        editing: false,
        editableCopy: {
          name: this.newTaskName,
          category: this.newTaskCategory,
          priority: this.newTaskPriority,
        },
      });
      this.newTaskName = '';
      this.newTaskCategory = 'Работа';
      this.newTaskPriority = 'Низкий';
    },
    editTask(task) {
      task.editing = true;
      task.editableCopy = { ...task };
    },
    saveTask(task) {
      task.name = task.editableCopy.name;
      task.category = task.editableCopy.category;
      task.priority = task.editableCopy.priority;
      task.editing = false;
    },
    removeTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
    },
  },
};
</script>

<style>
.todo-container {
  background-color: #fff;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  margin: 0 auto;
}

.completed {
  text-decoration: line-through;
  color: gray;
}

h2 {
  text-align: center;
}
</style>