<script setup>
//Via props ontvangt het de taakgegevens van App.vue. 
//Via emit stuurt het gebeurtenissen terug wanneer ik een taak afvink of verwijder.
const props = defineProps({
  task: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['delete', 'update-completed'])

function changeCompletion(event) {
  emit('update-completed', props.task.id, event.target.checked)
}

function removeTask() {
  emit('delete', props.task.id)
}

const priorityStyles = {
  Hoog: 'bg-rose-50 text-rose-700',
  Middel: 'bg-amber-50 text-amber-700',
  Laag: 'bg-emerald-50 text-emerald-700',
}
</script>

<template>
  <li class="flex items-center gap-3 rounded-xl border border-slate-100 bg-slate-50 p-3 sm:p-4">
    <input
      type="checkbox"
      :checked="task.completed"
      :aria-label="`Markeer ${task.title} als ${task.completed ? 'open' : 'afgerond'}`"
      class="h-5 w-5 shrink-0 cursor-pointer rounded border-slate-300 text-indigo-600 focus:ring-2 focus:ring-indigo-300"
      @change="changeCompletion"
    />

    <div class="min-w-0 flex-1">
      <p
        class="break-words text-sm font-medium"
        :class="task.completed ? 'text-slate-400 line-through' : 'text-slate-700'"
      >
        {{ task.title }}
      </p>
      <span
        class="mt-1 inline-flex rounded-full px-2.5 py-1 text-xs font-medium"
        :class="priorityStyles[task.priority] || priorityStyles.Middel"
      >
        {{ task.priority }}
      </span>
    </div>

    <button
      type="button"
      class="shrink-0 rounded-lg px-3 py-2 text-xs font-semibold text-rose-600 transition hover:bg-rose-50 focus:outline-none focus:ring-4 focus:ring-rose-100"
      :aria-label="`Verwijder ${task.title}`"
      @click="removeTask"
    >
      Verwijderen
    </button>
  </li>
</template>
// Een component is een apart onderdeel van mijn applicatie.
// Ik gebruik TodoItem.vue om iedere taak afzonderlijk weer te geven.
// Hierdoor blijft mijn code overzichtelijk en hoef ik niet alles in één bestand te zetten.
