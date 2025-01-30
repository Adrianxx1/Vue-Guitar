<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Guitarra from './components/Guitarra.vue';
import { db } from './data/guitarras.js'

const guitars = ref(db)
const carrito = ref([])
const total = computed(() => {
  // let total = 0;
  // carrito.value.forEach((guitarra) => {
  //   total += guitarra.cantidad * guitarra.precio
  // })
  // return total
  return carrito.value.reduce((total, guitarra) => total + guitarra.cantidad * guitarra.precio, 0)
})

function agregarCarrito(guitar) {
  const guitarId = carrito.value.findIndex((guitarra) => guitarra.id === guitar.id)
  if (guitarId === -1)
    carrito.value.push({ ...guitar, cantidad: 1 })
  else
    carrito.value[guitarId].cantidad++
}
function quitaUno(id) {
  const guitarId = carrito.value.findIndex((guitarra) => guitarra.id === id)
  if (carrito.value[guitarId].cantidad > 1)
    carrito.value[guitarId].cantidad--
  else
    carrito.value = carrito.value.filter((guitarra) => guitarra.id !== id)
}

function quitaGuitarra(id) {
  carrito.value = carrito.value.filter((guitarra) => guitarra.id !== id)
}

function vaciarCarrito() {
  carrito.value = []
}

function guardarStorage() {
  localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

function recuperaStorage() {
  const datos = localStorage.getItem('carrito')
  return datos ? JSON.parse(datos) : []
  // carrito.value = JSON.parse(localStorage.getItem('carrito')) ?? []
}

onMounted(() => {
  carrito.value = recuperaStorage()
})

watch(carrito, guardarStorage, { deep: true })

</script>

<template>
  <Header :carrito="carrito" :total="total" :guitar="guitars[3]" @agregar-carrito="agregarCarrito" @quita-uno="quitaUno"
    @quita-guitarra="quitaGuitarra" @vaciar-carrito="vaciarCarrito" />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitarra v-for="guitar in guitars" :key="guitar.id" :guitar="guitar" @agregar-carrito="agregarCarrito" />
    </div>
  </main>
  <Footer />
</template>