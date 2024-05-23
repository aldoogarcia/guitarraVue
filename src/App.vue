<script setup>
import { ref, onMounted } from 'vue'
import { db } from './data/guitarras'
import guitarraItem from './components/guitarra-item.vue'
import headerItem from './components/heder-item.vue'
import footerItem from './components/footer-item.vue'

//reactive se recomienda cuando se trabaja con objetos
// const state = reactive({
//   guitarras: [db]
// })
const guitarras = ref([])
const carrito = ref([])
onMounted(() => {
  guitarras.value = db
  // State.guitarras=db;
})
const agregarCarrito = (guitarra) => {
  const existeGuitarra = carrito.value.findIndex(estado => estado.id === guitarra.id)
  if (existeGuitarra >= 0) {
    carrito.value[existeGuitarra].cantidad++
  } else {
    guitarra.cantidad = 1
    carrito.value.push(guitarra)
  }
}

const aumentarCarrito=(id)=>{
  const index=carrito.value.findIndex(e => e.id===id);
  carrito.value[index].cantidad++;
}


const quitarCantidad=(id)=>{
  const index=carrito.value.findIndex(e => e.id===id);
  if(carrito.value[index].cantidad<=1){
    return;
  }else{
    carrito.value[index].cantidad--;
  }

}

</script>

<template>
  <headerItem
    :carrito="carrito"
    @agregar-cantidad="aumentarCarrito"
    @quitar-cantidad="quitarCantidad"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <!-- Trayendo primer componente -->
    <div class="row mt-5">
      <guitarraItem
        v-for="guitarra in guitarras"
        :guitarra="guitarra"
        @agregar-carrito="agregarCarrito"
      />
      <!-- FIN GUITARRA -->
    </div>
  </main>
  <footerItem />
</template>
