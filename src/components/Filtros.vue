
<script setup>
defineProps(['filtro'])
defineEmits(['update:filtro'])
</script>

<template>
    <div class="filtros sombra contenedor">
        <div class="campo">
            <label for="filtro">Categoria Del Gasto:</label>
            <select id="filtro"
                :value="filtro"
                @input="$emit('update:filtro', $event.target.value)"
            >
                <option value="">--seleccione una opcion--</option>
                <option value="ahorro">Ahorro</option>
                <option value="comida">Comida</option>
                <option value="casa">Casa</option>
                <option value="gastos">Gastos  varios</option>
                <option value="ocio">Ocio</option>
                <option value="salud">Salud</option>
                <option value="suscripciones">Suscripciones</option>
            </select>
        </div>
    </div>
</template>

<style scoped>

.filtros{
    margin-top: 10rem;
    padding: 2rem;
}

.filtros .campo{
    display: flex;
    align-items: center;
    gap:2rem;
}
.filtros label{
    font-size: 3rem;
    font-weight: 900;
    color: var(--verde-oscuro);
}
.filtros select{
    flex: 1;
    border: none;
    border-radius: 1rem;
    background-color: var(--verde-claro);
    text-align: center;


}

</style>