<script setup>
//importaciones
import FormSearch from './components/FormSearch.vue'
import BottomSheet from './components/FormBottomSheet.vue'
import Note from './components/Note.vue'
import { ref } from "vue"

//variables
let notes = ref([])
let showBottomSheet = ref(false)
let localStorageKeys = Object.keys(localStorage).sort(function (a, b) {
  return a - b
}) //obtiene las keys de la local Storage y las ordena de menor a mayor
let inputSearch = ref('')
let inputCheckbox = ref()

//funcionalidades
//guarda en un array las notas que estaban en la localSotare (en formato de objeto)
localStorageKeys.forEach((id) => {
  //obtengo la nota de la local Storage y lo paso a formato de objeto. A ese objeto le sumo el id
  let noteObject = JSON.parse(localStorage.getItem(id))
  noteObject.id = id
  //lo sumo a notes[]
  notes.value.push(noteObject)
})

function vShowNote(note){
  if(inputCheckbox.value == false && inputSearch == ''){return true}
  else if (inputCheckbox.value == true && note.checked == false){return false}
  return note.title.toLowerCase().includes(inputSearch.value.toLowerCase()) || note.content.toLowerCase().includes(inputSearch.value.toLowerCase()) 

}

</script>

<template>
  <header>
    <img src="./assets/icons8-koala-50.png" alt="" class="icono-koala">
    <h1>Koala List App</h1>
  </header>
  <main>
    <!-- buscador -->
    <FormSearch @getInputValues="(input, checked)=>{inputSearch = input; inputCheckbox = checked}"/>
    <!-- contenedor -->
    <div id="containter-scroll">
      <div id="container-notes">
        <template  
        v-for="note in notes" 
        :key="note.id">
          <Note 
          :note="note"
          v-show="vShowNote(note)"
          ></Note>
        </template>
        
      </div>
    </div>
  </main>
      <!-- Floating Button -->
    <button v-show="showBottomSheet == false" id="floating-button" @click.stop="showBottomSheet = true"></button>
    <!--Bottom Sheet Nueva Nota-->
  <BottomSheet
    :keys="localStorageKeys"
    v-show="showBottomSheet == true"
    @closeBS="showBottomSheet = false"
    @printNewNote = "(note)=> notes.push(note)"
    />
  
</template>

<style>
#containter-scroll {
  flex-grow: 1;
  position: relative;
}

#containter-scroll #container-notes {
  position: absolute;
  height: 100%;
  width: 100%;
  overflow-y: scroll;
  display: flex;
  flex-direction: column;
  gap: 12px;
  
}
#floating-button {
  position: absolute;
  bottom: 20px;
  right: 20px;
  padding: 6px 6px;
  appearance: none;
  font-family: "Heebo", sans-serif;
  font-weight: 400;
  border-radius: 20px;
  border: none;
  width: 50px;
  height: 50px;
  border-radius: 60px;
  background: url(assets/icons8-más-32.png);
  background-color: #8B80EF;
  background-position: center;
  background-size: 30px;
  background-repeat: no-repeat;
  -webkit-box-shadow: 5px 5px 8px 0px rgba(0, 0, 0, 0.17);
  -moz-box-shadow: 5px 5px 8px 0px rgba(0, 0, 0, 0.17);
  box-shadow: 5px 5px 8px 0px rgba(0, 0, 0, 0.17);
}
#floating-button.disabled {
  display: none;
}
</style>