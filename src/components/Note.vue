<script setup>
import { ref, onMounted } from "vue"

let exist = ref(true)
const props = defineProps(["note"])
const note = ref(props.note)
let dataEditable = ref(true)
let inputTitle = ref(note.value.title)
let inputContent = ref(note.value.content)
const textarea = ref(null)
let textareaScrollHeight = ref(0)

/* let computedHeight = ref(getHeightTextarea()) */
//cambio el valor de checked de string a booleano
note.value.checked = note.value.checked === "true" || note.value.checked === true

onMounted(() => {
    textareaScrollHeight.value = textarea.value.scrollHeight
    dataEditable.value = false
})


function toggleCheckedProperty() {
    note.value.checked = !note.value.checked
    localStorage.setItem(note.value.id, JSON.stringify(note.value))
}
function changeDataEditable(event) {
    if (event.target.tagName != "INPUT" && dataEditable.value == false) {
        dataEditable.value = true
        textarea.value.style.height = "auto"
        textarea.value.style.height = (textareaScrollHeight.value + 5)  + 'px'
    }
}
function save() {
    note.value.title = inputTitle
    note.value.content = inputContent
    localStorage.setItem(note.value.id, JSON.stringify(note.value))
    dataEditable.value = false
}
function eliminate() {
    localStorage.removeItem(note.value.id)
    exist.value = false

}
function resizeTextarea() {
    textarea.value.style.height = "auto"
    textarea.value.style.height = textarea.value.scrollHeight + 'px'
    textareaScrollHeight.value = textarea.value.scrollHeight
}
</script>
<template >
    <div v-if="exist" id="note" @click.stop="changeDataEditable" :data-id="note.id" :data-editable="dataEditable"
        :data-checked="note.checked">
        <!-- cuando no es editable -->
        <span v-show="dataEditable === false">
            <div>
                <h2 class="title">{{ note.title }}</h2>
                <input type="checkbox" class="checkbox" :checked="note.checked" @click.stop="toggleCheckedProperty">
            </div>
            <p class="content">{{ note.content || "" }}</p>
        </span>
        <!-- cuando es editable -->
        <span v-show="dataEditable === true">
            <input v-model="inputTitle" type="text" placeholder="Titulo" class="title">
            <textarea @input="resizeTextarea" ref="textarea" v-model="inputContent" id="textarea-editable-note"
                placeholder="Descripción" rows="1" class="content">{{ note.content }}</textarea>
            <div class="buttons">
                <button class="btn remove" @click.stop="eliminate">
                    <p>Eliminar</p>
                    <span class="icon remove"></span>
                </button>
                <button class="btn save" @click.stop="save">
                    <p>Guardar</p>
                    <span class="icon save"></span>
                </button>
                <button class="btn cancel" @click.stop="dataEditable = false">
                    <p>Cancelar</p>
                    <span class="icon cancel"></span>
                </button>
            </div>
        </span>
    </div>
</template>
<style>
#note {
    word-wrap: break-word;
    background-color: #dfdcfe;
    border-radius: 15px;
    padding: 15px;
}

#note div {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

#note div h2 {
    color: #8B80EF;
}

#note div input[type=checkbox] {
    appearance: none;
    cursor: pointer;
    width: 20px;
    height: 20px;
    border: 2px solid #A8A5C6;
    border-radius: 3px;
    background-color: #d4d1ee;
    background-position: center;
    background-size: cover;
}

#note div input[type=checkbox]:checked {
    appearance: none;
    border: 2px solid #8B80EF;
    background-color: #8B80EF;
    background-image: url(../assets/icons8-marca-de-verificación-64.png);
}

#note p {
    margin-top: 6px;
    color: #464453;
    white-space: pre-line;
}

/* cuando está completada */
#note[data-checked="true"][data-editable="false"] {
    background-color: #B6B0FB;
}

#note[data-checked="true"][data-editable="false"] h2,
#note[data-checked="true"][data-editable="false"] p {
    text-decoration: line-through;
}

#note[data-checked="true"][data-editable="false"] h2 {
    color: #8B80EF;
}

/* cuando es editable */
#note[data-editable="true"] {
    display: flex;
    flex-direction: column;
    gap: 6px;
    background-color: #B6B0FB;
}

#note[data-editable="true"] [type=text],
#note[data-editable="true"] textarea {
    resize: none; 
    overflow-y: hidden;
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

#note[data-editable="true"] [type=text]::placeholder,
#note[data-editable="true"] textarea::placeholder {
    font-style: italic;
    color: #aaa6c4;
}

#note[data-editable="true"] [type=text].title,
#note[data-editable="true"] textarea.title {
    font-size: smaller;
    font-weight: 600;
}

#note[data-editable="true"] [type=text].content,
#note[data-editable="true"] textarea.content {
    font-size: smaller;
    font-weight: 400;
    color: #807f89;
}

#note[data-editable="true"] div.buttons {
    margin-top: 6px;
}

#note[data-editable="true"] div.buttons button {
    padding: 6px 6px;
    appearance: none;
    font-family: "Heebo", sans-serif;
    font-weight: 400;
    border-radius: 20px;
    border: none;
    display: flex;
    gap: 6px;
    padding: 6px 12px;
    align-items: center;
}

#note[data-editable="true"] div.buttons button p {
    color: white;
    margin: 0;
}

#note[data-editable="true"] div.buttons button span.icon {
    display: block;
    width: 20px;
    height: 20px;
    background-position: center;
    background-size: contain;
}

#note[data-editable="true"] div.buttons button span.icon.remove {
    background-image: url(../assets/icons8-basura-32.png);
}

#note[data-editable="true"] div.buttons button span.icon.save {
    background-image: url(../assets/icons8-guardar-32.png);
}

#note[data-editable="true"] div.buttons button span.icon.cancel {
    background-image: url(../assets/icons8-multiplicar-32.png);
}

#note[data-editable="true"] div.buttons button.remove {
    background-color: #ff8a8a;
}

#note[data-editable="true"] div.buttons button.save {
    background-color: #8B80EF;
}

#note[data-editable="true"] div.buttons button.cancel {
    background-color: #dfdcfe;
}

#note[data-editable="true"] div.buttons button.cancel p {
    color: #8B80EF;
    font-weight: 600;
}
</style>