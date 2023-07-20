<script setup>
import { ref } from "vue";
import { db } from "./data/guitarras";
import { onMounted } from "vue";

import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
const guitarras = ref(db);
const guitarraHeader = ref(db[3]);
const carrito = ref([]);

onMounted(() => {
  const carritoStorage = localStorage.getItem("carrito");
  if (carritoStorage) {
    carrito.value = JSON.parse(carritoStorage);
  }
});

const setLocalStorage = () => {
  localStorage.setItem("carrito", JSON.stringify(carrito.value));
};
const agregarCarrito = (guitarra) => {
  const existeElemento = carrito.value.find(
    (producto) => producto.id === guitarra.id
  );
  if (!existeElemento) {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  } else {
    guitarra.cantidad++;
  }

  setLocalStorage();
};
const sumarCarrito = (guitarra) => {
  if (guitarra.cantidad >= 10) alert("Cantidad Máxima Permitida");
  else {
    guitarra.cantidad++;
  }
};
const restarCarrito = (guitarra) => {
  if (guitarra.cantidad <= 1) alert("Cantidad Minima Permitida");
  else {
    guitarra.cantidad--;
  }
};
const eliminarCarrito = (guitarra) => {
  carrito.value = carrito.value.filter(
    (producto) => producto.id != guitarra.id
  );
};
const vaciarCarrito = () => {
  carrito.value = [];
  alert("Carrito Vacío");
};
</script>

<template>
  <Header
    :carrito="carrito"
    :guitarraHeader="guitarraHeader"
    @agregar-carrito="agregarCarrito"
    @sumar-carrito="sumarCarrito"
    @restar-carrito="restarCarrito"
    @eliminar-carrito="eliminarCarrito"
    @vaciar-carrito="vaciarCarrito"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <div class="row mt-5">
      <Guitarra
        v-for="guitarra in guitarras"
        v-bind:guitarra="guitarra"
        @agregar-carrito="agregarCarrito"
      />
    </div>
  </main>
  <Footer />
</template>
