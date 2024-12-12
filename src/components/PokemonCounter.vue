<template>
    <div class="counter">
        <h2>Pokemones descubiertos: {{ count }}/20</h2>
    </div>
</template>

<script>
export default {
    name: 'PokemonCounter',
    props: {
        count: {
            type: Number,
            required: true,
            default: 0,
        },
    },
};
</script>

<style scoped>
.counter {
    background-color: #f5f5f5;
    padding: 1rem;
    border-radius: 8px;
    margin: 1rem 0;
    text-align: center;
}

h2 {
    color: #2c3e50;
    font-size: 1.5rem;
    margin: 0;
}
</style>
