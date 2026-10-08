<script setup lang="ts">
import { computed, ref } from 'vue'

interface Task {
  id: number
  label: string
  done: boolean
}

const newTask = ref('')
const tasks = ref<Task[]>([])
let nextId = 0

const remainingTasks = computed(() => tasks.value.filter((task) => !task.done).length)

function addTask() {
  const label = newTask.value.trim()
  if (!label) return

  tasks.value.push({ id: nextId++, label, done: false })
  newTask.value = ''
}

function deleteTask(id: number) {
  tasks.value = tasks.value.filter((task) => task.id !== id)
}
</script>

<template>
  <div class="app-shell">
    <header class="topbar">
      <a class="brand" href="#top" aria-label="Petit plan, accueil">
        <span class="brand-mark" aria-hidden="true"><span></span><span></span><span></span></span>
        <span>petit plan<span class="brand-period">.</span></span>
      </a>
      <span class="topbar-note">Une chose à la fois</span>
    </header>

    <main id="top" class="page-content">
      <section class="intro">
        <p class="eyebrow"><span>01</span> CARNET DU QUOTIDIEN</p>
        <h1>Les choses avancent,<br /><em>une à la fois.</em></h1>
        <p class="intro-copy">Notez vos priorités et gardez un œil sur ce qu’il vous reste à faire.</p>
      </section>

      <section class="task-board" aria-label="Gestionnaire de tâches">
        <div class="task-panel">
          <div class="panel-heading">
            <div>
              <p class="step-label">VOTRE LISTE</p>
              <h2>À faire aujourd’hui</h2>
            </div>
            <span class="panel-index">JOUR / 01</span>
          </div>

          <form class="add-form" @submit.prevent="addTask">
            <label class="sr-only" for="new-task">Nouvelle tâche</label>
            <input id="new-task" v-model="newTask" type="text" maxlength="120" placeholder="Ajouter une tâche…" autocomplete="off" />
            <button class="add-button" type="submit" aria-label="Ajouter la tâche"><span aria-hidden="true">+</span></button>
          </form>

          <div class="list-heading"><span>MES TÂCHES</span><span v-if="tasks.length">{{ tasks.length }} au total</span></div>
          <p v-if="tasks.length === 0" class="empty-state">Votre liste est encore vide.<br />Ajoutez une première tâche pour commencer.</p>
          <ul v-else class="task-list">
            <li v-for="task in tasks" :key="task.id" class="task-item" :class="{ completed: task.done }">
              <label class="task-label">
                <input v-model="task.done" type="checkbox" />
                <span class="checkmark" aria-hidden="true"></span>
                <span class="task-name">{{ task.label }}</span>
              </label>
              <button class="delete-button" type="button" :aria-label="`Supprimer ${task.label}`" @click="deleteTask(task.id)">Supprimer</button>
            </li>
          </ul>
        </div>

        <aside class="progress-panel" aria-live="polite">
          <div class="result-topline"><span>EN COURS</span><span class="result-icon" aria-hidden="true">↗</span></div>
          <p class="remaining-count">{{ remainingTasks }}<span>/ {{ tasks.length }}</span></p>
          <h2>{{ remainingTasks === 1 ? 'tâche restante' : 'tâches restantes' }}</h2>
          <p class="progress-note">{{ tasks.length === 0 ? 'Un petit pas suffit pour démarrer.' : remainingTasks === 0 ? 'Tout est terminé. Belle avancée.' : 'Continuez à votre rythme, vous avancez.' }}</p>
          <div class="progress-track" role="progressbar" :aria-valuenow="tasks.length - remainingTasks" :aria-valuemax="tasks.length || 1" aria-label="Tâches terminées">
            <span :style="{ width: `${tasks.length ? ((tasks.length - remainingTasks) / tasks.length) * 100 : 0}%` }"></span>
          </div>
          <p class="progress-caption">{{ tasks.length - remainingTasks }} terminée{{ tasks.length - remainingTasks > 1 ? 's' : '' }}</p>
        </aside>
      </section>

      <footer class="page-footer"><span>FAIRE DE LA PLACE À L’ESSENTIEL</span><span class="footer-rule"></span><span>{{ remainingTasks }} À FAIRE</span></footer>
    </main>
  </div>
</template>
