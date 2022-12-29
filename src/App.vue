<script setup>
import { ref } from "vue"

const showModal = ref(false)
const newNote = ref("")
const errorMessage = ref("")
const notes = ref([])

const addNote = () => {
  if (newNote.value.length < 3) {
    return errorMessage.value = "A note needs to have at least 3 characters."
  }

  notes.value.push({
    id: Math.floor(Math.random() * 100000),
    text: newNote.value,
    date: new Date(),
  })

  showModal.value = false
  newNote.value = ""
  errorMessage.value = ""
}

const deleteNote = (note) => {
  notes.value = notes.value.filter(n => n !== note)
}
</script>

<template>
  <main class="flex justify-center items-center">
    <div class="w-4/5 xl:w-2/4 mx-auto">
      <div class="flex justify-between items-center my-4">
        <h1 class="text-3xl font-bold">Notes</h1>
        <button @click="showModal = true" class=" text-3xl focus:scale-90 hover:rotate-[360deg] duration-300">🖋️</button>
      </div>

      <div class="flex flex-wrap gap-6 my-14">
        <div v-for="note in notes" :key="note.id" class="flex flex-col justify-between w-60 h-60 p-4 bg-slate-100 text-slate-700 dark:bg-sky-900 dark:text-slate-400 rounded">
          <div>
            <p class="text-sm italic opacity-50 mb-2">{{ note.date.toLocaleDateString("fr-FR") }}</p>
            <p class="main-text">{{ note.text }}</p>
          </div>

          <div class="flex justify-between">
            <p class="opacity-50">#{{ note.id }}</p>
            <button @click="deleteNote(note)" class="flex self-end text-red-100 bg-red-500 w-fit px-2 rounded-full hover:saturate-150 duration-150">delete</button>
          </div>
        </div>
      </div>
    </div>

    <div v-if="showModal" class="fixed bottom-0 p-0 w-full border-t-[1px] border-sky-300 backdrop-blur-md">
      <div class="flex flex-col justify-center items-center">
        <textarea v-model.trim="newNote" @keypress.enter="addNote" name="note" id="note" class="font-medium w-80 h-80 p-4 outline-none bg-transparent"></textarea>
        <p v-if="errorMessage" class="text-red-500 mt-2">{{ errorMessage }}</p>
        <button @click="addNote" class="my-4 mx-auto py-1 px-5 w-fit bg-sky-500 text-slate-100 rounded-full hover:saturate-150 duration-150">Create note</button>
      </div>
    </div>
  </main>
</template>