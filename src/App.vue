<template>
    <div class="app">
        <div class="title-container">
            <img src="./assets/img/pokemon-logo-transparent-free.webp" alt="Pokemon Logo" class="title-logo" />
            <h1>¿Quién es ese Pokemon?</h1>
        </div>
        <PokemonCounter :count="count" />
        <div class="pokemon-grid">
            <PokemonCard v-for="pokemon in pokemones" :key="pokemon.name" :pokemon="pokemon" @discovered="updateCounter" />
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from '@/components/PokemonCard.vue';
import PokemonCounter from '@/components/PokemonCounter.vue';

export default {
    name: 'App',
    components: {
        PokemonCard,
        PokemonCounter,
    },
    data() {
        return {
            pokemones: [],
            count: 0,
        };
    },
    async created() {
        try {
            const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=20');
            this.pokemones = response.data.results;
        } catch (error) {
            console.error('Error al cargar pokemones:', error);
        }
    },
    methods: {
        updateCounter() {
            this.count++;
        },
    },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Rubik+Dirt&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Fjalla+One&family=Knewave&display=swap');

body {
    margin: 0;
    padding: 0;
    background: linear-gradient(135deg, #2255b4 0%, #983a2a 100%);
    min-height: 100vh;
    font-family: 'Knewave', system-ui;
}

.app {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
}

.title-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin-bottom: 0.5rem;
}

.title-logo {
    height: 225px;
    width: auto;
    margin-bottom: -75px;
}

h1 {
    color: #ffffff;
    font-size: 3rem;
    font-family: 'Knewave', system-ui;
    font-weight: 100;
    text-shadow: 3px 3px 0 #000000, -3px -3px 0 #000000, 3px -3px 0 #000000, -3px 3px 0 #000000;
    letter-spacing: 2px;
    margin-bottom: 2rem;
}

h1::after {
    content: '';
    display: block;
    width: 300px;
    height: 4px;
    background: #ffd700;
    margin: 1rem auto;
    border-radius: 4px;
}

.pokemon-grid {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 1rem;
    padding: 1rem;
}

@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

@media (max-width: 768px) {
    .app {
        padding: 1rem;
    }

    h1 {
        font-size: 2rem;
    }

    .pokemon-grid {
        gap: 1rem;
    }
}
</style>
