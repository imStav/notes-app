<script setup>
import { ref } from "vue"

const showModal = ref(false)
const newNote = ref("")
const errorMessage = ref("")
const notes = ref([])

const addNote = () => {
  if (newNote.value.length < 10) {
    return errorMessage.value = "A note needs to have at least 10 characters."
  }

  notes.value.push({
    id: Math.floor(Math.random() * 100000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: "black"
  })

  showModal.value = false
  newNote.value = ""
}
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p class="error" v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Create note</button>
      </div>
    </div>

    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>

      <div class="cards-container">
        <div v-for="note in notes" :key="note.id" class="card">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("fr-FR") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-block: 2rem;
}

h1 {
  font-weight: bold;
  font-size: 2.5rem;
}

header button {
  border: none;
  height: 2.5rem;
  width: 2.5rem;
  border-radius: 100%;
  background-color: black;
  color: whitesmoke;
  font-size: 1.2rem;
  font-weight: bold;
  cursor: pointer;
}

textarea {
  padding: 1rem 1.2rem;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 225px;
  height: 225px;
  background-color: black;
  color: whitesmoke;
  border-radius: 10px;
  padding: 1rem 1.5rem;
}

.date {
  font-weight: bold;
}

.overlay {
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: #00000046;
  z-index: 10;
}

.modal {
  display: flex;
  flex-direction: column;
  padding: 2rem;
  position: relative;
  width: 750px;
  background-color: whitesmoke;
  color: black;
  border-radius: 10px;
}

.modal button {
  padding: 0.5rem 1.5rem;
  background-color: black;
  color: whitesmoke;
  border: none;
  margin-top: 1rem;
  border-radius: 10px;
  cursor: pointer;
}

.modal .close {
  background-color: #de3131;
}

.error {
  color: #de3131;
}
</style>