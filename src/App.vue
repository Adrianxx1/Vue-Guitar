<script setup>
import { ref, computed } from 'vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Guitarra from './components/Guitarra.vue';
import { db } from './data/guitarras';

const guitars = ref(db);
const carrito = ref([]);

const totalCarrito = computed(() => {
  return carrito.value.reduce((total, producto) => total + producto.precio * producto.cantidad, 0);
});

function agregarCarrito(guitar) {
  const productoEnCarrito = carrito.value.find(p => p.id === guitar.id);
  if (productoEnCarrito) {
    productoEnCarrito.cantidad++;
  } else {
    carrito.value.push({ ...guitar, cantidad: 1 });
  }
}

function quitarUno(id) {
  const productoEnCarrito = carrito.value.find(p => p.id === id);
  if (productoEnCarrito) {
    productoEnCarrito.cantidad--;
    if (productoEnCarrito.cantidad === 0) {
      carrito.value = carrito.value.filter(p => p.id !== id);
    }
  }
}

function quitarGuitarra(id) {
  carrito.value = carrito.value.filter(p => p.id !== id);
}

function vaciarCarrito() {
  carrito.value = [];
}
</script>

<template>
  <Header
    :carrito="carrito"
    :total="totalCarrito"
    @agregar-carrito="agregarCarrito"
    @quita-uno="quitaUno"
    @quita-guitarra="quitarGuitarra"
    @vaciar-carrito="vaciarCarrito"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <div class="row mt-5">
      <Guitarra
        v-for="guitar in guitars"
        :key="guitar.id"
        :guitar="guitar"
        @agregar-carrito="agregarCarrito"
      />
    </div>
  </main>
  <Footer />
</template>