
<script setup>
import {ref} from 'vue'
import Alerta from './Alerta.vue'

const presupuesto= ref(0)
const error= ref('')

const emit= defineEmits(['asignar-presupuesto'])

const definirPresupuesto= ()=>{
    if(presupuesto.value<=0 || presupuesto.value===''){
        error.value='Hubo un error, verifique la cantidad introducida'
        setTimeout(() => {
            error.value=''
        }, 3000)
        return
    }

    emit('asignar-presupuesto',presupuesto.value)
}



</script>

<template>
    <Alerta v-if="error">
        {{error}}
    </Alerta>
    <form class="presupuesto"
        @submit.prevent="definirPresupuesto"
    >
        <div class="campo">

            <label for="nuevo-presupuesto">Definir Presupuesto</label>
            
            <input 
            type="number"
            id="nuevo-presupuesto"
            class="nuevo-presupuesto"
            v-model="presupuesto"
            >

            <input type="submit">
        </div>

    </form>
</template>


<style scoped>
    .presupuesto{
        width: 100%;
    }
    .campo{
        display: grid;
        gap: 2rem;
    }

    .presupuesto label{
        font-size: 2.8rem;
        text-align: center;
        color: var(--azul);
    }
    .presupuesto input[type="number"]{
        background-color: var(--verde-claro);
        border-radius: 1rem;
        padding: 1rem;
        border: none;
        font-size: 2.2rem;
        text-align: center;

    }
    .presupuesto input[type="submit"]{
        background-color: var(--azul);
        border: none;
        border-radius: 1rem;
        padding: 1rem;
        text-align: center;
        font-size: 2rem;
        margin-top: 2rem;
        color: var(--blanco);
        font-weight: 900;
        width: 100%;
        cursor: pointer;
        transition: background-color ease 300ms;


    }
    .presupuesto input[type="submit"]:hover{
        
        background-color: var(--verde-oscuro);
    }

</style>