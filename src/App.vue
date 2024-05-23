<script setup>
import { ref, onMounted, watch } from 'vue'
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
const guitarra= ref([])
watch(carrito,()=>{
  guardarLocalStorage()
},{
deep:true
})
onMounted(() => {
  guitarras.value = db
  guitarra.value=db[3]
  const carritoStorage= localStorage.getItem("carrito")
  if(carritoStorage){
    carrito.value=JSON.parse(carritoStorage)
  }
  // State.guitarras=db;
})

const guardarLocalStorage=()=>{
localStorage.setItem("carrito", JSON.stringify(carrito.value))

}
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
const eliminarGuitarra=(id) =>{
  carrito.value=carrito.value.filter((estado)=>{return estado.id !== id})
}

const vaciarCarrito=()=>{
  carrito.value=[];
}

</script>

<template>
  <headerItem
    :carrito="carrito"
    :guitarra="guitarra"
    @agregar-cantidad="aumentarCarrito"
    @quitar-cantidad="quitarCantidad"
    @agregar-carrito-promocion="agregarCarrito"
    @vaciar-carrito="vaciarCarrito"
    @eliminar-guitarra="eliminarGuitarra"
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
