<template>
  <main>
    <div v-if="modal" class="overlay">
      <div class="modal">
        <p style="color: red; font-weight: bolder" v-if="errorMessage">
          {{ errorMessage }}
        </p>
        <textarea
          v-model.trim="newNote"
          name="notes"
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <button class="add-note-button" @click="addNote">Add note</button>
        <button @click="toggleModal" class="close">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="toggleModal">+</button>
      </header>
      <div class="cards-container">
        <div
          class="card"
          v-for="note in savedNotes"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
import { ref } from "vue";
interface Note {
  id: number;
  text: string;
  date: string;
  backgroundColor: any;
}

const modal = ref<any>(false);
const toggleModal = () => {
  modal.value = !modal.value;
  newNote.value = "";
  errorMessage.value = "";
};

const getRandomColor = () => {
  const color = "hsl(" + Math.random() * 360 + ", 100%, 75%";
  return color;
};

const newNote = ref<any>();
const savedNotes = ref<Note[]>([]);
const errorMessage = ref<string>("");

const addNote = () => {
  if (newNote.value.length < 1) {
    return (errorMessage.value = "Empty textarea can't be added to notes");
  }
  savedNotes.value!.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date().toLocaleDateString(),
    backgroundColor: getRandomColor(),
  });
  toggleModal();
  errorMessage.value = "";
};
</script>

<style scoped>
main {
  height: 100vh;
  width: 100wv;
}
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}
header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
h1 {
  font-weight: bold;
  font-size: xx-large;
}
header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: black;
  border-radius: 100%;
  color: white;
  font-size: large;
}
.cards-container {
  display: flex;
  flex-wrap: wrap;
}
.card {
  width: 225px;
  height: fit-content;
  padding: 10px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin: 10px;
  border: 2px solid rgba(0, 0, 0, 0.5);
}
.main-text {
  word-break: break-all;
}
.card p {
  font-size: large;
  width: 100%;
}
.date {
  margin-top: 20px;
  font-size: 14px !important;
  font-weight: bold;
}
.overlay {
  position: absolute;
  z-index: 10;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal {
  width: 500px;
  background-color: white;
  border-radius: 10px;
  padding: 20px;
  position: relative;
  display: flex;
  flex-direction: column;
}
.modal button {
  padding: 10px;
  width: 100%;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 10px;
  background-color: darkgreen;
}

.modal .close {
  background-color: red !important;
  margin-top: 5px;
  cursor: pointer;
}
</style>
