<script setup>
//importaciones, props y emits
import { ref } from "vue"
let props = defineProps(['keys'])
const emit = defineEmits(["printNewNote", "closeBS"])

//variables
let inputTitle = ref('')
let inputContent = ref('')
let keys = props.keys

//funcionalidades
let lastNoteId = keys[keys.length - 1] || 0
function createNewNote() {
  lastNoteId++
  let note = {
    id: lastNoteId,
    checked: false,
    content: inputContent.value,
    title: inputTitle.value
    
  }
  //guarda en la localStorage
  localStorage.setItem(lastNoteId, JSON.stringify(note))
  //emite la funcion printNewNote en el padre, pasandole la nueva nota
  emit ('printNewNote', note)
  emit ("closeBS")
  inputTitle.value=''
  inputContent.value=''
}
function autoResizeTextarea(e){
  const textarea = e.target
  textarea.style.height = 'auto';
  textarea.style.height = textarea.scrollHeight + 'px';
}
</script>
<template>
  <form id="bottom-sheet">
    <span @click="$emit('closeBS')" class="icon-close"></span>
    <input v-model="inputTitle" type="text" id="input-create-note-title" placeholder="Nuevo Título" required
      autocomplete="off">
    <textarea @input="autoResizeTextarea" v-model="inputContent" id="input-create-note-content" placeholder="Descripción" rows="1"></textarea>
    <button @click.prevent="createNewNote" type="submit" id="btn-create-new">Crear
    </button>
  </form>
</template>

<style>
#bottom-sheet {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  background-color: #B6B0FB;
  border-radius: 12px 12px 0 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  gap: 10px;
}

#bottom-sheet>* {
  appearance: none;
}

#bottom-sheet .icon-close {
  width: 20px;
  height: 20px;
  margin-left: auto;
  background: url(../assets/icons8-multiplicar-32.png);
  background-position: center;
  background-size: contain;
}

#bottom-sheet [type=text],
#bottom-sheet textarea {
  appearance: none;
  width: 100%;
  border: none;
  padding: 10px 15px;
  font-family: "Heebo", sans-serif;
  border-radius: 24px;
  color: #201d3a;
  background-color: #dfdcfe;
  color: #201d3a;
}

#bottom-sheet [type=text]::placeholder,
#bottom-sheet textarea::placeholder {
  font-style: italic;
  color: #aaa6c4;
}

#bottom-sheet [type=text]#input-create-note-title,
#bottom-sheet textarea#input-create-note-title {
  font-size: smaller;
  font-weight: 600;
}

#bottom-sheet [type=text]#input-create-note-content,
#bottom-sheet textarea#input-create-note-content {
  font-size: smaller;
  font-weight: 400;
  color: #807f89;
}

#bottom-sheet #btn-create-new {
  padding: 6px 6px;
  appearance: none;
  font-family: "Heebo", sans-serif;
  font-weight: 400;
  border-radius: 20px;
  border: none;
  display: flex;
  background-color: #8B80EF;
  color: white;
  width: max-content;
  padding-left: 15px;
  padding-right: 15px;
  align-items: center;
}

</style>