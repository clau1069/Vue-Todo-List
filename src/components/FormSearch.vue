<script setup>
import { ref } from "vue"
const input = ref()
const checked = ref(false)
//"$emit('getInputValues', input, checkbox)"

</script>

<template>
    <form id="form-search" >
        <div class="contenedor-search">
            <input @input="$emit('getInputValues', input, checked)" v-model="input" type="text" placeholder="buscar" >
            <button type="submit"></button>
        </div>
        <div class="filter">
            <p>Solo completados</p>
            <input @change="$emit('getInputValues', input, checked)" v-model="checked" type="checkbox" >
        </div>
    </form>
</template>

<style>
#form-search {
    display: flex;
    flex-direction: column;
}

.contenedor-search {
    margin-left: 20px;
    margin-right: 20px;
    position: relative;
    top: 0;
    left: 0;
}

.contenedor-search input {
    appearance: none;
    width: 100%;
    border: none;
    padding: 10px 15px;
    font-family: "Heebo", sans-serif;
    border-radius: 24px;
    color: #201d3a;
    background-color: #dfdcfe;
}

.contenedor-search input::placeholder {
    font-style: italic;
    color: #aaa6c4;
}

.contenedor-search button {
    position: absolute;
    top: 0;
    right: 0;
    margin-right: 10px;
    width: 20px;
    height: 100%;
    border: none;
    background: url(../assets/icons8-búsqueda-32.png);
    background-position: center;
    background-size: contain;
    background-repeat: no-repeat;
}
/* .filter {
  display: flex;
  margin: 6px 23px 15px auto;
  align-items: center;
}
.filter p {
  margin-right: 6px;
  color: #464453;
  font-size: small;
}
.filter input {
  appearance: none;
  cursor: pointer;
  width: 15px;
  height: 15px;
  border: 2px solid #A8A5C6;
  border-radius: 3px;
  background-color: #d4d1ee;
  background-position: center;
  background-size: cover;
}
.filter input:checked {
  appearance: none;
  border: 2px solid #8B80EF;
  background-color: #8B80EF;
  background-image: url(../assets/icons8-marca-de-verificación-64.png);
} */
</style>

