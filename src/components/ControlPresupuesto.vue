
<script setup>
import {computed} from 'vue'
import  CircleProgress from 'vue3-circle-progress'
import "vue3-circle-progress/dist/circle-progress.css"

import { formatearCantidad } from '../helpers';

const props= defineProps({
    presupuesto:{
        type: Number,
        require: true
    },
    disponible:{
        type: Number,
        require: true
    },
    gastado:{
        type:Number,
        require:true
    }
})

const emit= defineEmits(['reset-app'])

const porcentaje= computed(()=>{
    return parseInt(((props.presupuesto-props.disponible)/props.presupuesto)*100)
})

</script>

<template>
    <div class="dos-columnas">
        <div class="contenedor-grafico">
            <p class="porcentaje">{{ porcentaje }} %</p>
            <CircleProgress
                :percent="porcentaje"
                :size="250"
                :border-width="20"
                :border-bg-width="20"
                fill-color="#22577A"
            />
        </div>
        <div class="contenedor-presupuesto">
            <button class="reset-app"
                @click="$emit('reset-app')"
            >
                Reset
            </button>
            <p>
                <span>Presupuesto:</span>
                {{ formatearCantidad(presupuesto) }}
            </p>
            <p>
                <span>Gastado:</span>
                {{ formatearCantidad(gastado) }}

            </p>
            <p>
                <span>Disponible:</span>
                {{ formatearCantidad(disponible) }}

            </p>
        </div>

    </div>
</template>

<style  scoped>

    .dos-columnas{
        display: flex;
        flex-direction: column;
        align-items: center;

    }

    .dos-columnas > :first-child{
        margin-bottom: 3rem;
    }
    @media (min-width:768px){
        .dos-columnas{
            flex-direction: row;
            gap: 4rem;
            align-items: center;
        }
        .dos-columnas > :first-child{
            margin-bottom: 0;
        }
        
    }

    .contenedor-presupuesto{

        width: 100%;
    }
    .contenedor-presupuesto p{
        font-size: 2.4rem;
        text-align: left;
        color: var(--verde-oscuro);
    }

    @media (min-width:  768px){
        .contenedor-presupuesto p{
            font-size: 2.4rem;
            text-align: center;
        }
    }
    .contenedor-presupuesto span{
        color: var(--azul);
    }

    .reset-app{
        background-color: #DB2777;
        border: none;
        padding: 1rem;
        border-radius: 30px;
        width: 100%;
        color: var(--blanco);
        font-weight: 800;
        text-transform: uppercase;
        cursor: pointer;

        transition: ease box-shadow 300ms;
        transition: ease transform 300ms;


    }

    .reset-app:hover{
        box-shadow: -33px 18px 28px 0px rgba(0,0,0,0.1);
        transform: translate(1rem, -1rem);

    }

    .contenedor-grafico{
        position: relative;
    }
    .porcentaje{
        position: absolute;
        margin: auto;
        top: calc(50% - 1.4rem);
        left: 0;
        right: 0;
        text-align: center;
        z-index: 100;
        font-size: 3rem;
        font-weight: 900;
        color: var(--verde-oscuro);
    }

</style>