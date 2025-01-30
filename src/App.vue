<script setup>
import { ref } from 'vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Guitarra from './components/Guitarra.vue';
import { db } from './data/guitarras';

const guitars = ref(db)
const carrito = ref([])

function agregarCarrito(guitar){
    const guitarId = carrito
    .value.findIndex(g => g.id === guitar.id)
  if(guitarId === -1 )
    carrito.value.push({...guitar, cantidad: 1 })
  else
    carrito.value[guitarId].cantidad++
}

function quitaUno(id) {
  const guitarraId = carrito.value.findIndex(g => g.id === id);
  if (guitarraId !== -1) {
    if (carrito.value[guitarraId].cantidad > 1) {
      carrito.value[guitarraId].cantidad--;
    } else {
      carrito.value.splice(guitarraId, 1);
    }
  }
}

function quitaGuitarra(id) {
  const guitarraId = carrito.value.findIndex(g => g.id === id);
  if (guitarraId !== -1) {
    carrito.value.splice(guitarraId, 1);
  }
}

function vaciarCarrito() {
  carrito.value = [];
}
</script>

<template>
  <Header
    :carrito="carrito"
    @agregar-carrito="agregarCarrito"
    @quita-uno="quitaUno"
    @quita-guitarra="quitarGuitarra"
    @vaciar-carrito=""vaciarCarrito

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