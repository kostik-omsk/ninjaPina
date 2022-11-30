<template>
  <main>
    <header>
      <img src="./assets/pinia-logo.svg" alt="pinia logo" />
      <h1>Pinia</h1>
    </header>
    <div class="new-task-form">
      <TaskForm />
    </div>

    <div class="loading" v-if="loading">
      <h1>load...</h1>
    </div>

    <nav class="filter">
      <button @click="filter = 'all'">Все Задачи</button>
      <button @click="filter = 'favs'">Любимые задачи</button>
    </nav>
    <div class="task-list" v-if="filter == 'all'">
      <p>Всего задач + {{ totalCount }}</p>
      <div v-for="task in tasks" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>
    <div class="task-list" v-if="filter == 'favs'">
      <p>Любимые задачи {{ favCount }}</p>
      <div v-for="task in favs" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>
    <button class="btn-reset" @click="taskStore.$reset">reset</button>
  </main>
</template>

<script>
  import { storeToRefs } from 'pinia';
  import { ref } from 'vue';
  import TaskDetails from './components/TaskDetails.vue';
  import TaskForm from './components/TaskForm.vue';
  import { useTaskStore } from './store/TaskStore';
  export default {
    components: {
      TaskDetails,
      TaskForm,
    },
    setup() {
      const filter = ref('all');
      const taskStore = useTaskStore();

      const { tasks, loading, favs, totalCount, favCount } =
        storeToRefs(taskStore);

      taskStore.getTask();
      return { taskStore, filter, tasks, loading, favs, totalCount, favCount };
    },
  };
</script>
