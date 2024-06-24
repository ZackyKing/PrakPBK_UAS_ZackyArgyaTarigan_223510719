<template>
  <q-page class="column items-center justify-center q-pa-md bg-image">
    <div class="tasks-container">
      <div class="interactive-buttons">
        <q-btn flat @click="onButtonClick" label="1" color="primary" class="interactive-btn" />
        <q-btn flat @click="onButtonClick" label="2" color="primary" class="interactive-btn" />
        <q-btn flat @click="onButtonClick" label="3" color="primary" class="interactive-btn" />
        <q-btn flat @click="onButtonClick" label="4" color="primary" class="interactive-btn" />
      </div>
      <div class="tasks-section">
        <q-select
          v-model="selectedTask"
          :options="tasks"
          label="Select Task"
          emit-value
          map-options
          style="min-width: 250px;"
          transition-show="scale-transition"
          transition-hide="scale-transition"
        />
        <q-btn @click="openTaskInNewTab" label="Go to Task" color="primary" :loading="loading" />
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue';

const tasks = ref([
  { label: 'Tugas 1', value: 'https://zacky-projectcv.vercel.app/' },
  { label: 'Tugas 2', value: 'https://github.com/ZackyKing/PrakPBKTUGAS2/tree/main' },
  { label: 'Tugas 3', value: 'https://223510719-tarigan.vercel.app/' },
  { label: 'Tugas 4', value: 'https://github.com/ZackyKing/223510719-zacky-argya-tarigan-tugas4' },
  { label: 'Tugas 5', value: 'https://zackytarigan-223510719-googleplaystore.vercel.app/' },
  { label: 'Tugas 6', value: 'https://zackyargyatarigan-223510719-db-t6.vercel.app/' },
]);

const selectedTask = ref(null);
const loading = ref(false);

const openTaskInNewTab = () => {
  if (selectedTask.value) {
    loading.value = true;
    setTimeout(() => {
      window.open(selectedTask.value, '_blank');
      loading.value = false;
    }, 1000);
  }
};

const onButtonClick = () => {
  alert('Button clicked!');
};
</script>

<style scoped>
@keyframes pulse {
  0% {
    transform: scale(1);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  }
  50% {
    transform: scale(1.05);
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.4);
  }
  100% {
    transform: scale(1);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  }
}

.tasks-container {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.interactive-buttons {
  position: absolute;
  display: flex;
  gap: 10px;
  top: -50px;
}

.interactive-btn {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  transition: transform 0.3s, background-color 0.3s;
}

.interactive-btn:hover {
  transform: scale(1.2);
  background-color: #007bff !important;
}

.tasks-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
  background: #f4e4c1;
  padding: 40px;
  border-radius: 15px;
  font-size: 1.2em;
  animation: pulse 2s infinite;
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.3);
  transform: perspective(1000px) rotateY(10deg);
}

.q-btn {
  font-size: 1em !important;
  padding: 10px 20px;
  transition: background-color 0.3s, transform 0.3s;
}

.q-btn:hover {
  background-color: #007bff !important;
  transform: translateY(-3px);
}

.q-select {
  font-size: 1.2em !important;
}
</style>
