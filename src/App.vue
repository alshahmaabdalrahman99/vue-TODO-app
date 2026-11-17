<script setup>

/*ref()	Maakt een reactieve variabele.
Reactief betekent dat Vue veranderingen in de variabele bijhoudt.
Als de waarde verandert, werkt Vue de pagina automatisch bij.
computed()	maakt een berekende waarde.
watch()	Houdt veranderingen in gegevens in de gaten.*/

import { computed, ref, watch } from 'vue'
import TodoItem from './components/TodoItem.vue'

const STORAGE_KEY = 'vue-todo-app-tasks'
// loadTasks: Deze functie haalt eerder opgeslagen taken op.
function loadTasks() {
  /* try-catch: Probeer de code in het try-blok uit te voeren. Als er een fout optreedt,
   wordt de code in het catch-blok uitgevoerd.*/
  try {
    
    const savedTasks = localStorage.getItem(STORAGE_KEY)//localStorage.getItem:Haalt een waarde op uit de lokale opslag van de browser.
    return savedTasks ? JSON.parse(savedTasks) : []
  } catch (error) {
    console.error('Taken konden niet worden geladen:', error)
    return []
  }
}

const tasks = ref(loadTasks()) //	Bewaart alle taken.
const newTask = ref('')//	Bewaart de tekst die ik invoer.
const newPriority = ref('Middel')//Bewaart de gekozen prioriteit
const activeFilter = ref('Alles')//Bewaart het geselecteerde filter

const filters = ['Alles', 'Open', 'Afgerond']
const priorities = ['Hoog', 'Middel', 'Laag']

const openTaskCount = computed(() =>
  tasks.value.filter((task) => !task.completed).length /*in JavaScript gebruik je .value om de inhoud van een ref te lezen of te veranderen.
                                                          In de template gebruik je meestal alleen de naam*/
)

const completedTaskCount = computed(() =>
  tasks.value.filter((task) => task.completed).length
)

const filteredTasks = computed(() => {
  if (activeFilter.value === 'Open') {
    return tasks.value.filter((task) => !task.completed)
  }

  if (activeFilter.value === 'Afgerond') {
    return tasks.value.filter((task) => task.completed)
  }

  return tasks.value
})

function addTask() {
  const title = newTask.value.trim()

  if (!title) return

  tasks.value.push({
    id: `${Date.now()}-${Math.random().toString(16).slice(2)}`,
    title,
    priority: newPriority.value,
    completed: false,
  })

  newTask.value = ''
  newPriority.value = 'Middel'
}

function deleteTask(taskId) {
  tasks.value = tasks.value.filter((task) => task.id !== taskId)
}

function updateTaskCompletion(taskId, completed) {
  const task = tasks.value.find((item) => item.id === taskId)

  if (task) {
    task.completed = completed
  }
}

watch(
  tasks,
  (newTasks) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(newTasks))
  },
  { deep: true }
)
</script>

<template>
  <main class="min-h-screen bg-slate-100 px-4 py-10 text-slate-800 sm:px-6">
    <div class="mx-auto max-w-3xl">
      <header class="mb-8 text-center">
        <p class="mb-2 text-sm font-semibold uppercase tracking-[0.2em] text-indigo-600">
          Mijn overzicht
        </p>
        <h1 class="text-3xl font-bold tracking-tight text-slate-900 sm:text-4xl">
          Mijn to-do lijst
        </h1>
        <p class="mt-3 text-slate-500">
          Hier kun je al je taken bijhouden en filteren op open of afgerond.
        </p>
      </header>

      <section class="mb-6 grid grid-cols-2 gap-3 sm:grid-cols-3">
        <div class="rounded-2xl bg-white p-4 shadow-sm">
          <p class="text-sm text-slate-500">Totaal</p>
          <p class="mt-1 text-2xl font-bold text-slate-900">{{ tasks.length }}</p>
        </div>
        <div class="rounded-2xl bg-white p-4 shadow-sm">
          <p class="text-sm text-slate-500">Open taken</p>
          <p class="mt-1 text-2xl font-bold text-indigo-600">{{ openTaskCount }}</p>
        </div>
        <div class="col-span-2 rounded-2xl bg-white p-4 shadow-sm sm:col-span-1">
          <p class="text-sm text-slate-500">Afgerond</p>
          <p class="mt-1 text-2xl font-bold text-emerald-600">{{ completedTaskCount }}</p>
        </div>
      </section>

      <section class="rounded-2xl bg-white p-5 shadow-sm sm:p-7">
        <h2 class="text-lg font-semibold text-slate-900">Nieuwe taak</h2>

        <div class="mt-4 flex flex-col gap-3 sm:flex-row">
          <label class="sr-only" for="new-task">Nieuwe taak</label>
          <!-- met v-model koppel ik een invoerveld aan een variabele in Vue.-->
          <input
            id="new-task"
            v-model="newTask" 
            type="text"
            maxlength="120"
            placeholder="Wat moet je nog doen?"
            class="min-w-0 flex-1 rounded-xl border border-slate-200 px-4 py-3 text-sm outline-none transition placeholder:text-slate-400 focus:border-indigo-500 focus:ring-4 focus:ring-indigo-100"
            @keyup.enter="addTask"
          />

          <label class="sr-only" for="new-priority">Prioriteit</label>
          <select
            id="new-priority"
            v-model="newPriority"
            class="rounded-xl border border-slate-200 bg-white px-3 py-3 text-sm outline-none focus:border-indigo-500 focus:ring-4 focus:ring-indigo-100"
          >
            <option v-for="priority in priorities" :key="priority" :value="priority"><!--Ik gebruik v-for om alle taken uit mijn 
              takenlijst automatisch weer te geven. Voor iedere taak maakt Vue een TodoItem-component aan.
              Zo hoef ik niet voor iedere taak afzonderlijk code te schrijven-->
              {{ priority }}
            </option>
          </select>

          <button
            type="button"
            class="rounded-xl bg-indigo-600 px-5 py-3 text-sm font-semibold text-white transition hover:bg-indigo-700 focus:outline-none focus:ring-4 focus:ring-indigo-200"
            @click="addTask"
          >
            Taak toevoegen
          </button>
        </div>
        <p class="mt-2 text-xs text-slate-400">Kies een prioriteit en voeg je taak toe.</p>
      </section>

      <section class="mt-6 rounded-2xl bg-white p-5 shadow-sm sm:p-7">
        <div class="flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-between">
          <div>
            <h2 class="text-lg font-semibold text-slate-900">Mijn taken</h2>
            <p class="mt-1 text-sm text-slate-500">
              Je hebt nog {{ openTaskCount }} {{ openTaskCount === 1 ? 'taak' : 'taken' }} open.
            </p>
          </div>

          <div class="flex flex-wrap gap-2" aria-label="Taken filteren">
            <button
              v-for="filter in filters"
              :key="filter"
              type="button"
              class="rounded-full px-4 py-2 text-sm font-medium transition focus:outline-none focus:ring-4 focus:ring-indigo-100"
              :class="activeFilter === filter
                ? 'bg-indigo-600 text-white'
                : 'bg-slate-100 text-slate-600 hover:bg-slate-200'"
              :aria-pressed="activeFilter === filter"
              @click="activeFilter = filter"
            >
              {{ filter }}
            </button>
          </div>
        </div>
          
        <div v-if="filteredTasks.length === 0" class="mt-6 rounded-xl border border-dashed border-slate-200 px-4 py-10 text-center">
          <p class="font-medium text-slate-700">
            {{ tasks.length === 0 ? 'Nog geen taken toegevoegd' : 'Geen taken gevonden' }}
          </p>
          <p class="mt-1 text-sm text-slate-400">
            {{ tasks.length === 0 ? 'Voeg hierboven je eerste taak toe.' : 'Kies een ander filter om je taken te bekijken.' }}
          </p>
        </div>

        <ul v-else class="mt-6 space-y-3">
          <TodoItem
            v-for="task in filteredTasks"
            :key="task.id"
            :task="task"
            @delete="deleteTask"
            @update-completed="updateTaskCompletion"
          />
        </ul>
      </section>

      <p class="mt-5 text-center text-xs text-slate-400">
        Je taken worden automatisch bewaard op dit apparaat.
      </p>
    </div>
  </main>
</template>
// merdere compneten