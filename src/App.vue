<script setup>
import { ref, reactive, watch , computed, onMounted} from 'vue';
import Presupuesto from './components/Prespuesto.vue'
import Filtros from './components/Filtros.vue'
import Modal from './components/Modal.vue'
import ControlPresupuesto from './components/ControlPresupuesto.vue'
import Gasto from './components/Gasto.vue'
import iconoNuevoGasto from './assets/img/nuevo-gasto.svg'

import {generarId} from './helpers'

const modal= reactive({
  animar:false,
  mostrar:false
})

const gasto=reactive({
  nombre:'',
  cantidad:'',
  categoria:'',
  id:null,
  fecha: Date.now()

})

const gastos= ref([])

const presupuesto= ref(0)
const disponible= ref(0)
const gastado= ref(0)
const filtro= ref('')



watch(gastos,()=>{
  const totalGastado= gastos.value.reduce((total, gasto)=> gasto.cantidad+total,0)
  gastado.value=totalGastado
  disponible.value=presupuesto.value-gastado.value

  localStorage.setItem('gastos', JSON.stringify(gastos.value))



},{
  deep:true
})

watch(presupuesto,()=>{
  localStorage.setItem('presupuesto', presupuesto.value)
})

onMounted(()=>{
  const presupuestoStorage= localStorage.getItem('presupuesto')
  const gastosStorage= localStorage.getItem('gastos')
  if(presupuestoStorage){
    asignarPresupuesto( Number(presupuestoStorage))
  }
  if(gastosStorage){
    gastos.value=JSON.parse(gastosStorage)
  }
  
})


const asignarPresupuesto= (cantidad)=>{
  presupuesto.value= cantidad
  disponible.value=presupuesto.value

}

const mostrarModal= ()=>{
  modal.mostrar=true
  
  setTimeout(() => {
    modal.animar=true
    
  }, 300);

}
const cerrarModal= ()=>{
  modal.animar=false
  
  setTimeout(() => {
    modal.mostrar=false
    
  }, 300);
  reinciarStateGasto()
}

const guardarGasto= ()=>{
  if(gasto.id){
    const indexGasto= gastos.value.findIndex((gastoBusca)=> gasto.id===gastoBusca.id)
    gastos.value[indexGasto]={...gasto}

    
  }else{
    gastos.value.push({
      ...gasto,
      id:generarId()
    
    })
  }
  cerrarModal()
  reinciarStateGasto()
}

const seleccionarGasto= id=>{
  const gastoSeleccionado= gastos.value.filter((gasto)=> gasto.id===id)[0]

  gasto.cantidad= gastoSeleccionado.cantidad
  gasto.categoria= gastoSeleccionado.categoria
  gasto.nombre=gastoSeleccionado.nombre
  gasto.id=gastoSeleccionado.id
  gasto.fecha=gastoSeleccionado.fecha
  mostrarModal()
}
const reinciarStateGasto=()=>{
  Object.assign(gasto,{
    nombre:'',
    cantidad:'',
    categoria:'',
    id:null,
    fecha: Date.now()
  })
}

const eliminarGasto=()=>{
  gastos.value= gastos.value.filter(gastoState=> gastoState.id!==gasto.id)
  cerrarModal()
}

const gastosFiltrados= computed(()=>{
  if(filtro.value){
    return gastos.value.filter(gasto=> gasto.categoria===filtro.value)
  }
  return gastos.value
})

const resetApp=()=>{
  if(confirm('Deseas reiniciar todos los datos?')){

    presupuesto.value=0
    gastos.value=[]
    reinciarStateGasto()
  }
}




</script>

<template>
  <div :class="{fijar : modal.mostrar}">
    <header>
      <h1 class="header-h1">Planificador de Gastos</h1>

      <div class="contenedor contenedor-header sombra">
        <Presupuesto
        v-if="presupuesto===0"
        @asignar-presupuesto="asignarPresupuesto"
        />
        <ControlPresupuesto v-else
          :presupuesto="presupuesto"
          :disponible="disponible"
          :gastado="gastado"
          @reset-app="resetApp"
        />
      </div>
    </header>
    <main v-if="presupuesto">
      <Filtros
        v-model:filtro="filtro"
      />

      <div class="listado-gastos contenedor">
        <h2>{{ gastosFiltrados.length>0 ? 'Gastos' : 'No tienes gastos registrados' }}</h2>

        <Gasto
          v-for="gasto in gastosFiltrados"
          @seleccionar-gasto="seleccionarGasto"
          :gasto="gasto"
        />

      </div>



      <div class="crear-gasto"
        @click="mostrarModal"
      >
        <img :src="iconoNuevoGasto" alt="iconoNuevoGasto">
      </div>

      <Modal
        v-if="modal.mostrar"
        @cerrar-modal="cerrarModal"
        @guardar-gasto="guardarGasto"
        @eliminar-gasto="eliminarGasto"
        :modal="modal"
        :disponible="disponible"
        :id="gasto.id"
        v-model:nombre="gasto.nombre"
        v-model:cantidad="gasto.cantidad"
        v-model:categoria="gasto.categoria"


      />
      
    </main>
    
  </div>
</template>

<style >
  :root{
    --azul: #22577A;
    --blanco: #FFF;
    --verde-claro: #C7F9CC;
    --verde: #80ED99;
    --verde-oscuro:#57CC99;
    --negro: #38A3A5;

  }
  html{
    font-size: 62.5%;
    box-sizing: border-box;
  }
  *,
  *:before,
  *:after{
    box-sizing: inherit;
  }
  body{
    font-size: 1.6rem;
    font-family: "Lato", sans-serif;
    background-color: var(--verde-claro);
  }

  h1{
    font-size: 4rem;

  }
  h2{
    font-size: 3rem;
  }
  header{
    background-color: var(--azul);

  }
  .header-h1{
    padding: 3rem 0;
    margin: 0;
    text-align: center;
    color: var(--blanco);

  }
  .contenedor{
    width: 90%;
    max-width: 80rem;
    margin: 0 auto;
    border-radius: 3rem;

  }
  .contenedor-header{
    background-color: var(--blanco);
    margin-top: -5rem;
    transform: translateY(5rem);
    padding: 5rem;
  }
  .sombra{
    box-shadow: -23px 21px 8px -3px rgba(0,0,0,0.1);
    background-color: var(--blanco);
    border-radius: 1rem;

  }

  .crear-gasto{
    position: fixed;
    bottom: 5rem;
    right: 5rem;
    
  }
  .crear-gasto img{
    width: 5rem;
    cursor: pointer;
    transition: ease width 300ms;
  }

  .crear-gasto img:hover{
    width: 6rem;
  }
  
  .listado-gastos{
    margin-top: 10rem;
  }
  .listado-gastos h2{
    font-weight: 900;
    color: var(--verde-oscuro);

  }
  .fijar{
    overflow: hidden;
    height: 100vh;
  }
  
</style>
