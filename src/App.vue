<script setup>
//set state
import {ref} from "vue";

const showModal = ref(false);
// two way binding within textarea
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);

// give each card a different color
function getRandomColor(){
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  
}
// create a handler
const addNote = () => {
  if (newNote.value.length < 10){
    return errorMessage.value = "Note to be 10 carachters or more";
  }

  notes.value.push({
    id : Math.floor(Math.random() * 1000000),
    text : newNote.value,
    date: new Date() ,
    backgroundColor:  getRandomColor()
 });
 // automatically close the note
 showModal.value = false;
 newNote.value= "";
 errorMessage.value = "";
}

</script>

<template>
  <main>
    <!--v-if directive will render parent element as well as all children while showModel is true-->
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage">{{errorMessage}}</p>
        <button @click="addNote">Add note</button>
        <button class="close" @click="showModal = false"> close</button>
      </div>
    </div>
    
    <div class="container"> 
      <header>
        <h1> Notes </h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <!--Iterate over notes array and render html elemts 4 each partcular note-->
        <div v-for="note in notes" 
        :key="note.id"
        class="card" :style="{backgroundColor: note.backgroundColor}">
          <p class="main-text">{{note.text}}</p>
          <p class="date">{{note.date.toLocalDateString("en-US")}}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main{
  height: 100vh;
  width: 100vw;
}
.container{
  max-width: 1000px;
  padding: 10;
  margin: 0 auto;
}

header{
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h1{
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;

}

header button{
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card{
  width: 225;
  height: 225;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date{
  font-size:12,5 ;
  font-weight: bold;
}

.cards-container{
  display: flex;
  flex-wrap: wrapgi;
}

.overlay{
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0 ,0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal{
  width: 750px;
  border-color: azure;
  border-radius: 10px;
  padding: 30px;
  position: relative ;
  display: flex;
  flex-direction: column;
}

.modal button{
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}
.modal .close{
  background-color: red;
  margin-top: 7px;
}
</style>