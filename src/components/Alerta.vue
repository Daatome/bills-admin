<template>
    <div class="alerta">
        <slot></slot>
    </div>
</template>

<style  scoped>

    .alerta{
        padding: 1rem;
        font-weight: 900;
        font-size: 1.6rem;
        text-align: center;
        border-left: solid 0.5rem #ba0202;
        color: #ba0202 ;
        text-transform: uppercase;
        background-color: var(--blanco);


    }
</style>