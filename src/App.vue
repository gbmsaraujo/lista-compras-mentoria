<template lang="">
  <div>
    <h1>
      Lista de Compras
    </h1>

    <p>Minha Lista</p>


    <div>
      <label for="inserir">Insira um Item na Lista: </label>
      <input type="text" id="inserir" v-model="itemToAdd" @keyup.enter="addItemToList">
    </div>

    <button @click="addItemToList">Adicionar</button>

    <ul class="itens">
      <li v-for="(item, index) in listaCompras" :key="index">
        {{item}}
        <span class="edit" @click="setToEdit(index)">✏️</span>
        <span class="remove" @click="removeValue(index)">🗑️</span>
        <div  v-show="indexToEdit===index">
          <input type="text" v-model="itemEdited"> 
          <span class="check" @click="editValue(index)" >✅</span></div>
      </li>

    </ul>



    <p v-show="listaCompras.length === 0"> Nenhum item na lista! </p>

    <!-- <p v-if="numero < 4"> Numero é menor que 4</p>
    <p v-else-if="numero>4"> Numero Maior que 4</p>
    <p v-else>Numero Igual a 4</p> -->

    <!-- <p v-show="numero<4">Essa tag será mostrada</p> -->



  </div>
</template>

<script setup>
import { onMounted, ref, watch } from "vue";
// Add item ok
// Primeira letra da lista seja em Maiuscula - ok
// Evitar de add item Repetido 
// Remover item da lista de compras ok
// Editar item da lista ok
// Mostrar uma mensagem Lista vazia quando ela não tiver itens - ok

const name = ref("Gabriel");

const capitalizar = (texto) => {
  return texto.charAt(0).toUpperCase() + texto.slice(1).toLowerCase();
};

const itemToAdd = ref("");
const indexToEdit = ref("")
const itemEdited = ref("");
const listaCompras = ref([]);

onMounted(() => {
  const savedList = localStorage.getItem("listaCompras");
  if (savedList) {
    listaCompras.value = JSON.parse(savedList);
  }
});

watch(listaCompras, (newValue) => {
  localStorage.setItem("listaCompras", JSON.stringify(newValue));
}, { deep: true });

const addItemToList = () => {
  if (!itemToAdd.value) {
    return;
  }

  const newItem = capitalizar(itemToAdd.value);

  if (!listaCompras.value.includes(newItem)) {
    listaCompras.value.push(newItem);
    itemToAdd.value = "";
  } else {
    console.log(listaCompras.value)
    alert("Este item já está na lista!");
  }
};

const setToEdit = (indexItem) => {
  indexToEdit.value = indexItem
};

const editValue = (selectedIndex) => {
  listaCompras.value.splice(selectedIndex, 1, capitalizar(itemEdited.value))
  isEdit.value = false
  itemEdited.value = ""
};

const removeValue = (selectedIndex) => {
  const filterList = listaCompras.value.filter((value, index) => selectedIndex !== index)
  listaCompras.value = filterList
}
</script>

<style>
.edit {
  cursor: pointer;
  margin-left: 5px;
}

.remove {
  cursor: pointer;
  margin-left: 15px;
}

.check {
  cursor: pointer;
}
</style>