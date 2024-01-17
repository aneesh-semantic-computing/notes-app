<script setup>
import { ref } from "vue";
const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);
localStorage.getItem('notes') && notes.value.push(...JSON.parse(localStorage.getItem('notes')));
const errorMessage = ref("");

/**
 * Adds a note to the notes array and saves it to localStorage.
 *
 * @return {undefined} There is no return value.
 */
const addNote = () => {
  if(newNote.value.length < 10) {
    return errorMessage.value = "Note needs to be at least 10 characters long";
  }
  notes.value.push({
    id: Math.floor(Math.random() * 10000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: "hsl(" + Math.random() * 360 + ", 100%, 75%)",
  });
  localStorage.setItem('notes', JSON.stringify(notes.value));
  newNote.value = '';
  showModal.value = false;
  errorMessage.value = '';
}

/**
 * Removes a note from the notes array and updates the localStorage.
 *
 * @param {number} id - The id of the note to be removed.
 * @return {undefined} This function does not return a value.
 */
const removeNote = (id) => {
  // notes.value.splice(notes.indexOf(note.value), 1);
  notes.value = notes.value.filter((note) => note.id !== id);
  localStorage.setItem('notes', JSON.stringify(notes.value));
}
</script>
<template>
  <main>
    <div class="container">
      <div v-if="showModal" class="overlay">
        <div class="modal">
          <p @click="(newNote = ''); (errorMessage = ''); showModal = false">x</p>
          <textarea name="note" id="note" cols="30" rows="10" v-model.trim="newNote"></textarea>
          <p v-if="errorMessage">{{errorMessage}}</p>
          <button @click="addNote">Add note</button>
        </div>
      </div>
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div class="card" v-for="note in notes" :key="note.id" :style="{backgroundColor: note.backgroundColor}">
          <p @click="removeNote(note.id)" class="close-button">x</p>
          <p class="main-text">{{note.text}}</p>
          <p class="date">{{new Date(note.date).toLocaleString('en-gb')}}</p>
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
}
h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}
header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: #f8f8f8;
  border-radius: 100%;
  color: black;
  font-size: 20px;
}
.cards-container {
  display: flex;
  flex-wrap: wrap;
}
.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182,44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
  color: black;
}
.date {
  font-size: 12.5px;
  font-weight: bold;
}
.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgb(255, 255, 255, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}
.container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto
  }

  h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
  }

  .card {
    width: 225px;
    height: 225px;
    background-color: rgb(237, 182, 44);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
  }

  .card .close-button {
    color: black;
    margin-left: auto;
    font-size: 20px;
    z-index: 100000;
    cursor: pointer;
  }

  .main-text {
    line-height: 1.25;
    font-size: 17.5px;
    font-weight: bold;
  }

  .date {
    font-size: 12.5px;
    margin-top: auto;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(21, 20, 20);
    border-radius: 1000px;
    color: white;
    font-size: 20px;
  }
  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    transform: translate(-50%, -50%);
    top: 50%;
    left: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 10;
  }

  main {
    height: 100vh;
    width: 100vw;
  }

  .modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }

  .modal button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;

  }

  .modal .close-button {
    background-color: rgb(193, 15, 15);
    margin-top: 7px;
    color: white;
  }

  .modal p {
    margin-left: auto;
    font-size: 20px;
    z-index: 100000;
    cursor: pointer;
  }

  textarea {
    width: 100%;
    height: 200px;
    padding: 5px;
    font-size: 20px;
  }
</style>