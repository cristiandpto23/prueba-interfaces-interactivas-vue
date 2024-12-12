<template>
    <div class="pokemon-card">
        <div class="pokemon-image" :class="{ discovered: isDiscovered }">
            <img :src="pokemonImage" :alt="pokemon.name" />
        </div>
        <div class="pokemon-form" v-if="!isDiscovered">
            <input type="text" v-model="userGuess" @keyup.enter="checkGuess" placeholder="¿Quién es este Pokemon?" />
            <button @click="checkGuess">Descubrir</button>
        </div>
        <div class="pokemon-name" v-else>
            <h3>{{ pokemon.name }}</h3>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'PokemonCard',
    data() {
        return {};
    },
    props: {
        pokemon: {
            type: Object,
            required: true,
        },
    },
    data() {
        return {
            userGuess: '',
            isDiscovered: false,
            pokemonImage: '',
        };
    },
    methods: {
        async checkGuess() {
            console.log('Pokemon:', this.pokemon.name);
            if (this.userGuess.toLowerCase() === this.pokemon.name.toLowerCase()) {
                this.isDiscovered = true;
                this.$emit('discovered');
            }
            this.userGuess = '';
        },
    },
    async created() {
        try {
            const response = await axios.get(this.pokemon.url);
            this.pokemonImage = response.data.sprites.front_default;
        } catch (error) {
            console.error('Error cargando imagen:', error);
        }
    },
};
</script>

<style scoped>
.pokemon-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 200px;
    margin: 0.5rem;
    background: linear-gradient(145deg, #e0f2ff 0%, #fff8f0 100%);
    border-radius: 12px;
    padding: 1.5rem;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
    border: 1px solid rgba(66, 165, 245, 0.1);
}

.pokemon-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px 12px rgba(66, 165, 245, 0.2);
    background: linear-gradient(145deg, #d4edff 0%, #fff2e6 100%);
}

.pokemon-image {
    width: 180px;
    height: 180px;
    margin-bottom: 1rem;
    border-radius: 50%;
    background-color: #f5f5f5;
    padding: 1rem;
}

.pokemon-image img {
    width: 100%;
    height: 100%;
    object-fit: contain;
    filter: grayscale(100%) blur(5px) brightness(1.2) contrast(0.8);
    transition: filter 0.5s ease;
}

.pokemon-image.discovered img {
    filter: grayscale(0%) blur(0) brightness(1) contrast(1);
}

.pokemon-form {
    width: 100%;
    margin-top: 1rem;
    padding: 0 1rem;
}

input {
    width: 100%;
    padding: 0.8rem;
    margin-bottom: 0.8rem;
    border: 2px solid #e0e0e0;
    border-radius: 8px;
    font-size: 1rem;
    box-sizing: border-box;
    transition: border-color 0.3s;
}

input:focus {
    outline: none;
    border-color: #4caf50;
}

input::placeholder {
    font-size: 0.9rem;
    color: #999;
}

button {
    width: 100%;
    padding: 0.8rem;
    background: #4caf50;
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    cursor: pointer;
    transition: background 0.3s;
}

button:hover {
    background: #45a049;
}

.pokemon-name {
    margin-top: 1rem;
    text-align: center;
}

.pokemon-name h3 {
    color: #2c3e50;
    text-transform: capitalize;
    font-size: 1.2rem;
    margin: 0;
}
@media (max-width: 768px) {
    .pokemon-card {
        flex: 0 1 160px;
    }
}
</style>
