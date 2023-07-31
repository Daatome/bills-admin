
<script setup>
import { ref } from 'vue';
import Alerta from './Alerta.vue'
import iconoCerrar from '../assets/img/cerrar.svg'

const error = ref('')

const props= defineProps({
    modal:{
        type: Object,
        required: true
    },
    nombre:{
        type: String,
        required:true
    },
    cantidad:{
        type: [String, Number],
        required:true
    },
    categoria:{
        type: String,
        required:true
    },
    disponible:{
        type:Number,
        required:true
    },
    id:{
        type:[String,null],
        required:true
    },
    
})

const oldCantidad= props.cantidad

const emit = defineEmits(['cerrar-modal','update:nombre','update:cantidad','update:categoria','guardar-gasto','eliminar-gasto'])


const agregarGasto=()=>{
    const {nombre, cantidad, categoria,disponible, id}= props// destructuring
    if([nombre,cantidad,categoria].includes('')){
        error.value='Todos los campos son requeridos'
        setTimeout(() => {
            error.value=''
        }, 3000);
        return
    }

    if(cantidad<=0){
        error.value='La cantidad debe ser mayor a cero'
        setTimeout(() => {
            error.value=''
        }, 3000);
        return
    }

    if(id){
        if(cantidad>disponible+oldCantidad){
            error.value='Has excedido tu prespuesto'
            setTimeout(() => {
                error.value=''
            }, 3000);
            return
        }
    }else{

        if(cantidad>disponible){
            error.value='Has excedido tu prespuesto'
            setTimeout(() => {
                error.value=''
            }, 3000);
            return
        }
    }

    emit('guardar-gasto')
}
</script>

<template>
    <div class="modal">
        <div class="cerrar-modal" @click="$emit('cerrar-modal')">
            <img :src="iconoCerrar" alt="">
        </div>

        <div class="contenedor contenedor-formulario"
        :class="[modal.animar ? 'animar' : 'cerrar'] "
        >
            <form 
                @submit.prevent="agregarGasto"
                class="nuevo-gasto"
            >
                <legend>{{ id ?   'Editar Gasto' : 'Añadir Gasto'}}</legend>

                <Alerta v-if="error">
                    {{ error }}
                </Alerta>
                <div class="campo">
                    <label for=":">Nombre Del Gasto:</label>
                    <input type="text" id="nombre" placeholder="Introduce el nombre del gasto"
                        :value="nombre"
                        @input="$emit('update:nombre',$event.target.value)"
                    >
                </div>
                <div class="campo">
                    <label for="Cantidad">Cantidad Gastada:</label>
                    <input type="number" id="Cantidad" placeholder="Introduce La cantidad de dinero gastada"
                        :value="cantidad"
                        @input="$emit('update:cantidad', +$event.target.value)"
                    >
                </div>
                <div class="campo">
                    <label for="nombre">Categoria Del Gasto:</label>
                    <select id="categoria"
                        :value="categoria"
                        @input="$emit('update:categoria',$event.target.value)"

                    >
                        <option value="">--seleccione una opcion--</option>
                        <option value="ahorro">Ahorro</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="gastos">Gastos varios</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="suscripciones">Suscripciones</option>
                    </select>
                </div>

                <input 
                type="submit"
                :value="[id ? 'Guardar Cambios' : 'Añadir Gasto']"
                >
            </form>
            <button type="button" class="btn-eliminar" 
                @click="$emit('eliminar-gasto')"
                v-if="id"
            >Eliminar Gasto</button>
        </div>
    </div>
</template>

<style  scoped>
    .modal{
        position: absolute;
        background-color: rgb(0 0 0/ 0.9);
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
    }
    .cerrar-modal{
        position: absolute;
        right: 3rem;
        top: 1rem;
    }
    .cerrar-modal img{
        width: 3rem;
        cursor: pointer;
    }

    .nuevo-gasto{
        margin: 10rem auto 0 auto;
        display: grid;
        gap: 2rem;
    }
    .nuevo-gasto legend{
        color: var(--blanco);
        font-size: 3rem;
        font-weight: 700;
        text-align: center;
        margin-bottom: 2rem;
    }

    .contenedor-formulario{
        transition-property: all;
        transition-duration: 300ms;
        transition-timing-function: ease-in;
        opacity: 0;
    }
    .contenedor-formulario.animar{
        opacity: 1;
    }
    .contenedor-formulario.cerrar{
        opacity: 0;
    }
    .campo{
        display: grid;
        gap: 2rem;
    }

    .nuevo-gasto input, .nuevo-gasto select{
        background-color: var(--verde-claro);
        border-radius: 10rem;
        padding: 1rem;
        border: none;
        font-size: 2.2rem;
    }
    .nuevo-gasto label{
        color: var(--blanco);
        font-size: 3rem;
        
    }
    .nuevo-gasto input[type="submit"]{
        background-color: var(--azul);
        color: var(--blanco);
        font-weight: 700;
        margin-top: 2rem;
        transition: transform ease 300ms;
        cursor: pointer;
    }
    .nuevo-gasto input[type="submit"]:hover{
        transform: translateY(-0.7rem);
    }

    .btn-eliminar{
        padding: 1rem;
        width: 100%;
        background-color: #ef4444;
        font-weight: 700;
        color: var(--blanco);
        margin-top: 10rem;
        cursor: pointer;
        border: none;
        border-radius: 10rem;
    }
</style>