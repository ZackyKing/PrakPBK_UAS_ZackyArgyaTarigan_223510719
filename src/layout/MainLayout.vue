<template>
  <q-layout view="hHh lpR fFf bg-image">

    <q-header elevated class="bg-primary text-white custom-header" height-hint="98">
      <q-toolbar>
        <q-toolbar-title class="title-with-icon">
          <q-icon name="cloud" class="header-icon" /> ZackyArgyaTarigan (223510719)
        </q-toolbar-title>

        <div class="time-display">{{ time }}</div>

        <q-btn dense flat round icon="menu" @click="toggleRightDrawer" />
      </q-toolbar>

      <q-tabs align="center" class="animated-tabs">
        <q-route-tab to="/" label="Weather" />
        <q-route-tab to="/task" label="Task" />
      </q-tabs>
    </q-header>

    <q-drawer show-if-above v-model="rightDrawerOpen" side="right" bordered>
      <q-list>
        <q-item clickable v-ripple tag="router-link" to="/">
          <q-item-section>
            <q-icon name="cloud_queue" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Weather</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable v-ripple tag="router-link" to="/task">
          <q-item-section>
            <q-icon name="task" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Task</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>

    <q-footer elevated class="bg-primary text-white fixed-footer custom-footer" height-hint="98">
      <q-toolbar>
        <q-toolbar-title class="footer-title">
          <q-icon name="copyright" /> Copyright © ZackyArgyaTarigan®. All rights reserved.
        </q-toolbar-title>
      </q-toolbar>
    </q-footer>

  </q-layout>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  setup () {
    const rightDrawerOpen = ref(false)
    const time = ref(new Date().toLocaleTimeString())

    const toggleRightDrawer = () => {
      rightDrawerOpen.value = !rightDrawerOpen.value
    }

    const updateTime = () => {
      time.value = new Date().toLocaleTimeString()
    }

    onMounted(() => {
      setInterval(updateTime, 1000)
    })

    return {
      rightDrawerOpen,
      toggleRightDrawer,
      time
    }
  }
}
</script>

<style scoped>
.bg-image {
  background-image: url('../assets/bg1.gif');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

html, body, #app {
  height: 100%;
  margin: 0;
}

#app {
  display: flex;
  flex-direction: column;
}

.q-page-container {
  flex: 1;
}

.q-toolbar {
  padding: 0 1px !important; 
}

.q-toolbar__title:first-child {
  padding-left: 20px !important; 
}

.q-tab {
  min-height: 40px !important; 
  font-size: 2em !important; 
}

.q-footer {
  text-align: center;
  font-size: 2em !important; 
}

.footer-title {
  font-family: "sans-serif";
}

.time-display {
  margin-left: auto;
  margin-right: 20px;
  font-size: 1.5em;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
  100% {
    opacity: 1;
  }
}

.custom-header {
  background: linear-gradient(45deg, #2196F3, #21CBF3);
  color: white;
  font-size: 1.2em;
}

.custom-footer {
  background: linear-gradient(45deg, #2196F3, #21CBF3);
  color: white;
  font-size: 1.2em;
}

.title-with-icon {
  display: flex;
  align-items: center;
}

.header-icon {
  margin-right: 10px;
  animation: rotate 4s linear infinite;
}

@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.animated-tabs .q-tab {
  transition: transform 0.3s;
}

.animated-tabs .q-tab:hover {
  transform: scale(1.1);
}
</style>
